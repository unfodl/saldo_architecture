# Onchain And Operational Monitoring

## Monitoring Pipeline

The Stellar service streams/indexes relevant accounts and operations into immutable normalized events. The Orchestrator joins those events to payment records. Monitoring rules join onchain facts with Crossmint, application, compliance, AlfredPay, MoneyGram, SPEI, and biller telemetry. Alerts link to a runbook and case; secrets and PII are excluded.

## Signal Matrix

| Threat / condition | Observable signal | Source | Alert condition | Response owner | Initial remediation |
| --- | --- | --- | --- | --- | --- |
| Transaction tampering | Asset, issuer, destination, amount, memo, or envelope digest mismatch | Signing pipeline + Stellar | Any mismatch | Stellar service owner | Block submission; revoke session; investigate request chain |
| Unauthorized transfer | Confirmed outgoing USDC with no authorized payment ID | Stellar + Orchestrator | Any event | Abdul B. + Fatima G. | Freeze workflow, assess signer/recovery compromise, contact Crossmint |
| Duplicate execution | Same payment/external ID linked to multiple debits or payouts | All ledgers | Any duplicate | Graciela D. | Stop downstream action; open reconciliation case |
| Replay attempt | Reused idempotency key with changed payload or repeated callback nonce | API/provider telemetry | Any | Orchestrator/Core owner | Reject, preserve evidence, rotate credential if callback attack suspected |
| Recovery takeover | Signer/recovery change followed by rapid/new-destination spend | Crossmint + Stellar | Policy threshold or any high-risk combination | Abdul B. + Fatima G. | Hold limits/settlement, challenge user, remove compromised signer |
| Unexpected signer change | Signer inventory differs from approved baseline | Crossmint/onchain state | Any unplanned change | Abdul B. | Disable affected wallet operations and reconcile authority |
| Failed Stellar transaction | Submission result unsuccessful/expired or not found by SLA | Stellar service | Immediate terminal error or SLA breach | Stellar service owner | Do not start settlement; rebuild only with fresh authorization if required |
| Stuck confirmation | Submitted hash not confirmed by threshold | Stellar | Confirmation SLA breach | Stellar service owner | Query independent endpoint; classify dropped/failed/pending |
| Partial settlement | Stellar confirmed; AlfredPay/SPEI/biller failed or stale | Joined state | Partner SLA breach or terminal failure | Graciela D. + Fernando M. | Stop retries; verify idempotency; choose retry/refund/case |
| Payout without debit | Offchain completed without expected confirmed onchain precondition | Joined state | Any | Graciela D. | Stop related payouts; investigate state/control bypass |
| MoneyGram mismatch | Provider amount/wallet/memo/reference differs from indexed operation | MoneyGram + Stellar | Any | Fernando M. + Graciela D. | Hold completion, query authoritative status, contact partner |
| Screening bypass | Protected transition with absent/expired/failed decision | Orchestrator + compliance | Any | Fatima G. | Hold transaction, open compliance incident |
| Volume anomaly | Wallet/user/destination velocity or amount exceeds policy | Stellar + app | Risk-rule threshold | Fatima G. | Review/hold offchain settlement; file case as required |
| Indexer lag | Latest indexed ledger trails network | Stellar | Configured ledger/time threshold | Stellar service owner | Fail closed for completion; recover cursor/backfill |
| Credential abuse | Production key used from unexpected workload/location | Cloud/provider audit | Any | Fernando M. | Revoke/rotate key, isolate workload, assess transactions |

## Severity And Response

- **SEV-1:** unauthorized transfer, systemic double payment, signing/recovery compromise, or material compliance bypass. Page incident lead immediately; stop affected execution; preserve evidence; notify partners and users/regulators per approved plan.
- **SEV-2:** partial settlement, reconciliation mismatch, provider outage, or indexer lag affecting completion. Assign within 15 minutes during pilot support hours; halt unsafe transitions.
- **SEV-3:** single failed transaction, stale pending within funds-safe bounds, or non-security data-quality issue. Queue to owning service with SLA.

Marco M. coordinates incident acceptance and external communication. Technical containment belongs to the affected service owner; Graciela D. owns settlement cases; Fatima G. owns compliance disposition.

## Runbook Template

Every alert runbook records: trigger and severity, dashboards/queries, affected payment IDs, funds-at-risk estimate, containment command, partner escalation, evidence to retain, recovery validation, user communication, compliance/legal decision, post-incident reconciliation, and closure approval.

## Retention And Audit

Retain normalized events, alert transitions, acknowledgements, manual actions, and evidence references under the approved financial/compliance retention schedule. Access is role-based and audited. Raw provider payload retention and PII fields require provider-specific data mapping and legal approval.
