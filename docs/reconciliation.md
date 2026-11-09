# Reconciliation Architecture

## Canonical Record

The Orchestrator creates one immutable Saldo payment ID before any external side effect. That ID is the aggregate key for the user-visible workflow; every execution attempt and provider event has its own child ID so retries do not overwrite history.

Required fields, nullable only when the rail does not apply:

| Field | Purpose |
| --- | --- |
| `saldo_payment_id` | Canonical internal identifier |
| `payment_type` | Send, receive, bill payment, cash-in, cash-out, refund |
| `idempotency_key` | User-command deduplication scoped to user and operation |
| `saldo_user_id`, `wallet_address` | Pilot attribution and wallet binding |
| `stellar_network`, `asset_code`, `asset_issuer` | Exact USDC identity |
| `usdc_amount` | Decimal amount in canonical precision |
| `stellar_transaction_hash`, `operation_id`, `ledger_sequence` | Public onchain evidence |
| `alfredpay_quote_id`, `alfredpay_transaction_id` | Quote and conversion reference |
| `fx_rate`, `provider_fee`, `mxn_amount` | Settlement economics |
| `spei_reference`, `biller_payment_reference` | Offchain payout evidence |
| `moneygram_transaction_id`, `moneygram_reference` | Cash-rail evidence |
| `compliance_status`, `compliance_decision_id` | Eligibility and screening result |
| `workflow_status`, `stellar_status`, `settlement_status` | Separate aggregate and rail states |
| `refund_of_payment_id`, `exception_reference` | Compensation and operations linkage |
| `created_at`, `authorized_at`, `submitted_at`, `ledger_closed_at`, `settled_at`, `updated_at` | Event chronology |
| `provider_event_id`, `provider_event_time`, `observed_at` | Deduplication and ordering |

No PII is written to public Stellar data. Evidence exports redact user identity while retaining a stable pilot-user pseudonym.

## State Ownership And Transitions

| Transition | Initiator | Preconditions | Evidence |
| --- | --- | --- | --- |
| `CREATED → ELIGIBILITY_PASSED` | Orchestrator | Auth, wallet binding, KYC/AML and limits pass | Decision IDs |
| `→ QUOTED` | Java backend event | Valid bill/partner quote not expired | Quote ID, rate, fee, expiry |
| `→ AWAITING_AUTHORIZATION` | Orchestrator | Deterministic transaction built | Envelope digest and approval summary |
| `→ AUTHORIZED` | Stellar service event | Configured signer approved exact envelope | Authorization/signing evidence |
| `→ STELLAR_SUBMITTED` | Stellar service event | Signed envelope revalidated | Hash and submission response |
| `→ STELLAR_CONFIRMED` | Stellar indexer event | Successful ledger inclusion and expected operation | Hash, ledger, operation, amount/asset/destination |
| `→ SETTLEMENT_PENDING` | Orchestrator command | Onchain and compliance preconditions met | Partner idempotency key |
| `→ COMPLETED` | Orchestrator | All required onchain/offchain legs terminal-success | Provider and biller references |
| `→ FAILED` | Owning service event | Failure occurred before irreversible success | Structured reason |
| `→ EXCEPTION_REVIEW` | Orchestrator/monitor | Contradiction, timeout, or partial completion | Alert and case reference |
| `→ REFUND_PENDING/REFUNDED` | Orchestrator | Approved compensating record exists/confirms | Linked refund payment ID/hash |

Services cannot directly update aggregate status. They append normalized events; the Orchestrator applies a versioned state transition under optimistic locking.

## Idempotency And Duplicate Handling

- Mobile sends a UUID idempotency key; the Orchestrator returns the original result for the same user, operation, and canonical payload.
- A reused key with a different payload is rejected and alerted.
- Every downstream command uses a deterministic key derived from payment ID and operation type, never a fresh key on timeout retry.
- Provider callbacks are unique by provider/event ID where available, otherwise by a documented canonical payload digest plus transaction ID/status/version.
- Stellar submission is keyed by envelope hash; after an ambiguous timeout, query by transaction hash/account sequence before resubmitting.
- Database uniqueness constraints protect wallet/environment, transaction hash, partner transaction ID, and provider event identity.

## Out-Of-Order Events

Events retain provider time and observed time. The reducer accepts only transitions allowed by the current versioned state. A stale event is stored for audit but cannot regress a terminal state. A contradictory later event (for example `completed` followed by `failed`) creates `EXCEPTION_REVIEW`; provider semantics determine whether it is a reversal rather than a regression.

## Timeouts And Retries

- Each external operation has `next_check_at`, attempt count, backoff class, and SLA deadline.
- Read/status calls may retry with bounded exponential backoff and jitter.
- Side-effecting calls retry only with proven provider idempotency or after status lookup shows no execution.
- Expired Stellar transactions require reconstruction and fresh user authorization.
- SLA expiry changes the aggregate to pending-review or exception; it never fabricates failure or success.

## Daily And Intraday Reconciliation

1. Export internal records for the reconciliation window.
2. Re-index Stellar transactions from a durable cursor and independently verify successful USDC operations.
3. Import AlfredPay, SPEI, biller, and MoneyGram statements/statuses.
4. Join by exact external IDs, then compare wallet, asset, gross/net amount, fee, rate, memo, timestamp, and state.
5. Classify breaks: missing internal, missing external, amount mismatch, duplicate, stale pending, invalid transition, unlinked refund, or unknown wallet.
6. Assign a case owner and SLA; preserve evidence of investigation and resolution.
7. Require dual approval for manual compensation or state override.

Graciela D. owns operations reconciliation; Fernando M. owns integration defects; Fatima G. owns compliance breaks. The Orchestrator team owns reducer/idempotency defects, and the Stellar service owner owns indexing/submission defects.

## Core Invariants

- One payment ID has at most one active onchain debit attempt at a time.
- A completed bill payment has one confirmed eligible Stellar USDC leg and all required successful offchain references.
- A failed pre-submission payment has no successful Stellar debit.
- A refund never mutates or reuses the original transaction record.
- The same Stellar operation is counted once, even if it appears in multiple internal workflow rows.
- No provider success can bypass KYC/AML or required onchain confirmation.
