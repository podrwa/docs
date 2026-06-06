# PODRWA Architecture

## Capital Flow

> **Constraint:** Ethiopian law prohibits USDC/stablecoin transactions. All capital into Ethiopia flows as fiat bank wire. The vault is a state machine and audit log — not a payment rail into Ethiopia.

```
1. MENA investor subscribes to PODRWA SPV KSA (SAR/USD, fiat, off-chain, KYC/AML)
2. SPV confirms receipt → spv-bridge mints $PODRWA tokens on Solana (investor receipt)
3. Oracle verifies agricultural activity → PDA written to Solana
4. Vault approves tranche → emits on-chain event → SPV executes fiat wire to Ethiopian bank partner
5. Ethiopian bank partner disburses ETB to Turmi Technologies PLC
6. Turmi routes capital in-kind to vetted subcontractors (tractor operators, input suppliers, livestock vendors)
7. Export executed → MENA buyer → hard-currency proceeds → PODRWA SPV KSA (offshore)
8. Investor settlement in SAR/USD via KSA banking; $PODRWA tokens burned on settlement
9. Protocol fee → $DIRT AMM buyback on Solana
```

---

## Oracle Gate

Every payment release instruction must verify against the POD Oracle PDA:

```rust
require!(oracle_record.status == VerificationStatus::Confirmed, ErrorCode::OracleUnconfirmed);
require!(oracle_record.confidence >= 0.85, ErrorCode::ConfidenceBelowThreshold);
require!(oracle_record.contract_farming_status == ContractFarmingStatus::Registered, ErrorCode::ComplianceNotRegistered);
```

Oracle source: [proofofdirt/pod-oracle](https://github.com/proofofdirt/pod-oracle)

---

## Vault Types (MVP)

| Vault | Commodity | MENA Destination |
|-------|-----------|-----------------|
| Animal feed crop vault | Sorghum, maize | KSA, UAE, Jordan |
| Animal acquisition vault | Cattle, sheep, goats | KSA, Yemen, Egypt |
| Halal carcass meat vault | Beef, mutton | KSA, UAE, Kuwait |

---

## $PODRWA Token

$PODRWA is an investor receipt token on Solana — not a tradable speculative asset. Each token represents a verified fiat subscription to the PODRWA SPV KSA. Tokens are:
- Minted by the `spv-bridge` module when SPV confirms fiat receipt
- Non-transferable without SPV authority (KYC-gated)
- Burned on investor settlement at cycle close
- Used for governance and cycle-state tracking

---

## KSA SPV Structure

**PODRWA SPV Ltd** (Kingdom of Saudi Arabia, registration in progress):
- Accepts MENA investor subscriptions in SAR or USD
- Full KYC/AML compliance under SAMA regulation
- Retains hard-currency export proceeds offshore (before ETB conversion)
- Distributes investor returns in SAR/USD from KSA banking
- Shariah-compatible in-kind financing structure (Murabaha-compatible by design)

---

## Milestones

The PODRWA initiative launches **after** the POD Oracle demonstrates effectiveness. Key gates:

| Milestone | Trigger | Outcome |
|-----------|---------|---------|
| **Oracle MVP** | SAR + NDVI pipeline live, first plots verified | Vault contracts can be built and audited |
| **Pilot** | First export cycle completed with oracle verification | SPV formation and first vault subscription opens |
| **Milestone B** | Oracle indices reach 95% accuracy vs MENA spot prices | Buyers settle forward contracts directly with producers |

---

## Security Requirements

- Third-party audit mandatory before any vault reaches TVL > $100k
- All oracle integration must use verified PDA accounts
- SPV authority key must be a hardware wallet multisig
- No mainnet deployment without core team sign-off

Contact [email protected] before beginning any audit engagement.
