# Acceptance Tests And Mainnet Metrics

## Evidence Package

Each tranche produces a versioned evidence bundle containing test case/version, environment, build identifiers, redacted Saldo payment IDs, public Stellar transaction links/hashes where applicable, ledger sequences, screenshots or API outputs, provider references, expected/actual results, timestamps, tester, and known deviations. Secrets, OTPs, full identity data, and customer PII are excluded.

## Reviewer-Executable Tests

| ID | Test | Procedure | Pass criteria |
| --- | --- | --- | --- |
| W-01 | Provision wallet | On fresh Android and iOS test users, authenticate, create/retrieve wallet, and inspect exported mapping/network/account state | One wallet mapping per user/environment; valid Stellar address; required account/USDC readiness confirmed; no duplicate on retry |
| W-02 | Balance and receive | Send configured testnet USDC to each wallet and wait for indexer | Exact asset/issuer and amount appear once; hash and ledger link to history |
| W-03 | Send and authorization | Submit a USDC send, inspect approval fields, approve, and verify ledger | User sees exact network/asset/amount/destination/fee; one successful operation; internal record matches ledger |
| W-04 | Tamper rejection | Alter destination/amount/issuer between construction and returned signed envelope in test harness | Submission is blocked, security alert opens, no ledger operation occurs |
| W-05 | New-device recovery | Remove access to original device, recover on a new device, and perform a small send | Existing wallet address is recovered; new signer evidence recorded; old-signer policy applied; test send succeeds |
| W-06 | Backend authority | Attempt transfer with Saldo server credentials but without configured user authorization | Attempt fails under proposed model; signer inventory proves no Saldo unilateral path. If it succeeds, custody status and architecture must be revised before acceptance |
| B-01 | Bill payment | Pay a sandbox/test bill from USDC through partner/biller completion | Payment ID links quote, confirmed Stellar hash/ledger, partner ID, SPEI/biller reference, amounts/fees, and terminal state |
| B-02 | Idempotency | Repeat identical mobile request and provider callback; simulate initial timeout | One onchain debit and one payout; original result returned; duplicates retained but not re-applied |
| B-03 | Failed Stellar leg | Force insufficient balance, bad trustline, expiry, and submission error | No offchain settlement starts; structured failure is shown and auditable |
| B-04 | Failed offchain leg | Confirm Stellar debit then force AlfredPay/SPEI/biller failure | State becomes exception; no blind duplicate retry; approved retry/refund produces linked evidence |
| A-01 | AlfredPay tracking | Obtain quote, execute sandbox conversion/SPEI, and process callback or poll | Quote/rate/fee/expiry and all partner references reconcile; production is not claimed |
| M-01 | MoneyGram cash-in | Complete selected sandbox flow and deposit cash | Provider ID and cash reference link to one confirmed inbound USDC operation and final status |
| M-02 | MoneyGram cash-out | Complete selected sandbox flow, approve exact USDC transfer, and collect/confirm cash status | Provider ID, memo/reference, one confirmed debit, and terminal provider state reconcile |
| M-03 | MoneyGram cancel/refund | Cancel/expire or trigger supported refund state | State follows documented lifecycle; refund, if any, has separate hash/payment ID; no duplicate debit |
| R-01 | Out-of-order events | Deliver pending/completed/duplicate/late-failure events out of order | Terminal state does not regress; contradiction opens exception; every event remains auditable |
| S-01 | Screening gate | Use eligible, rejected, and unavailable-screening scenarios | Eligible proceeds; rejected/unavailable fails closed for protected transitions; decision ID retained |
| O-01 | Reconciliation replay | Rebuild a day from Stellar cursor plus partner extracts | All records match or produce classified, owned breaks; totals reproduce independently |

For partner tests, reviewers receive sandbox access or a recorded run plus redacted raw request/status evidence when partner terms prevent credential sharing.

## Mainnet Metric Definitions

Measurement window: the approved SCF #46 pilot start and end timestamps in UTC, recorded in the release evidence. Only Stellar mainnet is eligible.

An **eligible operation** is a successful Stellar payment operation that:

1. Uses the configured mainnet asset tuple `USDC:<approved Circle issuer>`.
2. Is included in a successful transaction and confirmed in a closed ledger.
3. Is linked to exactly one SCF #46 pilot Saldo payment ID and at least one enrolled pilot user wallet.
4. Represents the user-facing economic movement: mobile send, mobile receive from an external party, bill-payment debit, MoneyGram cash-in receipt, MoneyGram cash-out debit, or user refund.
5. Is not an internal treasury, fee-funding, reserve, sponsorship, sweep, conversion intermediate, partner settlement intermediate, test, failed, duplicate, or reversed-by-data-correction record.

One Stellar operation ID is counted once. A transfer between two enrolled Saldo pilot wallets contributes one transaction and its amount once, not once per sender and receiver. A multi-leg bill payment counts only the designated user-facing USDC operation, not downstream internal movements. A refund is a distinct user-facing operation and may count as transaction/volume only if the published SCF interpretation permits it; the primary report shows refund-inclusive and refund-exclusive totals separately.

### Cumulative USDC Volume

`sum(eligible_operation.amount)` using decimal USDC units, grouped by unique Stellar operation ID. Required threshold: **at least $5,000 USDC**. No FX conversion is applied because the measured asset is USDC; the report shows exact decimal amount and does not infer a market USD price.

### Successful Transactions

`count(distinct eligible_operation.transaction_hash)`. Required threshold: **at least 100**. A hash counts only when the transaction succeeded and contains at least one eligible user-facing USDC payment operation. Multiple eligible operations in one transaction contribute their non-duplicated amounts to volume but count as one transaction; one eligible operation cannot cause the same hash to count more than once.

### Unique Active Wallets

`count(distinct pilot_wallet_address)` where the enrolled address is source or destination of at least one eligible operation during the window. Required threshold: **at least 25**. Wallet aliases, replaced bindings, and duplicate user records are resolved by the immutable wallet mapping; one address counts once. Internal Saldo/partner wallets do not count.

## Reproducible Report

Publish a CSV or JSON manifest with:

- Redacted pilot user pseudonym and wallet address.
- Redacted Saldo payment ID.
- Payment type and inclusion/exclusion reason.
- Stellar transaction hash, operation ID, ledger sequence, close time, source, destination, asset code, issuer, and amount.
- Applicable redacted AlfredPay, SPEI, biller, MoneyGram, refund, and exception references.

A verification script or documented SQL/query performs:

```text
eligible = rows
  where network = "mainnet"
  and transaction_successful = true
  and operation_successful = true
  and asset = configured_mainnet_usdc
  and pilot_payment_id is not null
  and user_facing_leg = true
  and exclusion_reason is null
deduplicated = unique eligible by operation_id
volume = sum(deduplicated.amount)
transactions = count(distinct deduplicated.transaction_hash)
wallets = count(distinct enrolled pilot address appearing in deduplicated source/destination)
```

Reviewers can look up every hash on a public Stellar explorer or Horizon/RPC-derived dataset, verify ledger success and operation fields, rerun the manifest calculation, and sample-join redacted internal/partner references. The report includes excluded rows and reasons so reviewers can test for cherry-picking or double counting.

## Tranche Acceptance

- **Tranche 1:** W-01 through W-06 pass on both platforms on testnet; documentation and testnet build evidence published.
- **Tranche 2:** B-01 through O-01 pass in available sandboxes/test environments; threat model, alerts, reconciliation replay, and outstanding production dependencies published.
- **Tranche 3:** production gates close; mainnet pilot passes applicable tests; public manifest demonstrates all three thresholds; monitoring and incident ownership are active.
