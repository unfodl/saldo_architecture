# Wallet Authorization, Signing, And Recovery

## Evidence Summary

### Verified From Public Crossmint Documentation

- Crossmint supports Stellar wallets and native Kotlin and Swift wallet SDKs.
- Client-side wallets can use a device signer; Crossmint describes iOS Secure Enclave and Android Keystore storage for device keys.
- Recovery methods can add/remove operational signers and can restore signing access.
- Crossmint supports user-controlled and server-controlled signer/recovery options; custody classification depends on the deployed combination.
- Public Kotlin and Swift quickstarts exist, but their shown transactions use EVM test networks rather than a Saldo Stellar configuration.

These facts describe platform capability, not Saldo's tenant configuration.

### Proposed Saldo Configuration

| Authority | Proposed holder | Permitted action |
| --- | --- | --- |
| Operational signer | User device | Authorize user-wallet transactions after Saldo approval display |
| Recovery method | User-controlled verified factor | Enroll replacement device signer and remove lost signer |
| Transaction construction | Stellar settlement service | Build but not unilaterally authorize user transaction |
| Submission | Stellar settlement service | Submit a correctly authorized envelope and monitor result |
| Backend wallet APIs | Saldo services | Read wallet metadata/balance and create signing requests within least-privilege scopes |
| Saldo server signer | None on user wallets | Not permitted in proposed design |

Under this proposal, Saldo's backend cannot move user funds without authorization from a configured user-controlled signer or recovery method. That statement is a design requirement, **not yet a verified deployment fact**.

## Pending Technical Confirmation

Before using any custody label or launching mainnet, obtain and test:

1. Exact Crossmint wallet type, chain label, SDK/package versions, and production availability for Stellar on Kotlin and Swift.
2. The full operational signer and recovery-method inventory created by each platform.
3. Whether email/phone OTP, passkey, device signer, tenant admin, Crossmint support, policy engine, or any server signer can directly authorize a transaction or signer change.
4. Whether Saldo API keys with transaction/signature scopes can cause a user-wallet debit without an active user factor.
5. Whether Crossmint or Saldo can replace recovery methods, bypass recovery, export keys, freeze a wallet, or move funds under support procedures.
6. Exact transaction-construction/signing handoff for Stellar and whether the SDK signs XDR locally, approves a Crossmint transaction object, or uses another mechanism.
7. Account activation, reserve/trustline sponsorship, fee payment, sequence-number ownership, and transaction expiration behavior.
8. Audit APIs/events for signer addition/removal, recovery, authorization, and transaction requests.

Until answered, the wallet is described as a **Crossmint embedded wallet with custody classification pending configuration review**. MPC or embedded-wallet terminology alone is not evidence of noncustody.

## Provisioning And Identity Binding

- Saldo creates an immutable internal user ID after application authentication.
- The mobile client authenticates to Crossmint using the approved identity flow. Production should use Saldo's identity provider JWT if supported and approved; Crossmint's public docs recommend third-party authentication for production.
- The Orchestrator stores `(saldo_user_id, environment, crossmint_user_id/locator, crossmint_wallet_id/locator, stellar_address, status, created_at)` under a uniqueness constraint.
- Email and phone are verified attributes, never wallet ownership keys by themselves.
- Wallet creation uses a request idempotency key. On timeout, lookup precedes retry to avoid duplicate wallets.
- The Stellar service verifies network, account activation, USDC trustline, and sponsorship before marking the wallet ready.

## Transaction Authorization

1. The Orchestrator validates user/session, KYC eligibility, risk rules, limits, payment state, and idempotency.
2. The Stellar service builds a time-bounded transaction from server-side values and records an envelope digest.
3. Mobile displays network, source, destination, asset code and issuer, amount, fee, memo purpose, and expiry.
4. User explicitly approves; Crossmint applies the configured signer.
5. The Stellar service decodes the returned envelope and compares every security-relevant field with the approved record.
6. A mismatch is rejected and alerted; the service never "fixes" and submits a differently signed transaction.
7. Submission response, hash, ledger, result code, and indexed operations are stored under the same payment ID.

## Backend Permissions

- Use separate client and server API keys by environment and service.
- Grant only required scopes; signing/transaction scopes require security approval and compensating controls.
- Restrict production keys by origin/IP or workload identity where supported.
- Keep secrets in managed secret storage; prohibit logs, crash reports, mobile bundles, source control, and support tickets.
- Rotate keys on schedule and immediately after suspected exposure; test revocation.
- Alert on signer/recovery changes, privileged API use, unusual transaction requests, and production-key use from unexpected workloads.

## Recovery Threat Controls

- Require recovery-factor proof and re-authentication to the Saldo account.
- Notify the old device and verified channels on recovery start/completion when available.
- Apply risk-based cooling periods and reduced limits after recovery.
- Detect SIM-swap/email-takeover indicators through the identity/risk provider where available.
- Prevent support staff from changing wallet bindings without dual approval and an immutable case record.
- Verify the recovered wallet address matches the existing binding; do not create a replacement silently.
- Exercise lost-device, stolen-session, lost-recovery-factor, factor-change, and compromised-support scenarios.

Crossmint recovery is the proposed recovery infrastructure. SEP-30 is not assumed or included unless a separate, reviewed design is later approved.

## Android And iOS Differences

| Area | Android | iOS | Acceptance requirement |
| --- | --- | --- | --- |
| Native SDK | Kotlin | Swift | Exact Stellar support and versions recorded |
| Device key | Android Keystore per Crossmint description | Secure Enclave per Crossmint description | Hardware-backed/non-exportability verified on supported devices |
| Approval API | Public quickstart shows explicit approval in some examples | Public quickstart describes approval inside send flow | Stellar-specific behavior tested; equivalent user intent captured |
| App links | Android App Links/custom-tab return | Universal Links/ASWebAuthenticationSession return | Only allowlisted MoneyGram/Crossmint origins and signed state accepted |
| Device integrity | Play Integrity/risk signal where available | App Attest/DeviceCheck/risk signal where available | Signals inform risk policy; no silent security downgrade |
| Secure local data | Encrypted storage backed by Keystore | Keychain with appropriate accessibility class | Tokens cleared on logout/revocation; no signing secret logged |

## Production Security Gate

The mainnet gate requires a signed signer/recovery authority matrix, two-platform recovery demonstration, backend no-unilateral-movement test, credential review, transaction-tampering test, recovery abuse test, incident runbook, and legal/compliance custody review.
