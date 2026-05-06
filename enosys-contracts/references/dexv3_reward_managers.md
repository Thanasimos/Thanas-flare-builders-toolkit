# DEX V3 Reward Manager Addresses

Reward managers distribute per-epoch incentives to liquidity providers in specific pools.
Each pool can have multiple reward managers (for different incentive programs).

Key addresses:
- `0x0171f12Ee91d8ff7FEFC8cA21B417a1209864b19` — Old/expired reward manager (deadline 2024-10-21)
- `0x38B542B05198aE2c8A16520B9f8A208b6552B77a` — Current active reward manager (deadline 2026-05-14)
- `0xAbB9F5b56bE1AC2257E37BBEC3fee1a39fe99725` — FXRP reward manager (deadline 2025-11-15)

---

## Flare Mainnet — Active Reward Managers

Only pools with active (non-expired) deadlines as of March 2026:

| Pool | Reward Manager | Deadline | Reward/Epoch |
|------|---------------|----------|--------------|
| HLN/Wnat/3000 | `0x38B542B05198aE2c8A16520B9f8A208b6552B77a` | 2026-05-14 | 0.05 |
| Wnat/APS/3000 | `0x38B542B05198aE2c8A16520B9f8A208b6552B77a` | 2026-05-14 | 0.05 |
| Wnat/eUSDT/3000 | `0x38B542B05198aE2c8A16520B9f8A208b6552B77a` | 2026-05-14 | 0.0125 |
| Wnat/eETH/3000 | `0x38B542B05198aE2c8A16520B9f8A208b6552B77a` | 2026-05-14 | 0.0125 |
| Wnat/eQNT/3000 | `0x38B542B05198aE2c8A16520B9f8A208b6552B77a` | 2026-05-14 | 0.0125 |
| HLN/APS/3000 | `0x38B542B05198aE2c8A16520B9f8A208b6552B77a` | 2026-05-14 | 0.05 |
| HLN/USDT0/3000 | `0x38B542B05198aE2c8A16520B9f8A208b6552B77a` | 2026-05-14 | 0.05 |
| APS/USDT0/3000 | `0x38B542B05198aE2c8A16520B9f8A208b6552B77a` | 2026-05-14 | 0.05 |
| HLN/FXRP/3000 | `0x38B542B05198aE2c8A16520B9f8A208b6552B77a` | 2026-05-14 | 0.05 |
| APS/FXRP/3000 | `0x38B542B05198aE2c8A16520B9f8A208b6552B77a` | 2026-05-14 | 0.05 |
| Wnat/FXRP/3000 | `0x38B542B05198aE2c8A16520B9f8A208b6552B77a` | 2026-05-14 | 0.05 |
| CDP/USDT0/500 | `0x38B542B05198aE2c8A16520B9f8A208b6552B77a` | 2026-05-14 | 0.25 |

---

## Songbird — Active Reward Managers

| Pool | Reward Manager | Deadline | Reward/Epoch |
|------|---------------|----------|--------------|
| Wnat/EXFI/3000 | `0xa997E5FDd464b982d1Fbe51D8734A410744F3B7E` | 2027-01-29 | 0.025 |
| Wnat/exUSDT/3000 | `0xa997E5FDd464b982d1Fbe51D8734A410744F3B7E` | 2027-01-29 | 0.025 |
| SFIN/EXFI/3000 | `0xa997E5FDd464b982d1Fbe51D8734A410744F3B7E` | 2027-01-29 | 0.025 |
| Wnat/SFIN/3000 | `0xa997E5FDd464b982d1Fbe51D8734A410744F3B7E` | 2027-01-29 | 0.025 |
| Wnat/exETH/3000 | `0xa997E5FDd464b982d1Fbe51D8734A410744F3B7E` | 2027-01-29 | 0.025 |
| Wnat/exXDC/3000 | `0xa997E5FDd464b982d1Fbe51D8734A410744F3B7E` | 2027-01-29 | 0.025 |
| Wnat/FXRP/3000 | `0xa997E5FDd464b982d1Fbe51D8734A410744F3B7E` | 2027-01-29 | 0.025 |
| SFIN/FXRP/3000 | `0xa997E5FDd464b982d1Fbe51D8734A410744F3B7E` | 2027-01-29 | 0.025 |
| EXFI/FXRP/3000 | `0xa997E5FDd464b982d1Fbe51D8734A410744F3B7E` | 2027-01-29 | 0.025 |

---

## How Reward Managers Work

1. **EpochManager** tracks reward epochs (time periods).
2. Each pool has 1+ `RewardManager` contracts, each running a separate incentive campaign.
3. At the end of each epoch, the system calculates each LP's **active liquidity** (time × liquidity in range).
4. LPs with more concentrated, in-range liquidity earn a higher share of that epoch's rewards.
5. Rewards are claimed through the `NonfungiblePositionManager` or dedicated UI.
6. The `RewardManagerUpgradableFactory` creates new RewardManager instances for new incentive programs.

To query if a pool has active rewards:
```javascript
const epochManager = new ethers.Contract(EPOCH_MANAGER_ADDRESS, epochManagerAbi, provider);
const currentEpoch = await epochManager.currentEpochId();
// Then query RewardManager for the pool to check if epoch deadline has passed
```
