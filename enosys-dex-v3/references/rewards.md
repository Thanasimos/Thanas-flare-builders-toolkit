# Enosys DEX V3 — Reward Mechanics Reference

Source-verified from `dex-v3-main` repo (ActivePositionRow, ClaimFeesButton,
IncentiveRewardsWrapper, RFLRRewardsWrapper, config.ts).

---

## Overview

Every Enosys V3 LP position can earn up to four reward types simultaneously.
They are completely independent — each has its own contract, check method, and
claim transaction. The claim sequence runs them one after another; a failure in
one step does not block the others.

---

## 1. Swap Fees

**What:** Standard Uniswap V3 swap fees accumulated while position was in range.

**Where stored:** `tokensOwed0` and `tokensOwed1` fields of the `positions()` struct.
For active (in-range) positions, there are also accrued-but-not-yet-checkpointed
fees that must be calculated from fee growth data.

**How to check (inactive positions):**
```javascript
const pos = await positionManager.positions(tokenId)
const hasFees = pos.tokensOwed0 > 0n || pos.tokensOwed1 > 0n
```

**How to check (active positions — fee growth math):**
Uses `feeGrowthGlobal0X128`, `feeGrowthGlobal1X128` from pool `slot0`, plus
`feeGrowthOutside0X128` / `feeGrowthOutside1X128` from `pool.ticks(tickLower)`
and `pool.ticks(tickUpper)`. Compute `feeGrowthInside`, subtract
`feeGrowthInside0LastX128` stored in position, multiply by `liquidity / 2^128`.
Add result to `tokensOwed0/1` for the full pending amount.
(See `ActivePositionRowFull.getFees()` in `ActivePositionRow/index.tsx`.)

**How to claim:**
```javascript
const calldata = positionManager.interface.encodeFunctionData('collect', [{
  tokenId,
  recipient: walletAddress,
  amount0Max: 2n**128n - 1n,
  amount1Max: 2n**128n - 1n,
}])
await signer.sendTransaction({ to: POSITION_MANAGER, data: calldata })
```

**Token denomination:** token0 and token1 of the position pair.

---

## 2. APS Incentive Rewards (RewardManager)

**What:** APS token rewards emitted per-epoch to LPs who provided active liquidity.
Uses a 6-hour epoch system. Multiple reward programs can run simultaneously on
the same pool, each with its own RewardManager instance.

**Where:** Each pool has a `rewardManagers[]` array. Always use the **last entry**
as the currently active manager. Old entries are legacy/expired.

**How to check:**
```javascript
const rewardManager = new ethers.Contract(managerAddress, REWARD_MANAGER_ABI, provider)
const pending = await rewardManager.getUnclaimed([tokenId], 80)
// 80 = look back 80 epochs (~20 days). Returns uint256, 18 decimals.
// Returns 0 if pool has no reward manager — handle gracefully.
```

**How to claim:**
```javascript
await rewardManager.claim([tokenId], 10, recipientAddress, {
  gasLimit: Math.min(estimatedGas * 2, 8_000_000)
})
// epochsToClaimCnt=10 for the tx (vs 80 for the check) to stay within gas limits
```

**Gas notes:** Claim gas scales with `epochsToClaimCnt`. The app uses 10 per claim
tx and estimates gas, multiplying by 2 as a buffer, capped at 8M. Always override
gas for RewardManager claims or txs may fail with out-of-gas.

**Reward token:** Call `rewardManager.rewardToken()` to get the ERC-20 address.
On most Flare pools this is APS (`0xfF56Eb5b1a7FAa972291117E5E9565dA29bc808d`).

**Skip if:** Pool has `rewardManagers: []` — no APS program, skip all calls.

---

## 3. Delegation Rewards — FTSO (FtsoRewardRedistributor)

**What:** FTSO delegation rewards earned on the WFLR that is wrapped inside LP
positions. The PositionManager delegates its WFLR to FTSO providers on behalf of
LPs. Rewards are distributed per FTSO reward epoch.

**Contract:** `FTSO_REWARD_REDISTRIBUTOR = 0x5a0BfF8Ff1AF1DF28619Fce57d07E3bBb7BAF3d7`

**Eligibility:** Only pools with `native: true` in the pool registry. Non-native
pools don't hold WFLR (or hold a non-delegatable token), so skip these calls.

**How to check:**
```javascript
const ftso = new ethers.Contract(FTSO_REWARD_REDISTRIBUTOR, FTSO_MONTHLY_ABI, provider)
const pending = await ftso.getUnclaimed([tokenId])
// Returns uint256, 18 decimals, denominated in FLR/WFLR
```

**How to claim:**
```javascript
await ftso.claim([tokenId], recipientAddress)
```

**Token denomination:** FLR (native, paid as WFLR unwrapped to FLR).

---

## 4. FlareDrops — Monthly (MonthlyRewardManager)

**What:** Monthly FLR distribution to WFLR holders. LP positions holding WFLR
participate proportionally. This is the Flare "airdrop" program distributed
monthly from the original FLR token allocation.

**Contract:** `MONTHLY_REWARD_MANAGER = 0xef514BCC4ab6226f5666a30d0c4672f00946e3dD`

**Eligibility:** Only `native: true` pools **and** `chainId === 14` (Flare mainnet).
Not available on Songbird (chainId 19) — different contract structure there.

**How to check:**
```javascript
const monthly = new ethers.Contract(MONTHLY_REWARD_MANAGER, FTSO_MONTHLY_ABI, provider)
const pending = await monthly.getUnclaimed([tokenId])
// Same ABI shape as FtsoRewardRedistributor. Returns uint256, 18 decimals, FLR.
```

**How to claim:**
```javascript
await monthly.claim([tokenId], recipientAddress)
```

**Token denomination:** FLR.

---

## 5. rFLR — Display Only

**What:** Restricted FLR from the Flare Emission Committee's 510M FLR DeFi
incentive program. Allocated monthly to qualifying DEX V3 LPs based on TVL and
volume. **Not claimable on-chain from third-party tools.**

**How it works:** Enosys queries an internal backend API
(`/api/flr/v2/stats/rflr/{tokenId}`) to show pending amounts. The actual claim
happens at `https://portal.flare.network` using a merkle proof system.

**In our app:** For pools with `rflr: true` in the pool registry, show an
informational row:
```
↗  rFLR rewards may be pending — claim at portal.flare.network
```
Link directly to `https://portal.flare.network`. Never attempt a claim tx.

---

## Epoch system

The DEX V3 incentive system uses a custom epoch manager:
- Contract: `EPOCH_MANAGER = 0x389D610CBB52A2Da635a589013Ba542e807C74f7`
- `getCurrentRewardEpoch()` → current epoch number
- `epochPeriodSeconds()` → epoch length in seconds (initially 6 hours = 21600s)
- Rewards for epoch N become claimable after epoch N ends

Relevant for UI: can show "N epochs unclaimed" context to the user, but not
required for basic claim functionality.

---

## Multi-position batch patterns

The reward contracts accept **arrays** of tokenIds:
```javascript
// Check multiple positions in one call
const pending = await rewardManager.getUnclaimed([id1, id2, id3], 80)
// Returns total across all — useful for summary displays

// Claim multiple in one tx (saves gas vs individual txs)
await rewardManager.claim([id1, id2, id3], 10, recipient)
await ftso.claim([id1, id2, id3], recipient)
await monthly.claim([id1, id2, id3], recipient)
```

The NonfungiblePositionManager's `multicall` can batch multiple `collect()` or
`burn()` calls into a single transaction.

---

## Songbird differences (chainId 19)

Different contract addresses (from config.ts):
```javascript
POSITION_MANAGER:          '0xFC02eB1Ab5079627103357BfbB8167d3BBAc6f1e'
FTSO_REWARD_REDISTRIBUTOR: '0x421294D3eb38c87390fc7f1442623f5b7DBc5b86'
EPOCH_MANAGER:             '0xDD79E5b82948103273F6d63d566d97472a16f9e8'
// No MONTHLY_REWARD_MANAGER on Songbird
REWARD_MANAGERS_ARRAY: [   // multiple active managers on SGB
  '0xF0FD24Afef7425e19C4b2e910c3C199F8A8A3e2B',
  '0x0273cD7193D6a7340Cb59fE5486a3774dc025E78',
  '0xd178A5EE82ACec96450DFc454ab08a78D89D60a3',
  '0xa997E5FDd464b982d1Fbe51D8734A410744F3B7E',
]
```
No monthly FlareDrops on Songbird — skip `MONTHLY_REWARD_MANAGER` calls entirely.
