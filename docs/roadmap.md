# SCF #46 Roadmap And Budget

The roadmap is mobile-only. Partner production approvals are dependencies, not implied deliverables already secured. Budget amounts are fixed.

## Tranche 1 — Mobile Stellar Wallet MVP

**Target:** January 31, 2027. **Amount:** $31,000.

| Work item | Budget | Reviewer evidence |
| --- | ---: | --- |
| Crossmint wallet integration | $10,000 | Android/iOS provisioning, identity mapping, signer/recovery configuration record, authorization and recovery tests |
| Stellar backend integration | $9,000 | Account/USDC setup, deterministic construction, submission/confirmation/indexing, hash and ledger evidence |
| Android and iOS wallet UI | $8,000 | Balance, receive, send, history, failures, approval details on both native apps |
| Testnet deployment and documentation | $4,000 | Reproducible builds/test evidence, architecture, runbooks, open-decision register |
| **Tranche total** | **$31,000** | W-01 through W-06 pass on both platforms |

Exit gate: exact Crossmint Stellar signer/recovery behavior is demonstrated; custody status is updated from evidence; testnet USDC flows reconcile end to end.

## Tranche 2 — Payments, Partners, Reconciliation, Security

**Target:** March 31, 2027. **Amount:** $44,000.

| Work item | Budget | Reviewer evidence |
| --- | ---: | --- |
| USDC-funded bill payments | $10,000 | One payment ID links approved quote, onchain debit, biller outcome, failures and refund path |
| AlfredPay integration | $15,000 | Sandbox quote, fees/rate, conversion, SPEI status, callback/polling and exception evidence |
| MoneyGram Ramps integration | $12,000 | Selected MoneyGram path, cash-in/out, authentication/session, status, cancel/refund and references |
| Stellar indexing and reconciliation | $2,000 | Durable cursor, replay, deduplication, daily break report |
| Onchain monitoring plan and threat model | $5,000 | STRIDE tests, alert rules, owners, runbooks and incident exercise |
| **Tranche total** | **$44,000** | B-01 through O-01 pass in applicable sandbox/test environments |

Exit gate: partial settlements and duplicate/out-of-order events are safely handled; production dependencies and unresolved partner gaps are explicit.

## Tranche 3 — Mainnet Mobile Production Pilot

**Target:** May 31, 2027. **Amount:** $47,900.

| Work item | Budget | Reviewer evidence |
| --- | ---: | --- |
| Mainnet wallet rollout | $9,000 | Approved production signer/recovery configuration, staged rollout, wallet and USDC evidence |
| Production bill pay and settlement | $13,000 | Successful mainnet user-facing USDC and production settlement/biller references |
| MoneyGram production integration | $8,000 | Production certification/authorization evidence and reconciled cash transactions |
| Compliance and transaction monitoring | $11,000 | Active eligibility/screening, alerts, cases, incident and audit records |
| Monitoring, documentation, and open-source release | $6,900 | Public docs, redacted metric manifest, verification method, runbooks and known limitations |
| **Tranche total** | **$47,900** | Mainnet pilot and metric thresholds independently reproducible |

Exit gate: at least $5,000 eligible mainnet USDC volume, 100 successful eligible Stellar transactions, and 25 unique active pilot wallets under the methodology in [acceptance tests](acceptance-tests.md).

## Total

| Tranche | Amount |
| --- | ---: |
| Tranche 1 | $31,000 |
| Tranche 2 | $44,000 |
| Tranche 3 | $47,900 |
| **Total requested** | **$122,900** |

## Dependency Gates

- **Crossmint:** Stellar production availability, exact Android/iOS signer and recovery model, credentials/scopes, audit evidence, support escalation.
- **AlfredPay:** production onboarding, limits/corridor, credential and callback model, SLA, refund/reversal support.
- **MoneyGram:** selected product path, credentials, allowlisting, corridors, KYB/legal, certification, production keys/domain and go-live approval.
- **Development team:** private repository access, current deployment diagrams, Orchestrator location, schema/API evidence, existing compliance and incident procedures.

A missed partner gate is reported as a dependency blocker; it does not authorize substituting an unnamed adapter and claiming the named deliverable complete.
