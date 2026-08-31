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

- Stellar SEP-10 authentication with the MoneyGram anchor.
- SEP-24 interactive deposit and withdrawal for cash-in/cash-out.
- Provider-ready cash-in/cash-out adapter.
- External transaction reference capture.
- Status tracking and reconciliation.
- MoneyGram webview/browser handoff and return-status handling.
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

## SEP-30 Account Recovery

SEP-30 account recovery is a supporting Stellar standard for the wallet recovery workstream. It is not treated as a separate Integration Track building block.

Integration scope:

- Validate wallet recovery and new-device recovery assumptions on testnet.
- Track recovery events in the Orchestrator ledger.
- Verify recovered wallet access and Stellar account state before enabling wallet activity.
- Include recovery outcomes in testnet QA and security review.

Production boundary:

- SEP-30 recovery work starts on testnet before production launch.
- Production rollout happens only after successful testnet recovery validation.

## SEP-10 And SEP-24 For MoneyGram

SEP-10 and SEP-24 are supporting Stellar standards used by the MoneyGram cash-in/cash-out integration. They are not treated as separate Integration Track building blocks.

Integration scope:

- Use SEP-10 Stellar Web Auth before MoneyGram SEP-24 deposit or withdrawal requests.
- Use SEP-24 interactive deposit for cash-in and interactive withdrawal for cash-out.
- Open the MoneyGram interactive flow in a webview or browser surface.
- Store SEP-24 transaction IDs, MoneyGram reference numbers, Stellar transaction hashes, and final transaction status in the Orchestrator ledger.
- Support status polling, user-transfer states, refund/cancel states, and operational reconciliation.

Testnet/sandbox scope:

- Complete MoneyGram allowlisting and sandbox/testnet setup before production.
- Validate SEP-10 challenge signing, SEP-24 deposit, SEP-24 withdrawal, and refund/cancel handling in sandbox.
- Move to production only after certification, KYB/legal completion, and production domain/key setup.

## Stellar Network

Stellar is the USDC settlement network for wallet and payment flows.

Integration scope:

- Stellar account creation.
- USDC trustline setup.
- Transaction building, submission, and monitoring.
- Transaction hash storage for reconciliation.
- Testnet pilot followed by mainnet rollout.
