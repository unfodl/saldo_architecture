# STRIDE Threat Model

## Assets And Trust Boundaries

Protected assets include user authorization, signer/recovery factors, wallet bindings, USDC, provider credentials, KYC data, payment state, quotes, partner references, audit logs, and acceptance evidence. Trust boundaries exist between mobile/Crossmint, mobile/Saldo API, Saldo services, Saldo/Stellar, and Saldo/each partner.

## Threats And Controls

| STRIDE | Threat | Preventive controls | Detection | Owner |
| --- | --- | --- | --- | --- |
| Spoofing | Stolen Saldo session provisions or uses victim wallet | Short-lived tokens, issuer/audience checks, device/risk signals, immutable identity binding | New device, impossible travel, wallet-binding change | Abdul B. / Fatima G. |
| Spoofing | Fake Crossmint, MoneyGram, or provider endpoint | TLS, allowlisted origins, signed state/deep links, endpoint config by environment | Certificate/origin mismatch, callback auth failure | Abdul B. / Fernando M. |
| Spoofing | Forged provider callback | HMAC/signature/mTLS as supported, timestamp/nonce validation, status polling | Invalid signature, unknown event/transaction ID | Fernando M. |
| Tampering | Amount, destination, asset, memo, fee, or network changed before signing | Server construction, approval summary, time bounds, signed-envelope comparison, issuer allowlist | Envelope digest mismatch; unexpected indexed operation | Abdul B. / Stellar service owner |
| Tampering | Quote changed or replayed after expiry | Quote ID, exact economics, expiry gate, fresh approval | Expired quote execution or settlement mismatch | Fernando M. |
| Repudiation | User or operator denies approval/action | Immutable request, authorization, device/session, digest, and audit timestamps | Missing evidence or audit-chain gap | Marco M. / Operations |
| Information disclosure | Secrets or PII leak through logs/mobile/public ledger | Secret manager, redaction, no PII in memos, least privilege, encrypted storage | Secret scanning, DLP/log alerts, unusual key access | Fernando M. / Fatima G. |
| Denial of service | RPC/provider outage or callback flood | Rate limits, queues, circuit breakers, bounded retry, multi-endpoint read strategy | Error/latency/backlog/SLA alerts | Service owner |
| Elevation of privilege | Saldo backend/server signer can move user funds | Proposed no-server-signer model, scoped keys, dual approval, custody review | Signer inventory changes, privileged signing call | Abdul B. / Marco M. |
| Elevation of privilege | Recovery-factor takeover replaces signer | Strong factor, risk checks, cooling period, notifications, support dual control | Recovery plus rapid transfer; signer removal | Abdul B. / Fatima G. |
| Replay | Duplicate mobile command, signed transaction, or callback executes twice | Idempotency keys, sequence/time bounds, event uniqueness, state preconditions | Reused key/digest/hash/external ID | Orchestrator owner |
| Financial integrity | Stellar succeeds but payout fails | Saga states, no blind retry, reserve/compensation policy | Confirmed debit with stale/failed offchain leg | Graciela D. / Fernando M. |
| Financial integrity | Offchain payout succeeds twice | Deterministic provider key, unique external ID, dual-source reconciliation | Duplicate amount/reference/beneficiary window | Graciela D. / Fernando M. |
| Compliance | Ineligible user transacts or screening is bypassed | Eligibility gate before authorization/settlement, fail closed, case review | Missing/expired decision; high-risk pattern | Fatima G. |

## Wallet Recovery Abuse Cases

- Attacker controls email or phone but not the existing device.
- Attacker controls a Saldo session but not the Crossmint recovery factor.
- Support operator changes a wallet binding or recovery method.
- Old device remains authorized after new-device recovery.
- Recovery immediately precedes a high-value or new-destination transfer.
- Crossmint or tenant administrative recovery has broader authority than assumed.

Each case requires a pre-production test and retained result. Failure of the backend-no-unilateral-movement test blocks a noncustodial claim and triggers legal/compliance reassessment.

## Onchain Versus Offchain Detection

Onchain data can reveal signer changes represented onchain, unexpected assets/destinations, duplicate or unusual transfers, amount mismatch, missing/late transactions, and activity after a wallet is frozen internally. It cannot prove who controlled an email/SMS factor, whether KYC passed, whether a quote was authentic, whether cash was handed over, whether SPEI/biller settlement completed, or whether a backend credential was stolen. Those require Crossmint, application, identity, and partner telemetry.

The operational signal mapping and response procedures are in [onchain monitoring](onchain-monitoring.md).
