# Integrations

## Crossmint Embedded Stellar Wallets

Crossmint is the wallet provider for Android and iOS. The integration covers Stellar wallet provisioning, device signing, recovery, transaction authorization, and audit evidence.

Integration requirements:

- Bind an immutable Saldo user ID to the authenticated Crossmint identity, wallet locator, and Stellar address.
- Provision or retrieve exactly one intended pilot wallet per user/environment unless product rules explicitly allow more.
- Confirm Stellar account activation, base reserve sponsorship, USDC trustline creation, and fee sponsorship behavior.
- Construct transactions in the Stellar service and authorize them only through the configured Crossmint signer path.
- Verify the signed envelope matches the payment record before submission.
- Configure and test recovery on a new Android and iOS device.
- Export signer/recovery configuration evidence for custody and security review.

The target configuration is a device-held operational signer plus user-controlled recovery, with no Saldo server signer on user wallets. The deployed authority must be confirmed before production. See [wallet security](wallet-security.md).

Official references: [Crossmint wallets overview](https://docs.crossmint.com/wallets/overview), [supported chains](https://docs.crossmint.com/introduction/supported-chains), [custody models](https://docs.crossmint.com/wallets/concepts/custody-models), [recovery methods](https://docs.crossmint.com/wallets/concepts/recovery), [Kotlin quickstart](https://docs.crossmint.com/wallets/quickstarts/kotlin), and [Swift quickstart](https://docs.crossmint.com/wallets/quickstarts/swift).

## Stellar And USDC

- Environment is explicit on every request; testnet and mainnet records cannot share identifiers.
- Asset is a configured `(network, code, issuer)` tuple, not code `USDC` alone.
- Mainnet and testnet issuer values are pinned from authoritative sources during environment setup and covered by tests; no user-supplied issuer is accepted.
- Account activation, trustline/sponsorship, transaction time bounds, sequence handling, fees, memos, result codes, hashes, ledger sequences, and operation IDs are recorded.
- Indexing uses Stellar network data as onchain truth and can replay from a durable cursor.

## AlfredPay

**Reported status: sandbox access. Production onboarding is outstanding unless independent evidence is added.**

The Java Core Backend owns the integration:

1. Request a quote containing source/destination assets, amounts, FX rate, fees, expiry, and quote ID.
2. Present exact user economics before authorization; reject an expired or changed quote.
3. Initiate USDC-to-MXN conversion and SPEI payout with the Saldo payment ID as idempotency/correlation data where supported.
4. Store AlfredPay transaction ID, quote ID, SPEI reference, provider status, timestamps, and raw-payload digest.
5. Verify callback authenticity and event IDs; poll as fallback or when callbacks are not supported.
6. Normalize pending, completed, rejected, failed, reversed, cancelled, and refunded states without discarding provider detail.
7. Reconcile AlfredPay amount/fees/rate and SPEI/biller outcome against the confirmed Stellar leg.

Pending confirmation: production agreement and credentials, sandbox/production parity, rate limits, quote validity, webhook authentication, idempotency semantics, status lifecycle, refunds, reversals, supported corridors, limits, and operational SLAs.

## MoneyGram Ramps

MoneyGram Ramps provides the cash-in/cash-out experience. Its public documentation describes two Stellar integration paths:

- A widget/API-key path recommended for new Stellar pilots. The server creates a short-lived session; the mobile surface embeds the widget; a sign callback authorizes the Stellar USDC transfer; status endpoints and callbacks/polling provide transaction state.
- A legacy anchor path for existing partners using SEP-10 authentication and SEP-24 interactive deposit/withdraw, with provider transaction IDs and status polling.

Saldo will select and certify one path. Both require authenticated server-side setup, allowlisted interactive origins, provider transaction ID capture, exact wallet/amount/memo verification, user signing for cash-out, inbound payment detection for cash-in, terminal-state handling, cancellation/refund behavior, and reconciliation.

For the legacy path only:

- Discover current endpoints and signing keys from MoneyGram's `stellar.toml`; do not hardcode documentation examples.
- Verify the SEP-10 challenge network, domain, signing key, nonce, timeout, and transaction content before signing.
- Start SEP-24 deposit/withdraw only with the resulting short-lived token.
- Store the SEP-24 ID and poll authoritative transaction status through completion, refund, expiry, or error.

Production requires credentials, allowlisting, supported corridors/locations, KYB and legal completion, certification, production domains/keys, compliance authorization, and incident contacts.

Official references: [MoneyGram Stellar/USDC guide](https://xramps.moneygram.com/ops/developer/guides/stellar) and [MoneyGram widget/API guide](https://xramps.moneygram.com/ops/developer/guides/web-stellar).

## Billers, Banks, And Compliance Providers

These remain behind the Java Core Backend. Each adapter must define authentication, idempotency, request signing, callback verification, timeout, retry, terminal states, reversals/refunds, reconciliation fields, rate limits, and data-retention constraints. Their existence is part of the reported current backend; exact providers and production status require private-repository and contract evidence.
