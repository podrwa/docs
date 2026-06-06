# PODRWA — Export Commodity Vaults

> **Scope:** This repository contains the $PODRWA token, Solana/Anchor escrow vault contracts, KSA SPV bridge module, and the podrwa.com investor portal. It is a separate initiative from the [Proof of Dirt Oracle](https://github.com/proofofdirt/pod-oracle) — which this protocol depends on for agricultural verification.

---

## What This Is

$PODRWA is the tokenized RWA layer of the Proof of Dirt ecosystem. Solana smart contract vaults represent fractional export commodity cycles — animal feed crops, live animals, and Halal carcass meat — exported from Ethiopia to MENA markets.

MENA investors subscribe via **PODRWA SPV Ltd (KSA)** in SAR or USD. Capital routes in-kind to vetted subcontractors in Ethiopia. Export proceeds are retained in hard currency by the KSA SPV. Investors settle in SAR/USD. Every payment release is gated by the [POD Oracle](https://github.com/proofofdirt/pod-oracle).

**This is not a live product.** Oracle effectiveness must be demonstrated before any vaults go live. See [Milestone B](docs/architecture.md#milestones).

---

## Repository Structure

```
podrwa/
├── website/
│   └── index.html          # podrwa.com landing page
├── contracts/
│   ├── escrow/             # $PODRWA vault state machine (Solana/Anchor)
│   │   └── README.md
│   └── spv-bridge/         # SPV subscription receipt module
│       └── README.md
├── bounties/
│   ├── ESCROW-003.md       # Build the vault contracts — 60,000 $DIRT
│   └── BRIDGE-008.md       # Build the SPV bridge — 50,000 $DIRT
├── docs/
│   └── architecture.md     # Capital flow, oracle gate, SPV structure
└── .github/
    └── ISSUE_TEMPLATE/
        └── bounty_submission.md
```

---

## Oracle Dependency

**All payment releases require oracle confirmation.** Every tranche instruction must pass:

```rust
require!(oracle_record.status == VerificationStatus::Confirmed);
require!(oracle_record.confidence >= 0.85);
require!(oracle_record.contract_farming_status == ContractFarmingStatus::Registered);
```

Oracle code is open-source at [proofofdirt/pod-oracle](https://github.com/proofofdirt/pod-oracle). All oracle tooling is and will remain open source.

---

## Active Bounties

| Bounty | Scope | Reward |
|--------|-------|--------|
| [ESCROW-003](bounties/ESCROW-003.md) | $PODRWA vault state machine (Solana/Anchor) | 60,000 $DIRT |
| [BRIDGE-008](bounties/BRIDGE-008.md) | KSA SPV subscription receipt bridge | 50,000 $DIRT |

Contributors also earn Streamflow-vested $DIRT allocations as core builders. Contact [email protected] to discuss scope before starting.

---

## Security

> Third-party audit **mandatory** before any vault reaches TVL > $100k. Do not request mainnet deployment without explicit sign-off from the core team.

---

## Contact

- General: [email protected]
- KSA SPV / compliance: [email protected]
- Oracle integration: [github.com/proofofdirt/pod-oracle](https://github.com/proofofdirt/pod-oracle)
