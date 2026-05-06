# Enosys Loans — Full Address Reference (Flare Mainnet)

Enosys Loans is a fork of Liquity V2 deployed on Flare. It mints the **CDP stablecoin** against
overcollateralized positions (Troves). Currently 2 collateral branches: FXRP and WFLR.

---

## Core Contracts

| Contract | Address |
|----------|---------|
| boldToken (CDP) | `0x6Cd3a5Ba46FA254D4d2E3C2B37350ae337E94a0F` |
| collateralRegistry | `0x9474206bc035D03d142264fd9913d1D51246d3AC` |
| hintHelpers | `0x0f493519aFF37a4Ec95f31301b7c4F987915Acb0` |
| multiTroveGetter | `0xb80C59BBCEB205bCA25Eea2e4221717f23b339d3` |
| liquidationBot | `0xCB5e11E03c76396ae0e05A328707F85A725F0a9C` |

Number of branches: 2

---

## Branch 0 — FXRP Collateral

Collateral token: FXRP = `0xAd552A648C74D49E10027AB8a618A3ad4901c5bE`

| Contract | Address |
|----------|---------|
| addressesRegistry | `0xE492569dbB3273548c96C89Ab934bDdc6217C6f7` |
| borrowerOperations | `0x18139E09Fb9a683Dd2c2df5D0edAD942c19CE912` |
| troveManager | `0xc46e7d0538494FEb82b460b9723dAba0508C8Fb1` |
| stabilityPool | `0x2c817F7159c08d94f09764086330c96Bb3265A2f` |
| activePool | `0x65C378Bf4A68491436C84d8Da020b14FEfE03D17` |
| defaultPool | `0xa942bEf831574CFf20595fcDE7a23bE443f08c8E` |
| collSurplusPool | `0x2D973436F1382c5CA363d9de17309dBF84EbC2B8` |
| sortedTroves | `0x45726B674F26c06aBa56FC6d954347998935C388` |
| priceFeed | `0xFc35d431Ce1445B9c79ff38594EF454618D2Ec49` |
| gasPool | `0xa9A22e8777D245bd2D9D5F8E187C18d181DB4C11` |
| troveNFT | `0x5734E64d12621d353772D05210675B17923F3ff6` |
| metadataNFT | `0x9C5cc5005BE3f632ba1aCa0be9E55228E3B19019` |
| fixedAssetReader | `0x13e991858Ce3D16a8D94450CA8537Ec67E8d67df` |
| nftWnatBalanceTracker | `0x0000000000000000000000000000000000000000` |

---

## Branch 1 — WFLR Collateral

Collateral token: WFLR = `0x1D80c49BbBCd1C0911346656B529DF9E5c2F783d`

| Contract | Address |
|----------|---------|
| addressesRegistry | `0xdb2e8D166762C7E70EA8F088B945D213AEB313fb` |
| borrowerOperations | `0x19b154D5d20126a77309ae01931645a135E4E252` |
| troveManager | `0xB6cB0c5301D4E6e227Ba490cee7b92EB954ac06D` |
| stabilityPool | `0x0Dd6daab4cB9A0ba6707Cf59DBfbc28cc33CA24A` |
| activePool | `0xE4Fc0543990128612d8112c90cdECc252165D255` |
| defaultPool | `0x5066D530DCE0444dADe724B49625dF7ebEf4dEC1` |
| collSurplusPool | `0xa27E53BDD29FF85E62220F7fB4F4BE4Ea735B2F7` |
| sortedTroves | `0xF6468BD666a3Ab1e2BA94778c016549faEc6D028` |
| priceFeed | `0xBCB5420d7F6346DB739a7C49FC90AB37553B68dc` |
| gasPool | `0x865a55aA934521C8C383dc1F4FD6Cb0d5782E9bD` |
| troveNFT | `0x9EC6e96C8A96083daAAa68016FFcEd3D0606D72E` |
| metadataNFT | `0x5aCDD3262584615B7679b725EE000Bcc59411df6` |
| fixedAssetReader | `0x910233804884f9B2Ee2575d7E32Fb5FA816cF891` |
| nftWnatBalanceTracker | `0xEb0Ca51C163fC74051e44a1389552a53f957d4E3` |

---

## Implementation Contracts (Proxy Targets)

| Contract | Address |
|----------|---------|
| activePool | `0xfa88E9034b82e396bC40b4D22227b84e2a4546C1` |
| addressesRegistry | `0xB78DC196A2F8D03149bfD5FA53581B7CA13cf8C0` |
| boldToken | `0x03Dc7269D65018516133987de01104cf2607c884` |
| borrowerOperations | `0xC0a9bd9E7308a4d2e0f7c95bF3e3cd398Ea77d4C` |
| borrowerOperationsBatchFacet | `0xeEA488A129565530963439d2edf439A569865690` |
| collSurplusPool | `0x4Dd58a65fb1037A52DaD0dcDF411040A6a01AF1F` |
| collateralRegistry | `0x9cbB9b5376F2ecF281ff66BBA8fE2d70f48138F4` |
| defaultPool | `0xe8F2113D8158e4D5A4D200eC70b0e64B1EcFfD82` |
| fixedAssetReader | `0xFD34C634BA20DbA996D715D994ed16134Cb13daf` |
| gasPool | `0xd924BB9ca23C0D6B8d7C9b9376885BBD618F72eD` |
| hintHelpers | `0x8F9f301B7EC1F79edc5EDf9552dA8171784E8e6e` |
| metadataNFT | `0xdA3d760aB334f9f9bD313aE090489eC9104a9156` |
| multiTroveGetter | `0x790b54F142F2060F4c543017cEf2f0bcefAc4E88` |
| nftWnatBalanceTracker | `0xeCB53365B8B37485D11db7CADef169C148d42212` |
| priceFeed | `0x68Aa9AA27cfA5f4Fa71b22E614efe408281e1295` |
| sortedTroves | `0xAaDc37946cFC37D63A194464F722527C44e69557` |
| stabilityPool | `0x37FfD5d66Fb182493C315B8b8f1C9D67DcF95B18` |
| troveManager | `0xf6069D1BF97f913038fB9de55a477e6C1086Ad4e` |
| troveManagerRedemptionLiquidationFacet | `0xbE42FE93F52987BCaDFfEaC6d20c87021f6a2837` |
| troveNFT | `0xd415149037A876Cd8d162Aa41f1c572964415682` |
| liquidationBot | `0x6f1ff30f6eca70280a6522883dcaf884909449ad` |

---

## Key Protocol Parameters (at launch)

| Parameter | FXRP Branch | WFLR Branch |
|-----------|-------------|-------------|
| Mint cap | $4,000,000 | $1,000,000 |
| Min CDP per position | $500 | $500 |
| Collateral | FXRP (bridged XRP 1:1) | WFLR (wrapped native FLR) |
| Oracle | FTSO (decentralized) | FTSO (decentralized) |

---

## How Loans Works (for Integration)

1. **Open a Trove**: Call `BorrowerOperations.openTrove(...)` with collateral and CDP amount desired.
   - User sets their own `annualInterestRate` — lower rates get redeemed first if CDP drops below $1.
2. **Trove = NFT**: Each trove is minted as an NFT in `TroveNFT` — trove ID is the NFT token ID.
3. **Mint CDP**: CDP stablecoin is minted to user's address.
4. **Stability Pool**: Users can deposit CDP into `StabilityPool` to earn liquidation gains + fees.
5. **Price feed**: `PriceFeed` reads from FTSO oracle to get collateral price in USD.
6. **WFLR delegation**: WFLR collateral in the WFLR branch is auto-delegated via `NftWnatBalanceTracker`.
7. **Redemptions**: If CDP trades below $1, arbitrageurs redeem CDP against lowest-rate troves.
8. **Liquidation**: If collateral ratio drops below minimum, `LiquidationBot` or anyone can liquidate.
