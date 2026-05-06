# Enosys Contract Addresses — Coston2 Testnet (Chain ID: 114)

RPC: `https://coston2-api.flare.network/ext/C/rpc`  
Explorer: https://coston2-explorer.flare.network  
Faucet: https://faucet.flare.network (select Coston2, get C2FLR)

Use Coston2 for testing applications intended for Flare mainnet.

---

## Tokens

| Name | Symbol | Address |
|------|--------|---------|
| Wrapped C2FLR | Wnat | `0xc67dce33d7a8efa5ffeb961899c73fe01bce9273` |
| Test APS | tAPS | `0x86e944B8db65D5a7153c89fB0EC8E573Cac62676` |
| CDP (test) | CDP | `0xbb182afB633994B988b040fbE3862ff970DaaCB1` |
| tFXRP | tFXRP | `0xDd9815Fe3a4dd1b629a7457b32288C63B4320E44` |
| tstFXRP | tstFXRP | `0x36A752Ad2Fc41BD75db90B7856A1054cd7419441` |
| TokenA | TKNA | `0xa44F164844796826Bfe477C2Fb8088aED065305f` |
| BToken | BTKN | `0x2A807b1599eeD3B3C91DE5C2abeEb738EEE18D45` |
| texUSDT | texUSDT | `0x14565595F1Cdd211b3C041cFcBc80AF64c21C01D` |

---

## DEX V2

| Contract | Address |
|----------|---------|
| ProxyAdmin | `0x544A8FBa6feabB8647D09b4D0D21C9f21042238B` |
| EnosysDexFactory | `0x31BE10e58b03f6d99214dB7869F010a29D05B9Ed` |
| EnosysDexRouter | `0x948B78BE3771089693C68ED4E2ECEaB5565545Dd` |
| PairImplementation | `0x6DF7eC8482175Cf022D5Ae7B107288a457e608b2` |

### Sample Pairs (Coston2)
- TKNA-WC2FLR: `0x4F3ee383c3Bf1CE2727f754Df797aE143A7B6e77`
- BTKN-WC2FLR: `0x42567A17a065E0e5670533BE4C456eAE82460d27`
- texUSDT-WC2FLR: `0x87C9d18312666DDc2B78A958F114f1Eba094015a`

FTSO Redistributors:
| Contract | Address |
|----------|---------|
| WNatRegistry | `0x4bDcF740375298600367E15c07dD601a7593f1dd` |
| FtsoRewardRedistributor | `0xB739F2E36aB092d7A70E80E6115e2d485298b2Fa` |
| MonthlyAirdropRedistributor | `0x55f75F8c99411699BE9db13d14979E759921574c` |

External Flare:
| Contract | Address |
|----------|---------|
| FlareContractRegistry | `0xaD67FE66660Fb8dFE9d6b1b4240d8650e30F6019` |
| DistributionToDelegators | `0xbd33bdff04c357f7fc019e72d0504c24cf4aa010` |
| FtsoRewardManager | `0x16e6a0b7a26518c336bca0e935782c22a9ee07fd` |
| FtsoManager | `0xbb6d2b7db567bb65b93570e5bebf34a2b4a90cea` |
| FtsoRegistry | `0x48da21ce34966a64e267cefb78012c0282d0ac87` |

---

## DEX V3 (CLMM — Coston2)

| Contract | Address |
|----------|---------|
| EnosysDexV3Factory | `0x537279D95Dd98Ea5a5a4C24B523Df9959967A657` |
| SwapRouter | `0x5C0C46C61C7bfeE2A7a8dbCcd857C52Ed5bb0Ae3` |
| NonfungiblePositionManager | `0xD2fD55647A90fD1f2D071e115Bb713B3C145D5e2` |
| Quoter | `0x4269Cbc24B95e2351097d63B12CA9C4F23eB1D62` |
| QuoterV2 | `0x0e3e00f3a4BFe0a02cA90d54F053f7e9a24D1E13` |
| EpochManager | `0x9A61a670E03F140D7029c3eDC21F5Ea6369be7b1` |
| RewardManagerBeacon | `0xe92f2fC46F85ad50457c1facd783e0Af834d3397` |
| RewardManagerUpgradableFactory | `0x3289f0229F1272821417F636B17245eb4e4239ee` |
| ProxyAdmin / PoolsProxyAdmin | `0xAF832cb3BE0e953143A18b40905ce47618cD8e6A` |
| ProfitDistribution | `0x9D64670e09B52d14b36E291B02f2b6Ed5f7ad7Ff` |
| WNatRegistry | `0x840a78A217b0BB09f872d409E04690c58B6954DE` |
| FtsoRewardRedistributor | `0x88460936f55E75C9d6F67ae1449242AC5ad361C8` |
| MonthlyAirdropRedistributor | `0xc01A8a9f034fb068926C881BDD610d3790a01390` |
| Oracle | `0xf708Be808055aa3cD0520d10D352E8A1392f7086` |
| WNatBalanceHelper | `0x9385556B571ab92bf6dC9a0DbD75429Dd4d56F91` |
| PoolSnapshotHelper | `0x0Ec1314157A7202D08Ad014108750fcC3756C334` |

InitCodeHash: `0x0fcab4b6471d385aaf1a03c5e13c5904ba4796f7f48ac1b7893faee97ce7a2d9`

### Key Pools (Coston2)
- CDP/tFXRP/100: `0x0B9E176f5AE49074342C44fEAd29d48E69a3e574`
- CDP/Wnat/500: `0x7b9d21D5EbFDa561C24b13d33a4f243781aa308F`
- tAPS/Wnat/500: `0x7FF1be9528Ec0B5f1D451145279b6B748BB4B81B`

---

## APYCloud (Coston2)

| Contract | Address |
|----------|---------|
| ProxyAdmin | `0x6E8C3BCC1b4711e9dCC3F2f755B920548Bd8AeA8` |
| ProfitDistribution_80-20_DEX | `0xCF28Dfd4201bbcfed30E86d7171aF77CDCCAFee4` |
| ProfitDistribution_80-20_FtsoRewards | `0xAD308C28f34c36987b9f45333532185879Db4D11` |
| ProfitDistribution_50-50_SimpleStaking | `0xBE7Ebc08a1a9063a5b08109240b17CB41047DdC3` |

---

# Enosys Contract Addresses — Coston1 Testnet (Chain ID: 16)

RPC: `https://coston-api.flare.network/ext/C/rpc`  
Explorer: https://coston-explorer.flare.network  
Faucet: https://faucet.flare.network (select Coston, get CFLR)

Use Coston1 for testing applications intended for Songbird canary network.

---

## Tokens (Coston1)

| Name | Symbol | Address |
|------|--------|---------|
| Wrapped CFLR | Wnat | `0x767b25A658E8FC8ab6eBbd52043495dB61b4ea91` |
| ExFi (test) | EXFI | `0xC0848bA31e8df7A0535C75d2183aD1a3A0316756` |
| SparkFin (test) | SFIN | `0xa68df7AA04965F0Cc6b61a77da508af6e749AC42` |
| ExXDC (test) | WXDC | `0x2460Ebd7b0a4B019ebCE6bcF5a2F213BCdF10f48` |
| YUSD (test) | YUSD | `0x1e90ff56f900e1D8910695a5dfd4F787Fa1CB51F` |
| USDC (test) | USDC | `0xe1485a8Db4e7307788F16c29020aF2C5FCD01622` |
| etLINK (test) | etLINK | `0x20Ef7C1B6a62e5A00692285D46853ff24573DA39` |
| TRTX1 (test) | TRTX1 | `0xd96728C138c4719bFaE7F0f3eC9a27960A80f2Ba` |

---

## DEX V2 (Coston1)

| Contract | Address |
|----------|---------|
| Wnat | `0x767b25A658E8FC8ab6eBbd52043495dB61b4ea91` |
| ExfiProxy | `0xC0848bA31e8df7A0535C75d2183aD1a3A0316756` |
| SfinProxy | `0xa68df7AA04965F0Cc6b61a77da508af6e749AC42` |
| Kakeibo | `0x2f93A76eCB7cD88526c4161e25B6830F111a814A` |

---

## DEX V3 (Coston1)

| Contract | Address |
|----------|---------|
| EnosysDexV3Factory | `0xD89795D88783e61cb2378F7020f69a2129DaF6BB` |
| SwapRouter | `0xab9a5186284932f23c5Ec642BD7C19E58721f903` |
| NonfungiblePositionManager | `0xcEF460eE077674677C781D1cBbE7142b6c7a65F4` |
| Quoter | `0x99a75a7B90818dC33f2e9de3A668ac2Cd096642a` |
| QuoterV2 | `0xb9001934E27a9268D2E380c1bE0f13aa068ff554` |
| EpochManager | `0x48b6379a8230a72Abc2f6c23D589bEc13655C874` |
| RewardManagerBeacon | `0x42947b2075965d104E0e0f94cd373633192a449E` |
| RewardManagerUpgradableFactory | `0x2f149C900D1CFf2e6EA096D09EBbDD05a9aE8dB2` |
| ProxyAdmin | `0x715B0042874fcb0D316e72580859e29cB0D428f2` |
| PoolsProxyAdmin | `0x7F09C604Db6Eca575350fCA1e9278f7eE023EC81` |
| WNatRegistry | `0xee91ed566F175A749F75b3014ABc5fC29287a7ef` |
| FtsoRewardRedistributor | `0x42EB7C31F4F5c227f044ECEB6219f964648F5281` |
| Oracle | `0x90C3c78A206114D4613b3A1D71F4FE8033A44615` |

### Governance (Coston1)

| Contract | Address |
|----------|---------|
| EnosysGovernanceStakeManagerExfi | `0x81e0890d5bcB1909BC59D3ceb9133322cD6CD6E6` |
| EnosysGovernanceStakeManagerSfin | `0x059d531a9cc16c52c011994344B02507a22dC022` |
| EnosysGovernorSingle | `0x067a5356c122212ABB887A645a4aEe7b63f637E3` |
| EnosysGovernorDual | `0x98F8301E3273814c26b48d9c7C8841D914CD6caF` |
| ProxyAdmin | `0x2a36Ac7C355929F9958d5f56Be1B33C5158b6c38` |
