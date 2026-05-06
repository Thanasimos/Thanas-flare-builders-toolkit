# Enosys Loans — Fork Changes vs Liquity V2

Source: `Fork_changes_list.md` (provided by Enosys team)

These are the key ways Enosys Loans differs from the upstream Liquity V2 protocol.
Understanding these is critical when reading or extending the contract code.

---

## 1. Contract Upgradeability

Enosys made contracts **upgradeable** (OpenZeppelin proxy pattern) because the Flare Network
is relatively young and protocol-breaking changes at the network level are possible.

- All major contracts are behind transparent/UUPS proxies
- Ownership is held by a **Safe multisig with 4/6 signer requirements** — no single person can upgrade
- Even contracts without current admin functions use `Ownable2Step` as base to reserve storage layout

**Integration implication**: Always interact with **proxy addresses**, never implementation addresses.
Implementation addresses may change on upgrades. The proxy address is permanent.

---

## 2. Extended Owner/Admin Functions

- All contracts use `OpenZeppelin Ownable2Step` (two-step ownership transfer for safety)
- No ownership renouncing — admin control is intentionally retained
- Almost all constants converted to **mutable state variables** — can be updated via governance
- Parameters that can be updated by admin:
  - `SP_YIELD_SPLIT` — stability pool yield split
  - `MAX_DEBT_CAP` — maximum protocol debt ceiling
  - `UPFRONT_INTEREST_PERIOD` — interest period for upfront payment
  - `INTEREST_RATE_ADJ_COOLDOWN` — cooldown between interest rate changes
  - `MIN_BOLD_IN_SP` — minimum CDP in stability pool
  - `ETH_GAS_COMPENSATION` — gas compensation amount
- New collateral branches can be added by admin any time after launch

---

## 3. Collateral-Specific Mint Caps

New security feature not in upstream Liquity V2.

- Each collateral type has an independent **mint cap** (max CDP mintable against that collateral)
- Necessary because primary collateral (FXRP) is a bridged asset — economic risk is higher
- At launch: FXRP cap = $4M, WFLR cap = $1M
- Caps are updatable by admin (governance)

---

## 4. Flare Network wNAT Reward Systems

Native Flare incentives integrated:
- **FTSO Delegation Rewards**: wFLR held as collateral is auto-delegated via `NftWnatBalanceTracker`
- **FlareDrops**: Monthly FLR distributions claimable by trove owners
- WFLR branch has `nftWnatBalanceTracker` for tracking delegation state per NFT/trove

---

## 5. FTSO Oracle (Replaces Chainlink)

Chainlink does not support the Flare Network. Enosys built custom `PriceFeed` contracts:
- Reads prices from Flare's **FTSO (Flare Time Series Oracle)** — a decentralized, native oracle
- FTSO aggregates price feeds from independent signal providers (no single source of failure)
- Each collateral branch has its own `PriceFeed` contract with appropriate FTSO feed ID
- FTSO V2 supports thousands of price feeds — protocol is future-proofed for new collateral types

**Integration note**: If building integrations that need collateral prices, use the PriceFeed contract
address from the branch, not a raw FTSO feed — the PriceFeed handles decimals, fallbacks, etc.

---

## 6. Pauser Functions

Security feature for emergency scenarios:
- Admin can **pause** specific operations (borrowing, redemptions, etc.)
- Designed for market manipulation events or oracle failures
- Part of the broader Enosys security posture (also includes 4/6 Safe multisig)

---

## 7. Legacy Bug Fix

Fixed a bug from Liquity V1 that carried into V2, related to **total stake edge case scenarios**.
This is a correctness fix — no interface impact.

---

## 8. Semi-Diamond Proxy (Contract Size Limit)

Some contracts exceeded Ethereum/EVM 24KB contract size limit due to the additional features.
Enosys addressed this with a **semi-diamond proxy pattern**:

- Logic from `BorrowerOperations` moved to `BorrowerOperationsBatchFacet`
- Logic from `TroveManager` moved to `TroveManagerRedemptionLiquidationFacet`
- The main contracts delegate specific calls to these facet contracts
- ABIs for both the main contract and the facet are needed for full interaction

**Integration implication**: When calling redemption or liquidation functions, check whether
the function lives on `TroveManager` or `TroveManagerRedemptionLiquidationFacet`.

---

## 9. Branding / Token Rename

- Upstream uses **BOLD** as the stablecoin name
- Enosys renamed to **CDP** (Collateralized Debt Position token)
- Contract variable names may still say `boldToken` internally — this is the CDP token
- NFT card background uses a **static SVG** instead of the dynamically colorized version in Liquity V2
