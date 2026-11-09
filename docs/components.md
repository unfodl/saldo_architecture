# Components And Responsibilities

## Native Android App

**Existing boundary; SCF-funded extension.** Kotlin client for onboarding, Crossmint wallet access, USDC balance/receive/send/history, bill payment, MoneyGram interactive flows, recovery, and status display.

- Authenticates the user and presents transaction details for approval.
- Uses the supported Crossmint Android/Kotlin integration and Android Keystore-backed signer if confirmed.
- Never embeds server credentials or constructs authoritative settlement amounts.
- Rejects environment, network, asset, amount, or destination mismatches returned for approval.
- Shows pending, failed, refunded, and exception states without presenting offchain success before confirmation.

## Native iOS App

**Existing boundary; SCF-funded extension.** Swift client with the same product capability as Android.

- Uses the supported Crossmint Swift integration and Secure Enclave-backed signer if confirmed.
- Implements iOS-specific secure storage, app lifecycle, deep-link, and recovery tests.
- Requires equivalent approval semantics and evidence to Android despite SDK UX differences.

Crossmint's public quickstarts show different approval behavior for current Kotlin and Swift examples, but those examples use EVM test networks. Exact Stellar behavior is **Pending technical confirmation** and must be proven on both platforms.

## Crossmint

**Selected external dependency; configuration unverified.** Provides wallet provisioning and configurable signer/recovery infrastructure.

- Returns a stable wallet locator and Stellar address for the authenticated user.
- Applies the configured signing and recovery authority.
- Exposes wallet, transaction, signer, and recovery evidence needed for support and audit.
- Must not receive authority beyond the approved signer model.

The proposed user-wallet model is device operational signer plus user-controlled recovery. A Saldo server signer is not proposed. Any server, tenant-admin, support, or recovery capability that can authorize fund movement changes the custody and threat model and must be documented before production.

## Orchestrator API

**Existing architectural boundary; implementation evidence pending.** TypeScript API and canonical owner of user workflow and aggregate payment state.

- Creates one UUID/ULID Saldo payment ID before side effects.
- Enforces authentication, authorization, KYC eligibility, limits, idempotency, and state-machine preconditions.
- Stores wallet, Stellar, AlfredPay, SPEI, biller, MoneyGram, compliance, refund, and exception references.
- Issues commands to the Stellar service and Java backend.
- Deduplicates and orders normalized events by provider event ID/version and observed time.
- Exposes mobile history and support-safe status.

## Stellar Settlement Service

**Existing private repository; SCF-funded extension.** TypeScript service for Stellar-specific execution.

- Activates or verifies Stellar accounts and required USDC trustlines/sponsorship.
- Constructs deterministic transactions using an allowlisted network and USDC issuer.
- Requests signing according to the confirmed Crossmint authority model; it does not assume signing authority.
- Verifies signed envelope contents against the payment record before submission.
- Submits once, records hash and result, confirms ledger inclusion, and indexes relevant operations.
- Detects failed, expired, missing, duplicate, unexpected-asset, unexpected-destination, and amount-mismatch transactions.

## Java Core Backend

**Existing private repository; SCF-funded extension.** Integration owner for offchain financial and compliance systems.

- Obtains and expires AlfredPay quotes; records FX rate and fees.
- Initiates and tracks USDC-to-MXN conversion, SPEI payout, and biller execution.
- Integrates the selected MoneyGram Ramps path and stores all provider references.
- Runs or invokes KYC/AML eligibility, sanctions, and transaction-risk screening before protected transitions.
- Verifies provider callbacks, polls when required, normalizes status, and emits immutable events.
- Starts refunds/compensation only from an authorized Orchestrator command.

## Operations And Compliance

**Existing team functions; SCF-funded controls.** Review alerts, exceptions, reconciliation breaks, disputes, and incidents.

- Graciela D. owns operational settlement reconciliation and exception handling.
- Fatima G. owns compliance and transaction-monitoring review.
- Every manual action requires reason, actor, timestamp, prior state, resulting state, and case reference.
- Manual tooling must not expose signing secrets or allow unlogged payment-state edits.

## External Dependencies

| Dependency | Current evidence | Production dependency |
| --- | --- | --- |
| Stellar | Public network and APIs | Reliable RPC/Horizon/indexing access and pinned USDC identifiers |
| Crossmint | Public Stellar/wallet documentation; exact Saldo tenant configuration unavailable | Stellar SDK enablement, signer/recovery confirmation, production keys and support process |
| AlfredPay | Sandbox access reported | Production onboarding, credentials, limits, callback/polling and refund contract |
| MoneyGram Ramps | Access reported; public integration paths documented | Product-path selection, credentials, corridors, KYB/legal, certification and go-live approval |
| Billers/banks | Existing Java boundary reported | Contract, credentials, idempotency, status and refund behavior per provider |
| Compliance providers | Existing boundary reported | Named providers, rules, SLAs, retention and incident process |
