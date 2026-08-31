# Roadmap

## Phase 1: Wallet and Ledger Foundation

Environment: testnet, then production.

Scope:

- Embedded MPC mobile wallet provisioning.
- Stellar account creation and USDC trustline setup.
- SEP-30-style wallet recovery and new-device recovery testing.
- Transaction history and wallet-user mapping.
- Orchestrator ledger records for wallet and payment activity.

Recovery is implemented and tested early on testnet before bill pay, cash rails, or production rollout depend on the wallet.

## Phase 2: Bill Pay and Settlement

Environment: testnet, then production.

Scope:

- USDC-funded bill payment.
- alfredpay SPEI and USDC-to-MXN settlement.
- MoneyGram SEP-10 authentication and SEP-24 interactive deposit/withdraw flows.
- MoneyGram or provider-ready cash-in/cash-out adapter.
- Operations dashboard and reconciliation exports.

## Phase 3: Web Wallet Integrations

Environment: production web app.

Scope:

- Stellar Wallets Kit for browser wallet connection and signing.
- Circle CCTP for inbound USDC funding from supported external chains into Stellar USDC.
- Web ledger records for CCTP transfer references and Stellar transaction hashes.

Circle CCTP starts in the web app and is part of the production web wallet plan. It is not exposed in the initial consumer mobile wallet flow.

## Phase 4: DeFindex Staging/Testnet Exploration

Environment: staging/testnet only.

Scope:

- DeFindex vault deposit testing from the web app.
- DeFindex vault withdraw testing from the web app.
- DeFindex vault balance lookup from the web app.
- Stellar Wallets Kit signing for testnet vault operations.
- Staging dashboard records for vault references and Stellar testnet transaction hashes.

Production boundary:

- DeFindex is not part of the production launch scope.
- No production user funds are routed to DeFindex.
- DeFindex is not exposed in the consumer mobile wallet flow.
- DeFindex activity is tracked separately from production bill pay, SPEI, cash-in/cash-out, and Circle CCTP activity.
