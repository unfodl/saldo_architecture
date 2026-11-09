# Publishing And Review

This repository is the public architecture source for Saldo's SCF #46 mobile-only Stellar USDC integration.

## Pull Request Checklist

- Confirm the branch is `scf46-mobile-only-architecture` and targets `main`.
- Render both Mermaid files and all inline Mermaid blocks.
- Run the relative-link and terminology checks described below.
- Recalculate all tranche subtotals and the fixed $122,900 total.
- Obtain technical review from the mobile, Stellar, Java, operations, and compliance owners.
- Obtain written confirmation or retain an explicit pending label for every Crossmint, AlfredPay, and MoneyGram dependency.
- Verify that no secret, API credential, OTP, signer material, or customer PII is present.
- Do not merge until reviewer comments and open factual claims are resolved.

## Suggested Local Checks

```bash
rg -n -i 'Stellar Wallets Kit|Circle CCTP|DeFindex|SEP-30|provider-ready' README.md docs diagrams
rg -n '\$[0-9,]+' README.md docs/roadmap.md
git diff --check
```

Out-of-scope terms may appear only in explicit exclusions, historical evidence, or warnings against unsupported assumptions.

## Evidence Updates

When a pending fact is confirmed, add the evidence source and date, update every affected authority/dependency table, and revise acceptance tests if behavior differs. Do not silently convert a proposed design into a verified implementation claim.
