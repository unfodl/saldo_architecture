# Components

## Android App

Consumer mobile app for wallet onboarding, USDC balance, send, receive, transaction history, and bill payment.

Responsibilities:

- Call Orchestrator APIs.
- Display wallet and payment state.
- Trigger wallet creation through the backend.
- Show Stellar transaction status and payment history.

## iOS App

Consumer mobile app with the same wallet and bill payment capabilities as Android.

Responsibilities:

- Call Orchestrator APIs.
- Display wallet and payment state.
- Support USDC-funded bill pay flows.
- Show transaction history and settlement status.

## Web App

Operations and agent-facing interface.

Responsibilities:

- Display wallet, payment, and settlement status.
- Support operational review and reconciliation.
- Support agent workflows for cash position, limits, repayment status, and risk alerts.
- Use Stellar Wallets Kit for browser wallet connection/signing where agent signing is required.
- Support web-only inbound USDC funding from other chains through Circle CCTP.

## Orchestrator API

TypeScript service used by all apps.

Responsibilities:

- Maintain the canonical transaction ledger.
- Coordinate wallet, bill payment, SPEI, and cash-in/cash-out workflows.
- Store Stellar transaction hashes and partner transaction references.
- Track Circle CCTP transfer references for web wallet deposits.
- Normalize transaction states across systems.
- Expose transaction history and status to apps.

## Stellar Settlement Service

TypeScript service for Stellar network operations.

Responsibilities:

- Create Stellar accounts and USDC trustlines.
- Build and submit Stellar transactions.
- Monitor submitted transaction status.
- Return hashes, ledger status, and failure details.
- Isolate Stellar protocol changes from application workflows.

## Java Core Backend

Backend service for non-Stellar financial integrations.

Responsibilities:

- Integrate with Mexican billers and service providers.
- Integrate with banks and payment providers.
- Integrate with alfredpay for USDC-to-MXN settlement and SPEI.
- Integrate with MoneyGram or provider-ready cash rail adapters.
- Apply compliance checks and transaction controls.
- Return external settlement state to the Orchestrator.
