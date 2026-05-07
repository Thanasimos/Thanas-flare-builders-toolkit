---
name: enosys-contracts
description: >
  Essential reference skill for Enosys Global DeFi protocol development on the Flare Network and its related chains.
  Use this skill whenever Darren asks about: interacting with Enosys smart contracts (DEX V2, DEX V3/CLMM, Loans/CDP,
  APYCloud, Governance), writing new Solidity contracts that integrate with Enosys products, building scripts or tools
  that call Enosys contract functions, understanding how Enosys products work internally, looking up contract addresses
  on Flare, Songbird, Coston1, or Coston2, understanding token addresses (HLN, APS, WFLR, FXRP, CDP, eUSDT, etc.),
  or building anything DeFi-related on the Flare Network. Trigger on any mention of Enosys, Flare DEX, DEX V3,
  CLMM, CDP token, Loans protocol, APYCloud, HLN, APS, FTSO oracle integration, or Flare DeFi development.
---

# Enosys Global — Smart Contract Reference Skill

Enosys Global (formerly FLR Finance) is the leading DeFi suite on the Flare Network. This skill gives you
authoritative contract addresses, product architecture, and integration patterns across all four networks.

## Quick Reference: Network Configuration

| Network | Chain ID | Role | RPC | Explorer |
|---------|----------|------|-----|----------|
| **Flare** | 14 | Production mainnet | `https://flare-api.flare.network/ext/C/rpc` | https://mainnet.flarescan.com |
| **Songbird** | 19 | Canary network (live, real $) | `https://songbird-api.flare.network/ext/C/rpc` | https://songbird-explorer.flare.network |
| **Coston2** | 114 | Flare testnet | `https://coston2-api.flare.network/ext/C/rpc` | https://coston2-explorer.flare.network |
| **Coston1** | 16 | Songbird testnet | `https://coston-api.flare.network/ext/C/rpc` | https://coston-explorer.flare.network |

**FlareContractRegistry** (same address on ALL networks): `0xaD67FE66660Fb8dFE9d6b1b4240d8650e30F6019`
Use this to resolve native Flare system contract addresses (WNat, FtsoRewardManager, etc.) on any network.

---

## Product Overview

Enosys runs a suite of DeFi products, all fees flowing into the **APYCloud** (Central Yield Aggregator), which
distributes rewards to governance stakers.

### Product Hierarchy
```
APYCloud (ProfitDistribution contracts) ← collects fees from all products
├── DEX V2  — Uniswap V2 style CPAMM (constant product)
├── DEX V3  — Uniswap V3 style CLMM (concentrated liquidity)  ← main product on Flare
├── Loans   — Liquity V2 fork CDP protocol (mint CDP stablecoin)
├── Farms   — Yield farming / staking
├── Bridge  — Cross-chain asset bridge
└── Governance (APYCloud) — HLN + APS token staking, proposals
```

---

## Contract Addresses by Network

> **Full address tables are in the reference files. Read them before writing any integration code.**
> - `references/addresses_flare.md` — Flare mainnet (production)
> - `references/addresses_songbird.md` — Songbird canary network
> - `references/addresses_coston2.md` — Coston2 testnet (Flare test)
> - `references/addresses_coston1.md` — Coston1 testnet (Songbird test)

### Most-Used Addresses: Flare Mainnet (Chain ID 14)

**Core Tokens**
```
WFLR (Wnat):  0x1D80c49BbBCd1C0911346656B529DF9E5c2F783d
sFLR:         0x12e605bc104e93B45e1aD99F9e555f659051c2BB
HLN:          0x140D8d3649Ec605CF69018C627fB44cCC76eC89f
APS:          0xfF56Eb5b1a7FAa972291117E5E9565dA29bc808d
FXRP:         0xAd552A648C74D49E10027AB8a618A3ad4901c5bE
CDP (stable): 0x6Cd3a5Ba46FA254D4d2E3C2B37350ae337E94a0F
eUSDT:        0x96B41289D90444B8adD57e6F265DB5aE8651DF29
USDT0:        0xe7cd86e13AC4309349F30B3435a9d337750fC82D
```

**DEX V2 (Uniswap V2 style)**
```
EnosysDexFactory: 0x28b70f6Ed97429E40FE9a9CD3EB8E86BCBA11dd4
EnosysDexRouter:  0x088EeCB467B3968Da36c71F05023A1d3133B2B83
```

**DEX V3 (Concentrated Liquidity / CLMM)**
```
EnosysDexV3Factory:         0x17AA157AC8C54034381b840Cb8f6bf7Fc355f0de
SwapRouter:                 0x5FD34090E9b195d8482Ad3CC63dB078534F1b113
NonfungiblePositionManager: 0xD9770b1C7A6ccd33C75b5bcB1c0078f46bE46657
Quoter (V1):                0x5Be6edd89feC3ab9889D49337B19C101712eBB77
QuoterV2:                   0x0A32EE3f66cC9E68ffb7cBeCf77bAef03e2d7C56
EpochManager:               0x389D610CBB52A2Da635a589013Ba542e807C74f7
RewardManagerUpgradableFactory: 0xbEEFA7FAb245B568183D5f67731487908630d801
```

**Loans (Liquity V2 Fork)**
```
boldToken (CDP):      0x6Cd3a5Ba46FA254D4d2E3C2B37350ae337E94a0F
collateralRegistry:   0x9474206bc035D03d142264fd9913d1D51246d3AC
hintHelpers:          0x0f493519aFF37a4Ec95f31301b7c4F987915Acb0
multiTroveGetter:     0xb80C59BBCEB205bCA25Eea2e4221717f23b339d3
liquidationBot:       0xCB5e11E03c76396ae0e05A328707F85A725F0a9C
--- FXRP Branch ---
borrowerOperations:   0x18139E09Fb9a683Dd2c2df5D0edAD942c19CE912
troveManager:         0xc46e7d0538494FEb82b460b9723dAba0508C8Fb1
stabilityPool:        0x2c817F7159c08d94f09764086330c96Bb3265A2f
priceFeed (FXRP):     0xFc35d431Ce1445B9c79ff38594EF454618D2Ec49
--- WFLR Branch ---
borrowerOperations:   0x19b154D5d20126a77309ae01931645a135E4E252
troveManager:         0xB6cB0c5301D4E6e227Ba490cee7b92EB954ac06D
stabilityPool:        0x0Dd6daab4cB9A0ba6707Cf59DBfbc28cc33CA24A
priceFeed (WFLR):     0xBCB5420d7F6346DB739a7C49FC90AB37553B68dc
```

**Governance & APYCloud**
```
EnosysGovernanceStakeManagerHln: 0x988E94a0AEFB1fCdC0C4d44dDBa103C5d4c6c6b0
EnosysGovernanceStakeManagerAps: 0x7eB8CeB0F64D934a31835b98eB4cbAb3cA56dF28
EnosysGovernorSingle:            0x00074DdA3EE851a69475b453Ab95C20b7a867325
EnosysGovernorDual:              0xb03D96550Ab218161ECeccbd78A0EA136BA20B1c
ProfitDistribution_80-20_DexV2:  0xc66ff685376Cdb6D56410101757d27F13dd0bd0A
ProfitDistribution_80-20_DexV3:  0x36794833ae009368fdFb9736216F1160B9d1cBbE
ProfitDistribution_50-50_SimpleStaking: 0xd7C06B3dD6597A2958D14cd6478f444C095e5d25
ProfitDistribution_50-50_FtsoRewards:  0xf4bC63DAE1F7E424E28E67138b3Fb00B6c2820AE
```

**Third-Party / Aggregator Contracts (Flare Mainnet)**
```
BlazeSwap Router:   0xe3A1b355ca63abCBC9589334B5e609583C7BAa06
OpenOcean Exchange: 0x6352a56caadC4F1E25CD6c75970Fa768A3304e64
Multicall3:         0xcA11bde05977b3631167028862bE2a173976CA11
```

---

## How Enosys Products Work

### DEX V2 — Constant Product AMM
- Uniswap V2 fork. Pairs are proxy contracts (upgradeable), factory deploys new pairs.
- LP tokens are ERC-20, fungible within a pair. Fees auto-compound to pool.
- **L1 rewards**: wFLR held in pools is delegated → earns FTSO delegation rewards + FlareDrops → redistributed to LPs via `FtsoRewardRedistributor` and `MonthlyAirdropRedistributor`.
- Key router functions: `swapExactTokensForTokens`, `addLiquidity`, `removeLiquidity`

### DEX V3 — Concentrated Liquidity Market Maker (CLMM)
- Uniswap V3 architecture. Each position = a unique NFT (ERC-721) from `NonfungiblePositionManager`.
- Liquidity is deployed in specific **price tick ranges** — capital only works when price is in range.
- **Fee tiers**: 100 (0.01%), 500 (0.05%), 3000 (0.3%), 10000 (1%) bps — each is a separate pool.
- Pool naming convention in addresses file: `TOKEN_A/TOKEN_B/FEE_TIER` e.g. `Wnat/eUSDT/3000`
- **Epoch-based reward system**: `EpochManager` tracks each reward epoch. `RewardManager` contracts distribute per-pool incentives. Rewards are weighted by time-in-range × liquidity.
- rFLR incentives: user's active liquidity proportion during an epoch determines share of rFLR rewards (vest 12 months, 50% penalty for early claim).
- L1 rewards flow through same redistribution contracts as V2.
- Key contracts: Factory (creates pools), SwapRouter (executes swaps), NonfungiblePositionManager (manage positions), Quoter/QuoterV2 (simulate swaps off-chain).
  - **QuoterV1** (`0x5Be6edd89feC3ab9889D49337B19C101712eBB77`): ABI uses `quoteExactInputSingle(tokenIn, tokenOut, fee, amountIn, sqrtPriceLimitX96) returns (uint256)` — use for simple single-uint256 quote calls.
  - **QuoterV2** (`0x0A32EE3f66cC9E68ffb7cBeCf77bAef03e2d7C56`): returns a tuple — use when you need gas estimates or additional output data.

**Pool address derivation**: Use `EnosysDexV3Factory.getPool(tokenA, tokenB, fee)` to get pool address.

### Loans — Liquity V2 Fork (CDP Protocol)
- Users lock collateral (FXRP or WFLR) in **Troves** to mint the **CDP stablecoin** (ticker: CDP).
- CDP maintains ~$1 peg via stability pool, user-set interest rates, and redemption mechanism.
- **Branches**: Each collateral type has its own set of contracts (BorrowerOperations, TroveManager, StabilityPool, PriceFeed). Currently 2 branches on Flare: FXRP and WFLR.
- **FTSO oracle**: `PriceFeed` contracts use Flare's FTSO (not Chainlink) for collateral pricing.
- **wFLR delegation**: Collateral deposited as WFLR is auto-delegated for FTSO rewards + FlareDrops.
- Fork changes vs Liquity V2 (see `references/fork_changes.md`):
  - Contracts are upgradeable (OZ proxy pattern) — owner is a 4/6 Safe multisig
  - FTSO replaces Chainlink oracle
  - CDP renamed from BOLD
  - Pauser functions added
  - Collateral-specific mint caps
  - Semi-diamond proxy for large contracts (BorrowerOperations/TroveManager facets)
- Key flow: `BorrowerOperations.openTrove()` → mint CDP → use CDP in ecosystem

### APYCloud — Profit Distribution
- `ProfitDistribution` proxy contracts collect fee revenue from DEX V2 and DEX V3.
- 80/20 split: 80% to protocol, 20% to APYCloud treasury (or varies by product).
- Governance stakers (HLN/APS staked in `EnosysGovernanceStakeManager`) receive yield from APYCloud.
- Two governance paths: `EnosysGovernorSingle` (one token) and `EnosysGovernorDual` (both tokens).

### Governance Tokens
| Token | Symbol | Max Supply | Role |
|-------|--------|-----------|------|
| Helion | HLN | 150,000,000 | Secondary governance. Can vote on token listings/pairs. Replaces fees on DEX. |
| Apsis | APS | 15,000 | Primary governance. Higher voting weight. Primary reward token from farms. |

- HLN staked in `EnosysGovernanceStakeManagerHln` → earns APYCloud yield
- APS staked in `EnosysGovernanceStakeManagerAps` → earns APYCloud yield
- Proposals require both to be staked; `EnosysGovernorDual` requires both token types

---

## Integration Patterns

### Fetch ABI from Flarescan API
```javascript
const BASE_URL = {
  flare:    "https://mainnet.flarescan.com/api",
  songbird: "https://songbird-explorer.flare.network/api",
  coston2:  "https://coston2-explorer.flare.network/api",
  coston1:  "https://coston-explorer.flare.network/api",
};
const addr = "0x17AA157AC8C54034381b840Cb8f6bf7Fc355f0de"; // V3 Factory
const res = await fetch(`${BASE_URL.flare}?module=contract&action=getabi&address=${addr}`);
const abi = JSON.parse((await res.json()).result);
```

### Connect to Flare with ethers.js
```javascript
import { ethers } from "ethers";
const provider = new ethers.JsonRpcProvider("https://flare-api.flare.network/ext/C/rpc");
// Chain ID 14 for Flare, 19 for Songbird, 114 for Coston2, 16 for Coston1
```

### Execute a V3 Swap (exactInputSingle)
```javascript
// Always approve router first: token.approve(SWAP_ROUTER_ADDRESS, amountIn)
const router = new ethers.Contract(SWAP_ROUTER_ADDRESS, SWAP_ROUTER_ABI, signer);
const tx = await router.exactInputSingle({
  tokenIn: WFLR_ADDRESS,
  tokenOut: EUSDT_ADDRESS,
  fee: 3000,                         // 0.3% pool
  recipient: userAddress,
  deadline: Math.floor(Date.now() / 1000) + 300,
  amountIn: ethers.parseEther("100"),
  amountOutMinimum: 0n,              // set properly in production!
  sqrtPriceLimitX96: 0n,             // 0 = no price limit
});
```

### Collect All Fees from a V3 Position
```javascript
const npm = new ethers.Contract(NPM_ADDRESS, NPM_ABI, signer);
const MAX_UINT128 = (2n ** 128n) - 1n;
await npm.collect({
  tokenId: myTokenId,
  recipient: userAddress,
  amount0Max: MAX_UINT128,
  amount1Max: MAX_UINT128,
});
```

### Claim DEX V3 Incentive Rewards
```javascript
const rewardManager = new ethers.Contract(REWARD_MANAGER_ADDRESS, REWARD_MANAGER_ABI, signer);
// Check what's available first (free read)
const unclaimed = await rewardManager.getUnclaimed([tokenId], 100); // up to 100 epochs
// Then claim
await rewardManager.claim([tokenId], 100, userAddress);
```


```javascript
// Use QuoterV2 (off-chain simulation, doesn't spend gas on actual swap)
const quoter = new ethers.Contract(
  "0x0A32EE3f66cC9E68ffb7cBeCf77bAef03e2d7C56", // QuoterV2 Flare
  quoterV2Abi,
  provider
);
const quote = await quoter.quoteExactInputSingle.staticCall({
  tokenIn: WFLR_ADDRESS,
  tokenOut: EUSDT_ADDRESS,
  fee: 3000,
  amountIn: ethers.parseEther("100"),
  sqrtPriceLimitX96: 0n,
});
```

### Get Pool Address from V3 Factory
```javascript
const factory = new ethers.Contract(
  "0x17AA157AC8C54034381b840Cb8f6bf7Fc355f0de",
  ["function getPool(address,address,uint24) view returns (address)"],
  provider
);
const poolAddress = await factory.getPool(tokenA, tokenB, feeTier);
```

### Read a Trove (Loans)
```javascript
const troveManager = new ethers.Contract(
  "0xc46e7d0538494FEb82b460b9723dAba0508C8Fb1", // FXRP branch
  ["function getTroveDebt(uint256) view returns (uint256)",
   "function getTroveColl(uint256) view returns (uint256)"],
  provider
);
```

---

## Flare-Specific Development Notes

1. **FTSO Oracle**: Flare has a native decentralized oracle. Access via `FlareContractRegistry` to get `FtsoRegistry`/`FtsoV2` address. No Chainlink on Flare.
2. **wFLR delegation**: Any contract holding wFLR should consider delegation to earn FTSO rewards. Use `WNatRegistry` to track delegations across product contracts.
3. **FlareDrops**: Monthly distributions to wFLR holders (including those in LPs). Handled by `MonthlyAirdropRedistributor`.
4. **Block time**: ~1.8 seconds, near-instant finality. Gas token is FLR/SGB/CFLR/C2FLR depending on network.
5. **Proxy pattern**: Most Enosys contracts are upgradeable proxies. Always interact with the **proxy address**, not the implementation. ProxyAdmin controls upgrades (owner = Enosys Safe 4/6 multisig).
6. **EVM compatible**: Flare is fully EVM-compatible. Standard Hardhat/Foundry/ethers.js workflows apply.
7. **Testing track**: Develop on Coston2 → stage on Songbird → production on Flare (mirrors Enosys' own 11-environment testing process).
8. **Swap callback requirement**: Any contract calling `pool.swap()` directly MUST implement `enosysdexV3SwapCallback(int256 amount0Delta, int256 amount1Delta, bytes data)`. Use SwapRouter for standard swaps — it handles the callback internally.
9. **Protocol fee collection**: Protocol fees accumulate in each pool and must be swept to `ProfitDistribution` by calling `factory.collectProtocolFeesAll()` (callable by anyone). The `ProfitDistribution` then distributes to APYCloud stakers.
10. **Governance voting power**: In `EnosysGovernorDual`, `1 APS = 10,000 HLN` in vote weight. Tokens must be **staked** in the StakeManager to vote — holding in wallet doesn't count.
11. **Fee tier → tick spacing**: 100 bps → spacing 1, 500 bps → spacing 10, 3000 bps → spacing 60, 10000 bps → spacing 200. Tick ranges in `mint()` must be multiples of the tick spacing.
12. **Universal Router**: Supports batched V2+V3 swaps and ETH wrapping in one tx. Command bytes from `Commands.sol`: `0x00`=V3_EXACT_IN, `0x01`=V3_EXACT_OUT, `0x08`=V2_EXACT_IN, `0x0b`=WRAP_ETH. Available on Songbird (`0xfc737fa...`) and Coston1 (`0xc97ff9a...`).
13. **Solidity version**: DEX V3 contracts use `pragma solidity =0.7.6`. Governance and APYCloud use `^0.8.13`. Keep this in mind when writing integrating contracts or copying interfaces.

---

## Reference Files

Load these when working on specific areas:

- **`references/abis_and_interfaces.md`** — Full ABI fragments for all major contracts (SwapRouter, NonfungiblePositionManager, Factory, Pool, EpochManager, RewardManager, Governance). **Read this first when writing integration code.**
- **`references/addresses_flare.md`** — All Flare mainnet addresses (tokens, pools, all products)
- **`references/addresses_songbird.md`** — All Songbird addresses
- **`references/addresses_coston2_coston1.md`** — Both testnet addresses (Coston2 + Coston1)
- **`references/loans_addresses.md`** — Full Loans protocol address breakdown (branches + implementations)
- **`references/dexv3_reward_managers.md`** — DEX V3 per-pool reward manager addresses + deadlines
- **`references/fork_changes.md`** — Enosys Loans fork changes vs Liquity V2
