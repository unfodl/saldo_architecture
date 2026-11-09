# SCF #46 Scope And Evidence Status

## Objective

SCF #46 funds a focused integration of Stellar USDC into Saldo's existing native mobile and bill-payment systems. It does not fund a new web-wallet, agent-credit, cross-chain, card, lending, or yield product.

## Status Matrix

| Capability | Status | SCF #46 treatment |
| --- | --- | --- |
| Android application | Existing repository; private access not available during this review | Extend in Kotlin for Crossmint and Stellar USDC. |
| iOS application | Existing repository; private access not available during this review | Extend in Swift for Crossmint and Stellar USDC. |
| Java Core Backend | Existing repository; private access not available during this review | Preserve biller, bank, partner, and compliance boundary. |
| TypeScript Stellar service | Existing repository; private access not available during this review | Extend construction, submission, monitoring, indexing, and reconciliation. |
| Orchestrator API | Existing architectural boundary; implementation repository/location pending confirmation | Own canonical workflow and payment state. |
| `saldo_web` | Public historical prototype | Excluded from funded delivery and acceptance metrics. |
| Crossmint | Selected wallet provider; exact Saldo configuration unverified | Proposed mobile integration; signer, recovery, custody, and Stellar SDK behavior pending confirmation. |
| AlfredPay | Sandbox access reported | Build and verify sandbox flow; production onboarding is a dependency. |
| MoneyGram Ramps | Access reported | Exact product path, credentials, corridors, certification, and production approval pending confirmation. |

## In Scope

- Mobile wallet provisioning and Saldo-user-to-Crossmint-wallet mapping.
- Stellar account activation and official network-specific USDC asset configuration.
- USDC send, receive, balance, and indexed transaction history.
- User authorization, transaction signing, new-device recovery, and security testing.
- USDC-funded bill payment through the existing Java and TypeScript services.
- AlfredPay quotes, fees, FX, USDC-to-MXN conversion, SPEI payout, status, refund, and reconciliation.
- MoneyGram Ramps cash-in/cash-out, interactive flow, authentication, status, cancellation/refund, and reconciliation.
- KYC/AML eligibility, sanctions/risk screening, audit logs, incident response, and operational monitoring.
- Public, redacted evidence for the three mainnet acceptance metrics.

## Explicitly Out Of Scope

- Agent liquidity, credit limits, lending, and repayment engines.
- Agent-facing web dashboards and browser wallet signing.
- Stellar Wallets Kit.
- Circle CCTP and any cross-chain funding.
- DeFindex or other yield/vault integrations.
- Visa prepaid cards.
- Any provider substitution presented as completion of the named MoneyGram integration.

Out-of-scope systems may continue to exist, but their code, transactions, costs, and metrics cannot be counted toward SCF #46 acceptance.

## Evidence Rules

1. An architectural intention is labeled **Proposed**, not implemented.
2. Partner access does not imply certification or production approval.
3. A wallet custody label follows the deployed signer and recovery authority, not the words "MPC" or "embedded wallet."
4. Private repository links that cannot be accessed are labeled access-restricted, not nonexistent.
5. Assigned responsibilities do not prove commit authorship.
6. Mainnet metrics require public transaction hashes plus redacted internal records tied to Saldo pilot payment IDs.

## Decisions Required Before Production

- Confirm the exact Crossmint Stellar wallet type, SDK versions, signer set, recovery set, transaction-approval UX, and backend permissions for Android and iOS.
- Confirm who can add/remove signers, invoke recovery, or authorize a transfer without an active user session.
- Confirm Stellar account activation and reserve sponsorship responsibility.
- Pin mainnet and testnet USDC asset code/issuer pairs from an authoritative allowlist; reject unconfigured assets.
- Confirm AlfredPay production onboarding, credentials, limits, webhook/polling contract, refund support, and sandbox-to-production differences.
- Select the current MoneyGram integration path (widget/API or legacy SEP-10/SEP-24), then confirm credentials, corridors, KYB/legal status, certification, callbacks/polling, and production authorization.
- Confirm the Orchestrator's implementation repository and current persistence/event model.
- Confirm compliance providers, screening timing, case-management ownership, retention periods, and incident contacts.
