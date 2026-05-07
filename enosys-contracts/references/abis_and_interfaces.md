# Enosys Contract ABIs & Interfaces

Sourced directly from the Enosys contract repositories. Use these for ethers.js / viem contract calls.

---

## DEX V3 — SwapRouter (ISwapRouter)

Address (Flare): `0x5FD34090E9b195d8482Ad3CC63dB078534F1b113`

```solidity
// Single-hop exact input swap
function exactInputSingle(ExactInputSingleParams calldata params) external payable returns (uint256 amountOut)
// params: { tokenIn, tokenOut, fee, recipient, deadline, amountIn, amountOutMinimum, sqrtPriceLimitX96 }

// Multi-hop exact input swap (encoded path)
function exactInput(ExactInputParams calldata params) external payable returns (uint256 amountOut)
// params: { path, recipient, deadline, amountIn, amountOutMinimum }

// Single-hop exact output swap
function exactOutputSingle(ExactOutputSingleParams calldata params) external payable returns (uint256 amountIn)
// params: { tokenIn, tokenOut, fee, recipient, deadline, amountOut, amountInMaximum, sqrtPriceLimitX96 }

// Multi-hop exact output swap
function exactOutput(ExactOutputParams calldata params) external payable returns (uint256 amountIn)
// params: { path, recipient, deadline, amountOut, amountInMaximum }
```

**Multi-hop path encoding**: ABI-encode as `abi.encodePacked(token0, fee0, token1, fee1, token2)` — tokens as `address` (20 bytes), fees as `uint24` (3 bytes).

**Swap callback**: The pool calls `enosysdexV3SwapCallback(int256 amount0Delta, int256 amount1Delta, bytes data)` on the caller. When writing contracts that trigger swaps, you must implement this callback.

### ethers.js ABI fragment
```javascript
const SWAP_ROUTER_ABI = [
  "function exactInputSingle((address tokenIn, address tokenOut, uint24 fee, address recipient, uint256 deadline, uint256 amountIn, uint256 amountOutMinimum, uint160 sqrtPriceLimitX96)) external payable returns (uint256 amountOut)",
  "function exactInput((bytes path, address recipient, uint256 deadline, uint256 amountIn, uint256 amountOutMinimum)) external payable returns (uint256 amountOut)",
  "function exactOutputSingle((address tokenIn, address tokenOut, uint24 fee, address recipient, uint256 deadline, uint256 amountOut, uint256 amountInMaximum, uint160 sqrtPriceLimitX96)) external payable returns (uint256 amountIn)",
  "function exactOutput((bytes path, address recipient, uint256 deadline, uint256 amountOut, uint256 amountInMaximum)) external payable returns (uint256 amountIn)",
];
```

---

## DEX V3 — NonfungiblePositionManager (INonfungiblePositionManager)

Address (Flare): `0xD9770b1C7A6ccd33C75b5bcB1c0078f46bE46657`

```solidity
// Read a position by NFT token ID
function positions(uint256 tokenId) external view returns (
    uint96 nonce, address operator,
    address token0, address token1, uint24 fee,
    int24 tickLower, int24 tickUpper,
    uint128 liquidity,
    uint256 feeGrowthInside0LastX128, uint256 feeGrowthInside1LastX128,
    uint128 tokensOwed0, uint128 tokensOwed1
)

// Mint (create) a new LP position
function mint(MintParams calldata params) external payable returns (
    uint256 tokenId, uint128 liquidity, uint256 amount0, uint256 amount1
)
// MintParams: { token0, token1, fee, tickLower, tickUpper, amount0Desired, amount1Desired, amount0Min, amount1Min, recipient, deadline }

// Add liquidity to existing position
function increaseLiquidity(IncreaseLiquidityParams calldata params) external payable returns (
    uint128 liquidity, uint256 amount0, uint256 amount1
)
// IncreaseLiquidityParams: { tokenId, amount0Desired, amount1Desired, amount0Min, amount1Min, deadline }

// Remove liquidity from position (does NOT transfer tokens - use collect() after)
function decreaseLiquidity(DecreaseLiquidityParams calldata params) external payable returns (
    uint256 amount0, uint256 amount1
)
// DecreaseLiquidityParams: { tokenId, liquidity, amount0Min, amount1Min, deadline }

// Collect tokens owed (fees + withdrawn liquidity)
function collect(CollectParams calldata params) external payable returns (uint256 amount0, uint256 amount1)
// CollectParams: { tokenId, recipient, amount0Max, amount1Max }
// Use type(uint128).max for amount0Max/amount1Max to collect all

// Burn a fully-empty position NFT
function burn(uint256 tokenId) external payable

// ERC-721 standard
function ownerOf(uint256 tokenId) external view returns (address)
function tokenByIndex(uint256 index) external view returns (uint256)
function totalSupply() external view returns (uint256)

// Get the pool address for a given tokenId
function poolByTokenId(uint256 tokenId) external view returns (address)
```

### ethers.js ABI fragment
```javascript
const NPM_ABI = [
  "function positions(uint256 tokenId) external view returns (uint96 nonce, address operator, address token0, address token1, uint24 fee, int24 tickLower, int24 tickUpper, uint128 liquidity, uint256 feeGrowthInside0LastX128, uint256 feeGrowthInside1LastX128, uint128 tokensOwed0, uint128 tokensOwed1)",
  "function mint((address token0, address token1, uint24 fee, int24 tickLower, int24 tickUpper, uint256 amount0Desired, uint256 amount1Desired, uint256 amount0Min, uint256 amount1Min, address recipient, uint256 deadline)) external payable returns (uint256 tokenId, uint128 liquidity, uint256 amount0, uint256 amount1)",
  "function increaseLiquidity((uint256 tokenId, uint256 amount0Desired, uint256 amount1Desired, uint256 amount0Min, uint256 amount1Min, uint256 deadline)) external payable returns (uint128 liquidity, uint256 amount0, uint256 amount1)",
  "function decreaseLiquidity((uint256 tokenId, uint128 liquidity, uint256 amount0Min, uint256 amount1Min, uint256 deadline)) external payable returns (uint256 amount0, uint256 amount1)",
  "function collect((uint256 tokenId, address recipient, uint128 amount0Max, uint128 amount1Max)) external payable returns (uint256 amount0, uint256 amount1)",
  "function burn(uint256 tokenId) external payable",
  "function ownerOf(uint256 tokenId) external view returns (address)",
  "function totalSupply() external view returns (uint256)",
  "function tokenByIndex(uint256 index) external view returns (uint256)",
  "function poolByTokenId(uint256 tokenId) external view returns (address)",
  "event IncreaseLiquidity(uint256 indexed tokenId, uint128 liquidity, uint256 amount0, uint256 amount1)",
  "event DecreaseLiquidity(uint256 indexed tokenId, uint128 liquidity, uint256 amount0, uint256 amount1)",
  "event Collect(uint256 indexed tokenId, address recipient, uint256 amount0, uint256 amount1)",
  "event Transfer(address indexed from, address indexed to, uint256 indexed tokenId)",
];
```

---

## DEX V3 — Factory (IEnosysDexV3Factory)

Address (Flare): `0x17AA157AC8C54034381b840Cb8f6bf7Fc355f0de`

```javascript
const FACTORY_ABI = [
  "function getPool(address tokenA, address tokenB, uint24 fee) external view returns (address pool)",
  "function createPool(address tokenA, address tokenB, uint24 fee) external returns (address pool)",
  "function feeAmountTickSpacing(uint24 fee) external view returns (int24)",
  "function isPoolDeployedByFactory(address pool) external view returns (bool)",
  "function poolsCount() external view returns (uint256)",
  "function getPools() external view returns (address[])",
  "function pools(uint256 index) external view returns (address)",
  "function owner() external view returns (address)",
  "function governor() external view returns (address)",
  "function protocolFee() external view returns (uint8)",
  "function protocolFeeReceiver() external view returns (address)",
  "function collectProtocolFeesAll() external",
  "function collectProtocolFees(uint256 startPoolIndex, uint256 count) external",
  "event PoolCreated(address indexed token0, address indexed token1, uint24 indexed fee, int24 tickSpacing, address pool)",
];
```

**Fee tier → tick spacing mapping:**
- 100 bps (0.01%) → tickSpacing 1
- 500 bps (0.05%) → tickSpacing 10
- 3000 bps (0.3%) → tickSpacing 60
- 10000 bps (1%) → tickSpacing 200

---

## DEX V3 — Pool (IEnosysDexV3Pool)

```javascript
const POOL_ABI = [
  "function factory() external view returns (address)",
  "function token0() external view returns (address)",
  "function token1() external view returns (address)",
  "function fee() external view returns (uint24)",
  "function tickSpacing() external view returns (int24)",
  "function slot0() external view returns (uint160 sqrtPriceX96, int24 tick, uint16 observationIndex, uint16 observationCardinality, uint16 observationCardinalityNext, uint8 feeProtocol, bool unlocked)",
  "function liquidity() external view returns (uint128)",
  "function feeGrowthGlobal0X128() external view returns (uint256)",
  "function feeGrowthGlobal1X128() external view returns (uint256)",
  "function ticks(int24 tick) external view returns (uint128 liquidityGross, int128 liquidityNet, uint256 feeGrowthOutside0X128, uint256 feeGrowthOutside1X128, int56 tickCumulativeOutside, uint160 secondsPerLiquidityOutsideX128, uint32 secondsOutside, bool initialized)",
  "function swap(address recipient, bool zeroForOne, int256 amountSpecified, uint160 sqrtPriceLimitX96, bytes calldata data) external returns (int256 amount0, int256 amount1)",
  "event Swap(address indexed sender, address indexed recipient, int256 amount0, int256 amount1, uint160 sqrtPriceX96, uint128 liquidity, int24 tick)",
  "event Mint(address sender, address indexed owner, int24 indexed tickLower, int24 indexed tickUpper, uint128 amount, uint256 amount0, uint256 amount1)",
  "event Burn(address indexed owner, int24 indexed tickLower, int24 indexed tickUpper, uint128 amount, uint256 amount0, uint256 amount1)",
  "event Collect(address indexed owner, address recipient, int24 indexed tickLower, int24 indexed tickUpper, uint128 amount0, uint128 amount1)",
];
```

**Reading current price from `slot0`:**
```javascript
const [sqrtPriceX96, tick] = await pool.slot0();
// price = (sqrtPriceX96 / 2^96)^2  — gives token1 per token0
const price = (Number(sqrtPriceX96) / 2**96) ** 2;
```

---

## DEX V3 — EpochManager

Address (Flare): `0x389D610CBB52A2Da635a589013Ba542e807C74f7`

```javascript
const EPOCH_MANAGER_ABI = [
  "function getCurrentRewardEpoch() external view returns (uint256)",
  "function getEpochForTimestamp(uint256 timestamp) external view returns (uint256)",
  "function getEpochStartTime(uint256 epoch) external view returns (uint256)",
  "function startTime() external view returns (uint256)",
  "function startEpoch() external view returns (uint256)",
  "function epochPeriodSeconds() external view returns (uint256)",
];
```

---

## DEX V3 — RewardManager

Address (current active, Flare): `0x38B542B05198aE2c8A16520B9f8A208b6552B77a`

```javascript
const REWARD_MANAGER_ABI = [
  // Claim rewards for a list of NFT position IDs
  "function claim(uint256[] memory nftIds, uint256 epochsToClaimCnt, address recipient) external returns (uint256)",
  // Check unclaimed rewards (read-only, no gas)
  "function getUnclaimed(uint256[] memory nftIds, uint256 epochToClaimCnt) external view returns (uint256 totalReward)",
  // Check pending rewards for current epoch
  "function getPending(uint256[] memory nftIds) external view returns (uint256)",
  // Cache pool rewards up to a given epoch (can be called by anyone)
  "function cachePoolRewards(address pool, uint256 upToEpoch) external",
  // Last epoch claimed for a position
  "function epochToClaimNext(uint256 nftId) external view returns (uint256)",
  // Rewards configured for a pool+epoch
  "function rewardsForEpoch(address pool) external view returns (bool isRewardsReceived, uint256 totalRewards)",
  "function rewardToken() external view returns (address)",
  "function epochLowest() external view returns (uint256)",
  "function epochHighest() external view returns (uint256)",
  "event RewardClaimed(uint256 indexed nftId, uint256 indexed fromEpoch, uint256 indexed toEpoch, uint256 reward)",
];
```

---

## APYCloud — ProfitDistribution

```javascript
const PROFIT_DISTRIBUTION_ABI = [
  "function distributeRegisterTokens() external",
  // Call this to trigger fee collection and distribution to governance stakers
];
```

---

## Governance — EnosysGovernanceStakeManager

Addresses (Flare):
- HLN: `0x988E94a0AEFB1fCdC0C4d44dDBa103C5d4c6c6b0`
- APS: `0x7eB8CeB0F64D934a31835b98eB4cbAb3cA56dF28`

```javascript
const STAKE_MANAGER_ABI = [
  // Stake governance tokens
  "function stake(uint256 amount) external",
  "function stakeFor(address recipient, uint256 amount) external",
  // Withdraw staked tokens
  "function withdraw(address recipient, uint256 amount) external",
  // Check staked balance
  "function getBalanceAt(address account, uint256 blockNumber) external view returns (uint256)",
  "function totalSupply() external view returns (uint256)",
  // Claim APYCloud yield rewards
  "function claimRewards(address[] memory tokens) external",
  "function stakingToken() external view returns (address)",
  "function rewardTokens(uint256 index) external view returns (address)",
  "event Staked(address owner, address indexed user, uint256 amount, uint256 indexed epoch)",
  "event Withdrawn(address indexed user, address recipient, uint256 amount, uint256 indexed epoch)",
];
```

**Key governance mechanic**: In `EnosysGovernorDual`, 1 APS = 10,000 HLN in voting weight (`manager1to0Rate = 10_000`). The Dual governor calls `addVoting` and `updateVoting` on both stake managers when proposals are created/resolved.

---

## Governance — EnosysGovernorDual / EnosysGovernorSingle

Addresses (Flare):
- Dual: `0xb03D96550Ab218161ECeccbd78A0EA136BA20B1c`
- Single: `0x00074DdA3EE851a69475b453Ab95C20b7a867325`

```javascript
const GOVERNOR_ABI = [
  "function propose(address[] memory targets, uint256[] memory values, bytes[] memory calldatas, string memory description) external returns (uint256 proposalId)",
  "function castVote(uint256 proposalId, uint8 support) external returns (uint256 weight)",
  "function castVoteWithReason(uint256 proposalId, uint8 support, string calldata reason) external returns (uint256 weight)",
  "function execute(address[] memory targets, uint256[] memory values, bytes[] memory calldatas, bytes32 descriptionHash) external payable returns (uint256 proposalId)",
  "function queue(address[] memory targets, uint256[] memory values, bytes[] memory calldatas, bytes32 descriptionHash) external returns (uint256 proposalId)",
  "function state(uint256 proposalId) external view returns (uint8)",
  // 0=Pending, 1=Active, 2=Canceled, 3=Defeated, 4=Succeeded, 5=Queued, 6=Expired, 7=Executed
  "function proposalVotes(uint256 proposalId) external view returns (uint256 againstVotes, uint256 forVotes, uint256 abstainVotes)",
  "function getVotes(address account, uint256 blockNumber) external view returns (uint256)",
  "function stakeManager0() external view returns (address)",
  "function stakeManager1() external view returns (address)",
  "event ProposalCreated(uint256 proposalId, address proposer, address[] targets, uint256[] values, string[] signatures, bytes[] calldatas, uint256 startBlock, uint256 endBlock, string description)",
  "event VoteCast(address indexed voter, uint256 proposalId, uint8 support, uint256 weight, string reason)",
];
```

---

## Universal Router

**What it is**: A batched router that can execute V2 swaps, V3 swaps, and other operations in a single transaction using a command-based system.

**Addresses:**
- Songbird: `0xfc737fa11f5c29072c1adaad599a3c42097a1972`
- Coston1: `0xc97ff9a440532c8324c83b74de79d2a470f6f0b5`
- Flare: (check latest deployment)

**Key commands** (from `Commands.sol`):
```javascript
const Commands = {
  V3_SWAP_EXACT_IN:  0x00,
  V3_SWAP_EXACT_OUT: 0x01,
  SWEEP:             0x04,
  TRANSFER:          0x05,
  PAY_PORTION:       0x06,
  V2_SWAP_EXACT_IN:  0x08,
  V2_SWAP_EXACT_OUT: 0x09,
  WRAP_ETH:          0x0b,
  UNWRAP_WETH:       0x0c,
  APPROVE_ERC20:     0x22,
};
// FLAG_ALLOW_REVERT = 0x80 — OR with command byte to make it non-reverting
```

```javascript
const UNIVERSAL_ROUTER_ABI = [
  "function execute(bytes calldata commands, bytes[] calldata inputs, uint256 deadline) external payable",
  "function execute(bytes calldata commands, bytes[] calldata inputs) external payable",
];
```

**Usage pattern**:
```javascript
// Example: V3 exact-input swap via Universal Router
const commands = "0x00"; // V3_SWAP_EXACT_IN
const V3_PATH = ethers.solidityPacked(
  ["address", "uint24", "address"],
  [WFLR_ADDRESS, 3000, EUSDT_ADDRESS]
);
const inputs = [
  ethers.AbiCoder.defaultAbiCoder().encode(
    ["address", "uint256", "uint256", "bytes", "bool"],
    [recipient, amountIn, amountOutMin, V3_PATH, payerIsUser]
  )
];
await universalRouter.execute(commands, inputs, deadline);
```

---

## RPC endpoints

Use the public Flare Foundation endpoints, or your own provider key (Ankr, etc.) for higher rate limits and larger `eth_getLogs` ranges:
- **Flare**: `https://flare-api.flare.network/ext/C/rpc`
- **Songbird**: `https://songbird-api.flare.network/ext/C/rpc`
- **Coston2**: `https://coston2-api.flare.network/ext/C/rpc`

---

## Pool Init Code Hash (for off-chain pool address computation)

**Coston2**: `0x0fcab4b6471d385aaf1a03c5e13c5904ba4796f7f48ac1b7893faee97ce7a2d9`

The pool init code hash is needed to compute pool addresses off-chain without an RPC call:
```javascript
// Off-chain pool address computation (same as Uniswap V3 CREATE2 pattern)
import { ethers } from "ethers";
function computePoolAddress(factory, tokenA, tokenB, fee, initCodeHash) {
  const [token0, token1] = tokenA.toLowerCase() < tokenB.toLowerCase()
    ? [tokenA, tokenB] : [tokenB, tokenA];
  const key = ethers.solidityPackedKeccak256(
    ["address", "address", "uint24"], [token0, token1, fee]
  );
  return ethers.getCreate2Address(factory, key, initCodeHash);
}
// NOTE: For Flare/Songbird mainnet, get the hash from the factory contract directly
// or use factory.getPool() — more reliable than hardcoding
```
