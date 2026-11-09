# Repository And Responsibility Inventory

## Repository Verification

Verified on November 9, 2026 using public GitHub access and the checked-out architecture history. No authenticated access to the listed private repositories was available; an inaccessible private URL is not evidence that a repository does not exist.

| Repository | Observed visibility/access | SCF #46 role |
| --- | --- | --- |
| [unfodl/saldo_architecture](https://github.com/unfodl/saldo_architecture) | Public; fetched successfully | Public architecture, security, reconciliation, roadmap, and acceptance evidence |
| [unfodl/saldo_web](https://github.com/unfodl/saldo_web) | Public; page and source inventory visible | Historical web prototype; explicitly excluded from funded scope and metrics |
| [unfodl/saldo](https://github.com/unfodl/saldo) | Access-restricted from anonymous/current credentials | Reported Java Core Backend for billers, banks, partners, and compliance |
| [unfodl/saldowallet_android](https://github.com/unfodl/saldowallet_android) | Access-restricted from anonymous/current credentials | Reported native Android/Kotlin application |
| [unfodl/saldo-stellar](https://github.com/unfodl/saldo-stellar) | Access-restricted from anonymous/current credentials | Reported TypeScript Stellar settlement service |
| [unfodl/saldo-stellar-ios](https://github.com/unfodl/saldo-stellar-ios) | Access-restricted from anonymous/current credentials | Reported native iOS/Swift application |

The repository containing the TypeScript Orchestrator implementation is **Pending technical confirmation**.

## Public Development Evidence

The fetched `saldo_architecture` history contains real commits including:

- `21bf15c` — “Clarify MPC wallet provider scope” (author recorded as `marco`, 2026-08-31).
- `27e5bef` — “Add signup flow and MoneyGram SEP standards” (author recorded as `marco`, 2026-08-31).
- `2a02c30` — “Add mobile bill pay flow image” (author recorded as `marco`, 2026-08-30).
- `898f214` — “Add repository map” (author recorded as `marco`, 2026-08-18).
- `f612e1f` — “Update README.md” (author recorded as `Marco`, 2026-08-16).

This proves activity in the public architecture repository by those Git author identities. It does not independently prove that the author is Marco M., prove work in private repositories, or establish contribution percentages. The public `saldo_web` repository is visible and describes a Crossmint/Stellar prototype, but its implementation is historical evidence outside SCF #46.

## Assigned Responsibility Matrix

These are project assignments supplied for SCF #46, not verified commit authorship.

| Person | Assigned responsibility | Acceptance ownership |
| --- | --- | --- |
| Fernando M. | Java backend, AlfredPay, MoneyGram, billers, reconciliation | Partner requests/statuses, idempotency, settlement/refund evidence, integration incidents |
| Abdul B. | Native mobile apps, Crossmint, signing and recovery | Android/iOS wallet flows, authority matrix, recovery and tamper tests |
| Marco M. | Product coordination, delivery acceptance, integration oversight | Scope control, tranche evidence, dependency decisions, incident coordination |
| Graciela D. | Operations and settlement reconciliation | Daily/intraday breaks, exceptions, refunds, operational sign-off |
| Fatima G. | Compliance and transaction monitoring | Eligibility/screening rules, alerts, cases, compliance sign-off |

## Evidence Needed From Private Repositories

- Default branch, latest commit, release tags, contributor history, and ownership/CODEOWNERS.
- Current service boundaries, deployment configuration, schemas, API contracts, and tests.
- Existing Crossmint code/configuration and signer/recovery evidence.
- Current AlfredPay and MoneyGram clients, credentials environment, status models, and sandbox evidence.
- Existing compliance, audit, reconciliation, monitoring, and incident controls.

Private evidence may be shared with reviewers under appropriate access without making code or secrets public.
