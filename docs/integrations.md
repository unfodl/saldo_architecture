# Integrations

## SCF Integration Track Building Blocks

Saldo's Integration Track work focuses on production building blocks plus one staging/testnet-only DeFindex integration.

## alfredpay

alfredpay is used for USDC-to-MXN settlement, SPEI transfers, and local banking rail connectivity across Latin America.

Integration scope:

- USDC-to-MXN settlement workflow.
- SPEI transfer initiation and tracking.
- Quote, FX rate, fee, and settlement status handling.
- Reconciliation records across Stellar transaction, alfredpay transaction, MXN payout, and biller payment.

## MoneyGram

MoneyGram is used for cash-in/cash-out through physical locations, subject to partner approval.

Integration scope:

- Provider-ready cash-in/cash-out adapter.
- External transaction reference capture.
- Status tracking and reconciliation.
- Connection to MoneyGram first if approval is complete.

If approval is not complete, the deliverable remains a provider-ready cash rail adapter that can connect to MoneyGram or another approved provider.

## Stellar Wallets Kit

Stellar Wallets Kit is used in the web interface for browser wallet connection and signing.

Integration scope:

- Connect Stellar-compatible wallets from the agent web interface.
- Request signatures for agent or operations workflows.
- Pass signed payloads to backend services for execution and reconciliation.

Stellar Wallets Kit is not used as the main consumer mobile wallet. Consumer mobile wallets use an embedded non-custodial MPC provider.

## Circle CCTP

Circle CCTP is used for web-only inbound USDC funding from supported external chains into Stellar USDC.

Integration scope:

- Add cross-chain USDC receive flow to the web wallet interface.
- Capture CCTP transfer references.
- Monitor resulting Stellar USDC receipt.
- Link CCTP reference, Stellar transaction hash, wallet address, and deposit status in the Orchestrator ledger.

Circle CCTP is part of the production web app plan. It is not part of the initial consumer mobile wallet flow. Mobile wallets continue to use the embedded MPC provider and Stellar USDC account model.

## DeFindex

DeFindex is used only for staging/testnet exploration from the web app.

Integration scope:

- Add testnet vault deposit, withdraw, and balance flows to the web app.
- Connect testnet signing through Stellar Wallets Kit.
- Track DeFindex vault references and related Stellar testnet transaction hashes.
- Validate dashboard and reconciliation patterns for vault-style activity.

Production boundary:

- DeFindex is not part of the production launch scope.
- No production user funds are routed to DeFindex.
- DeFindex is not exposed in the consumer mobile wallet flow.
- DeFindex activity is tracked separately from production bill pay, SPEI, cash-in/cash-out, and CCTP activity.

## MPC Wallet Provider

The MPC wallet provider is implementation infrastructure for the consumer mobile app.

Integration scope:

- Embedded non-custodial wallet provisioning.
- Recovery and new-device flows.
- Wallet-user mapping through the Orchestrator.

The MPC provider is intentionally not named as a primary SCF Integration Track building block.

## Stellar Network

Stellar is the USDC settlement network for wallet and payment flows.

Integration scope:

- Stellar account creation.
- USDC trustline setup.
- Transaction building, submission, and monitoring.
- Transaction hash storage for reconciliation.
- Testnet pilot followed by mainnet rollout.
