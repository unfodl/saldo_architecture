# Transaction Flows

## Wallet Provisioning

1. User starts onboarding in the mobile app.
2. App calls the Orchestrator.
3. Orchestrator creates the user wallet record.
4. Embedded MPC wallet provider provisions the non-custodial wallet.
5. Stellar settlement service creates or verifies the Stellar account and USDC trustline.
6. Orchestrator stores the wallet address and setup status.
7. App shows wallet as ready.

## USDC-Funded Bill Payment

1. User selects a biller and confirms payment from USDC balance.
2. App sends payment request to the Orchestrator.
3. Orchestrator creates a Saldo payment record.
4. Stellar settlement service submits the required Stellar transaction and returns the transaction hash.
5. Orchestrator calls the Java Core Backend to settle the bill payment.
6. Core Backend routes settlement through biller, bank, or alfredpay rails.
7. Core Backend returns partner references and final or pending settlement status.
8. Orchestrator updates the ledger and app-visible transaction history.

## SPEI / MXN Settlement With alfredpay

1. User or workflow requests MXN settlement or SPEI transfer.
2. Orchestrator records the request and validates transaction state.
3. Stellar settlement service confirms the related USDC movement.
4. Java Core Backend calls alfredpay for USDC-to-MXN settlement or SPEI transfer.
5. alfredpay returns quote, transaction reference, and status.
6. Core Backend reports status to the Orchestrator.
7. Orchestrator stores the alfredpay reference and updates the transaction ledger.

## Cash-In / Cash-Out

1. User starts a cash-in or cash-out flow.
2. App calls the Orchestrator.
3. Orchestrator creates the cash transaction record.
4. Java Core Backend routes the request through MoneyGram if approved, or through a provider-ready cash rail adapter.
5. Stellar settlement service handles any required USDC movement.
6. Core Backend returns external provider reference and status.
7. Orchestrator reconciles Stellar and cash rail state.

## Agent Web Signing

1. Agent opens the web interface.
2. Web app uses Stellar Wallets Kit to connect a Stellar-compatible wallet.
3. Agent reviews an operation requiring signature.
4. Web app requests signature through Stellar Wallets Kit.
5. Signed payload is submitted to the Orchestrator.
6. Orchestrator routes Stellar operations to the Stellar settlement service and operational updates to the Java Core Backend.
