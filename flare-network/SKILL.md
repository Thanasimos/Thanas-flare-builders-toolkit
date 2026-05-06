---
name: flare-network
description: >
  Concrete deployment registry for the Flare-family chains (Flare, Songbird, Coston2,
  Coston). Use when you need: a chain ID, an RPC URL, a block explorer URL, a token
  address (WFLR, USDT0, sFLR, FXRP, USDC, etc.), or the address of an ecosystem
  contract (Permit2, Multicall3, Enosys V3 / SparkDEX V3.1 / SparkDEX V4 / BlazeSwap
  / OpenOcean factories/routers/quoters/position-managers). Trigger on any mention of:
  "what's the address of X on Flare", "RPC for Songbird", "explorer API for Coston2",
  "is Permit2 deployed on Songbird", "WFLR address", "Multicall3 on Flare", "verify
  contract on flarescan", or any chain-info / address-lookup question. Pair with
  `flare-general` for protocol/architecture descriptions; this skill is the addresses-
  and-endpoints registry.
---

# Flare Network — Deployment Registry

Authoritative addresses + endpoints for the Flare-family chains. **For protocol
architecture (FTSO, FDC, FAssets, Smart Accounts), see the `flare-general` skill.**
This skill answers "WHERE is X" questions — concrete addresses, RPC URLs, explorer
endpoints, and verification commands.

If you're unsure whether something is in scope: this skill is for *deployed
contracts* and *network endpoints*. Anything about how a protocol works lives in
the protocol-specific skills (`flare-ftso`, `flare-fassets`, etc.).

## Chain matrix

| Chain | ID | Type | Native | Wrapped Native | Block Time |
|---|---|---|---|---|---|
| **Flare** | 14 | Mainnet | FLR | WFLR | ~2s |
| **Songbird** | 19 | Canary mainnet (real $) | SGB | WSGB | ~2s |
| **Coston2** | 114 | Flare testnet | C2FLR | WC2FLR | ~2s |
| **Coston** | 16 | Songbird testnet | CFLR | WCFLR | ~2s |

## RPC endpoints

### Public RPCs (rate-limited, 30-block `eth_getLogs` cap)

```
flare:    https://flare-api.flare.network/ext/C/rpc
songbird: https://songbird-api.flare.network/ext/C/rpc
coston2:  https://coston2-api.flare.network/ext/C/rpc
coston:   https://coston-api.flare.network/ext/C/rpc
```

### Faster / paid alternatives

| Provider | Notes |
|---|---|
| **Ankr (paid tier)** | `https://rpc.ankr.com/flare/<KEY>` — domain-locked browser keys (origin-restricted); separate IP-locked key needed for server-side scripts/forge. Lifts `eth_getLogs` to ~5k blocks. |
| **REDACTED mirror** | `https://REDACTED-RPC/ext/bc/C/rpc` — community-run Flare-only mirror, useful when public RPC rate-limits hit during Foundry fork tests. |

**Origin lock note** — a browser-locked Ankr key returns `Origin not allowed (-32079)` to curl / forge-script. Use the public RPC for deploys; reserve Ankr for browser code.

## Block explorers

### Flare

| Explorer | API | Verifier type |
|---|---|---|
| `https://flare-explorer.flare.network` (Blockscout) | `https://flare-explorer.flare.network/api/` | `blockscout` |
| `https://flarescan.com` (Routescan-backed) | `https://api.routescan.io/v2/network/mainnet/evm/14/etherscan` | `etherscan` |

### Songbird

| Explorer | API | Verifier type |
|---|---|---|
| `https://songbird-explorer.flare.network` | `https://songbird-explorer.flare.network/api/` | `blockscout` |
| `https://songbird.flarescan.com` | `https://api.routescan.io/v2/network/mainnet/evm/19/etherscan` | `etherscan` |

### Coston2

| Explorer | API | Verifier type |
|---|---|---|
| `https://coston2-explorer.flare.network` | `https://coston2-explorer.flare.network/api/` | `blockscout` |
| `https://coston2.flarescan.com` | `https://api.routescan.io/v2/network/testnet/evm/114/etherscan` | `etherscan` |

### Verification command (Foundry)

Both explorer families work via `forge verify-contract`:

```bash
# Blockscout (no API key needed)
forge verify-contract \
  --rpc-url flare \
  --verifier blockscout \
  --verifier-url "https://flare-explorer.flare.network/api/" \
  --num-of-optimizations 200 --compiler-version 0.8.24 \
  --constructor-args $ENCODED_ARGS \
  $ADDRESS \
  src/MyContract.sol:MyContract

# Routescan / Etherscan-style (use literal "verifyContract" as the key)
forge verify-contract \
  --verifier etherscan \
  --verifier-url "https://api.routescan.io/v2/network/mainnet/evm/14/etherscan" \
  --etherscan-api-key "verifyContract" \
  --num-of-optimizations 200 --compiler-version 0.8.24 \
  --constructor-args $ENCODED_ARGS \
  $ADDRESS \
  src/MyContract.sol:MyContract
```

Constructor args go in via `--constructor-args 0x...`. Get them from the broadcast log:
`broadcast/Deploy.s.sol/<chainId>/run-latest.json` → `transactions[i].arguments`,
then `cast abi-encode "constructor(...)" <args...>` to encode.

## Cross-chain infrastructure

### Permit2 (Uniswap canonical)

`0x000000000022D473030F116dDEE9F6B43aC78BA3` — **deployed on Flare ONLY** (chain 14).
NOT deployed on Songbird, NOT deployed on Coston2. Verify via:

```bash
cast code 0x000000000022D473030F116dDEE9F6B43aC78BA3 --rpc-url <chain>
# Returns full bytecode on Flare; returns 0x on Songbird/Coston2
```

Implication: Permit2-dependent contracts (batch sweeps, signed-permit flows) cannot
be canary-tested on Songbird. Either deploy your own Permit2 fork there (~3000
LOC), use mocks, or skip Songbird and validate via Flare fork tests only.

### Multicall3

`0xcA11bde05977b3631167028862bE2a173976CA11` on **all** Flare-family chains (Flare,
Songbird, Coston2). Standard Multicall3 ABI. Useful for batched RPC reads.

When defining a custom chain in viem, you must explicitly register `contracts.multicall3`
or `publicClient.multicall(...)` will throw silently:

```ts
defineChain({
  id: 14,
  name: 'Flare',
  // ...
  contracts: {
    multicall3: {
      address: '0xcA11bde05977b3631167028862bE2a173976CA11',
      blockCreated: 3002461,
    },
  },
})
```

## Tokens

Addresses are case-insensitive on the EVM but stored canonical-checksummed below.

### Flare (chain 14)

| Symbol | Address | Decimals | Notes |
|---|---|---|---|
| **WFLR** | `0x1D80c49BbBCd1C0911346656B529DF9E5c2F783d` | 18 | Wrapped native; FTSO delegation hooks (see `flare-dapp-pitfalls`) |
| **sFLR** | `0x12e605bc104e93B45e1aD99F9e555f659051c2BB` | 18 | Liquid-staked FLR |
| **USDT0** | `0xe7cd86e13AC4309349F30B3435a9d337750fC82D` | 6 | Tether's Flare deployment; **blacklistable** |
| **USDC.e** | `0xfbDa5F676cB37624f28265A144A48B0d6e87d3b6` | 6 | LayerZero-bridged USDC; **blacklistable** |
| **FXRP** | `0x9b27dF2BC1AC0F5dE8f43eF1e7df45f5be17Bf08` | 6 | FAssets-bridged XRP (see `flare-fassets`) |
| **HLN** | `0x140Df95E16D9eF21BD4d97c8E5A1bD4D04d23739` | 18 | Enosys governance token |
| **APS** | `0x6b8758DB1148Cb5e8A6c3D3a2c34a4eFf5Ed0e80` | 18 | Enosys auto-pool stake |
| **eUSDT** | `0x96B41289D90444B8adD57e6F265DB5aE8651DF29` | 6 | Enosys-wrapped USDT |
| **rFLR** | `0x26d460c3Cf931Fb2014FA436a49e3Af08619810e` | 18 | Reward FLR (FlareDrops) |

### Songbird (chain 19)

| Symbol | Address | Decimals | Notes |
|---|---|---|---|
| **WSGB** | `0x02f0826ef6aD107Cfc861152B32B52fD11BaB9ED` | 18 | Wrapped native |
| **USDT_SG** | `0x99e1A4329afA76e0fC34C9bd56b016A509ECD37e` | 6 | Songbird USDT variant |

### Coston2 (chain 114, testnet)

| Symbol | Address | Decimals |
|---|---|---|
| **WC2FLR** | `0x767eecF7c0fa1d0F9CC0D75D6b5Bb2B8C0CF8A4b` | 18 |

Token lists are not exhaustive — for the long tail (airdrops, dust), discover via
the explorer's `account.tokenlist` API. For Flare:

```bash
curl -s "https://flare-explorer.flare.network/api?module=account&action=tokenlist&address=$WALLET"
```

## Concentrated-liquidity DEXes

Three CLMM families on Flare today. Picking the right one depends on whether you
need V3-shape (`getPool(t0,t1,fee)`) or Algebra-shape (`poolByPair(t0,t1)`) ABI.

### Enosys V3 (Uniswap V3-compatible) — Flare

| Contract | Address |
|---|---|
| Factory | `0x17AA157AC8C54034381b840Cb8f6bf7Fc355f0de` |
| NonfungiblePositionManager | `0xD9770b1C7A6ccd33C75b5bcB1c0078f46bE46657` |
| SwapRouter | `0x5FD34090E9b195d8482Ad3CC63dB078534F1b113` |
| QuoterV2 | `0x0A32EE3f66cC9E68ffb7cBeCf77bAef03e2d7C56` |
| FtsoRewardRedistributorForNft (FTSO only) | `0x5a0BfF8Ff1AF1DF28619Fce57d07E3bBb7BAF3d7` |
| FlaresRewardsRedistributorForNft (legacy, pre-2026-04) | `0x4410B821Fa1041D242f2C199C0EA78E4A5f15F19` |
| FtsoRewardRedistributorForAddress (EOA delegations) | `0x45aAa2e37B89f7514DF69FA084f381C67891C381` |

**FTSO redistributor naming gotcha (Flare):** the active NFT redistributor was rotated 2026-04
from `FlaresRewardsRedistributorForNft` (`0x4410B821…`) to `FtsoRewardRedistributorForNft`
(`0x5a0BfF8F…`). Same ABI shape; only the address changed. Rewards accrued under the legacy
contract are still claimable via direct calls — see `enosys-dex-v3` for the address-allowlist
recovery pattern.

### Enosys V3 — Songbird

| Contract | Address |
|---|---|
| Factory | `0x416F1CcBc55033Ae0133DA96F9096Fe8c2c17E7d` |
| NonfungiblePositionManager | `0xFC02eB1Ab5079627103357BfbB8167d3BBAc6f1e` |
| SwapRouter | `0x51bB58357c81523Dc7fc9D05f0C5921b122EE114` |
| QuoterV2 | `0x4855f257B4BD4e5724E39D3938128Cd363EDD6C7` |
| FtsoRewardRedistributorForNft | `0x421294D3eb38c87390fc7f1442623f5b7DBc5b86` |

### SparkDEX V3.1 (Uniswap V3-compatible, no FTSO redistributor) — Flare

| Contract | Address |
|---|---|
| Factory | `0x8A2578d23d4C532cC9A98FaD91C0523f5efDE652` |
| NonfungiblePositionManager | `0xEE5FF5Bc5F852764b5584d92A4d592A53DC527da` |
| SwapRouter | `0x8a1E35F5c98C4E85B36B7B253222eE17773b2781` |
| QuoterV2 | `0x5B5513c55fd06e2658010c121c37b07fC8e8B705` |

LP fees still accrue; FTSO delegation rewards do not (no redistributor deployed for
this fork). Position NFT lifecycle is otherwise identical to Enosys V3.

### SparkDEX V4 (Algebra V1.9+ with plugins, dynamic fees) — Flare

| Contract | Address |
|---|---|
| Factory | `0x805488DaA81c1b9e7C5cE3f1DCeA28F21448EC6A` |
| NonfungiblePositionManager | `0x49BE8AA6c684b15e0C5450e8Fa0b16Bec1435596` |
| SwapRouter | `0x69D57B9D705eaD73a5d2f2476C30c55bD755cc2F` |
| QuoterV2 | `0x6AD6A4f233F1E33613e996CCc17409B93fF8bf5f` |
| Default pool deployer | `0x59a662Ed724F19AD019307126CbEBdcF4b57d6B1` |

Algebra differs from V3 in three places that matter when integrating:
1. Pool resolution — `factory.poolByPair(token0, token1)` (no fee tier).
2. Pool state — `globalState()` returns 6 fields (V3's `slot0()` returns 7), but
   `tick` is at index 1 in both.
3. `MintParams` has `address deployer` where V3 has `uint24 fee`. Pass `address(0)`
   to use the factory's default deployer (current SparkDEX V4 pattern). For
   non-default-deployer pools, you must pass the actual deployer address; verify by
   computing `factory.computePoolAddress(t0, t1)` matches the pool you're targeting.

## V2 routers (Uniswap V2-compatible)

### Flare

| Router | Address | Selector for `swapExactTokensForTokens` |
|---|---|---|
| **Enosys V2 Router** | `0x088EeCB467B3968Da36c71F05023A1d3133B2B83` | `0x38ed1739` |
| **BlazeSwap Router** | `0xe3A1b355ca63abCBC9589334B5e609583C7BAa06` | `0x38ed1739` |

**BlazeSwap quirk** — its `swapExactTokensForTokens` checks `amountOutMin` against
the **actual recipient balance change**, not the `getAmountsOut` quote. For
fee-on-transfer tokens this means a quote-derived minOut will trip
`BlazeSwapRouter: INSUFFICIENT_OUTPUT_AMOUNT`. See `flare-security` for the
double-FoT-aware integration pattern.

## Aggregators

### OpenOcean

| Chain | Exchange contract |
|---|---|
| Flare | `0x6352a56caadC4F1E25CD6c75970Fa768A3304e64` |

Function selector for `swap`: **`0x90411a32`** (signature
`swap(address,(SwapDescription),(uint256,uint256,uint256,bytes)[])`). Do NOT guess
this from the human-readable signature — verify against the actual returned
calldata from the OO API:

```bash
curl -s '<OO API endpoint>' | jq -r '.data.data[2:10]'
```

OO API endpoint: `https://open-api.openocean.finance/v3/{chainId}/swap_quote`. Free,
unauthenticated, ~$0.01 minimum-USD-value gate per quote (legs below that get
rejected — fall back to V2 routers for dust).

## Cross-chain bridges

### LayerZero

LayerZero V2 endpoints are deployed on Flare and Songbird. For up-to-date contract
addresses, see <https://docs.layerzero.network/v2/deployments>.

### Stargate

Stargate v2 has Flare deployments for USDC and USDT0 routes. See
<https://stargateprotocol.gitbook.io/stargate>.

## Faucets (testnet only)

| Chain | Faucet |
|---|---|
| Coston2 (C2FLR) | <https://coston2-faucet.flare.network> |
| Coston (CFLR) | <https://coston-faucet.flare.network> |

## Common command snippets

### Read deployed bytecode (verify a contract exists at an address)

```bash
cast code $ADDRESS --rpc-url flare
# returns 0x if no contract; full bytecode if deployed
```

### Find deployed function selectors in a contract's dispatch table

```bash
cast code $ADDRESS --rpc-url flare | grep -oE '63[0-9a-f]{8}1461' | head
# 63XXXXXXXX14 = PUSH4 selector + EQ — that's the dispatcher's selector check
```

### Decode a 4-byte selector into a function signature

```bash
cast 4byte 0x90411a32
# returns swap(address,(address,address,address,address,uint256,uint256,uint256,bytes),(uint256,uint256,uint256,bytes)[])
```

### Pull constructor args from a Foundry broadcast log

```bash
node -e "console.log(JSON.stringify(require('./broadcast/Deploy.s.sol/14/run-latest.json').transactions.find(t => t.contractName === 'MyContract').arguments))"
```

### Encode constructor args for verification

```bash
cast abi-encode "constructor(address,address)" 0xabc... 0xdef...
```

## Foundry config (recommended for Flare-family work)

```toml
# foundry.toml
[profile.default]
src = "src"
out = "out"
libs = ["lib"]
optimizer = true
optimizer_runs = 200
solc_version = "0.8.24"
via_ir = true                  # required for stack-too-deep on complex contracts
fs_permissions = [{ access = "read", path = "config/" }]

[rpc_endpoints]
flare      = "https://flare-api.flare.network/ext/C/rpc"
flare_fast = "https://REDACTED-RPC/ext/bc/C/rpc"
songbird   = "https://songbird-api.flare.network/ext/C/rpc"
coston2    = "https://coston2-api.flare.network/ext/C/rpc"
```

## When to use other skills

- **Protocol architecture** (FTSO, FDC, FAssets, Smart Accounts, FCC/TEE) — `flare-general`, then the protocol-specific skill.
- **FTSO price feeds, anchor feeds, volatility incentives** — `flare-ftso`.
- **Flare Data Connector (cross-chain proofs)** — `flare-fdc`.
- **FAssets / FXRP minting + redemption** — `flare-fassets`.
- **ERC-4337 smart accounts on Flare** — `flare-smart-accounts`.
- **Enosys-specific contracts** (DEX V3 reward managers, Loans/CDP, governance) — `enosys-contracts`, `enosys-dex-v3`.
- **Cross-project gotchas** (RPC log range, EIP-3855, MetaMask SDK hangs, claim-OOG) — `flare-dapp-pitfalls`.
- **Flare-specific security review** of a deployment — `flare-security`.
- **Generic Solidity standards** — `solidity` (Cyfrin).
- **Auditing workflow** — `audit` (checklist) and `audit-contract` (adversarial multi-agent).
