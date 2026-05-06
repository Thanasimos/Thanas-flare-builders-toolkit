# Enosys Contract Addresses — Songbird Canary Network (Chain ID: 19)

RPC: `https://songbird-api.flare.network/ext/C/rpc`  
Explorer: https://songbird-explorer.flare.network  
FlareContractRegistry: `0xaD67FE66660Fb8dFE9d6b1b4240d8650e30F6019`

Note: Songbird is the "Experimental Finance" canary network. It has real economic value (SGB token).
Enosys tests new features here before deploying to Flare mainnet.

---

## Tokens

| Name | Symbol | Address |
|------|--------|---------|
| Wrapped SGB | WSGB (wnat) | `0x02f0826ef6aD107Cfc861152B32B52fD11BaB9ED` |
| ExFi | EXFI | `0xC348F894d0E939FE72c467156E6d7DcbD6f16e21` |
| SparkFin | SFIN | `0x0D94e59332732D18CF3a3D457A8886A2AE29eA1B` |
| ExXDC | exXDC | `0xC54De96302981f87a6F80E4607593681B44d670c` |
| Flare XRP (SGB) | FXRP | `0xF9a84f4ec903F4EaB117A9c1098BeC078BA7027d` |
| ExETH | exETH | `0x36B84a93820175c02F57E5c87a7Ea33Df58E5c94` |
| ExUSDT | exUSDT | `0x1a7b46656B2b8b29B1694229e122d066020503D0` |
| USDX | USDX | `0x4A771Cc1a39FDd8AA08B8EA51F7Fd412e73B3d2B` |
| FDOGE | FDOGE | `0xaa25ee3B68c515e69A463876Ab262bc4e8339030` |
| CAND (legacy) | candProxy | `0x70Ad7172EF0b131A1428D0c1F66457EB041f2176` |

---

## DEX V2

| Contract | Address |
|----------|---------|
| wnat | `0x02f0826ef6aD107Cfc861152B32B52fD11BaB9ED` |
| candProxy | `0x70Ad7172EF0b131A1428D0c1F66457EB041f2176` |
| exfiProxy | `0xC348F894d0E939FE72c467156E6d7DcbD6f16e21` |
| sfinProxy | `0x0D94e59332732D18CF3a3D457A8886A2AE29eA1B` |
| exXdc | `0xC54De96302981f87a6F80E4607593681B44d670c` |
| dFLR | `0x6f1Be01f9cD0c14E38f94E81Cb281ecB98Cc6A9b` |
| kakeiboProxy | `0xEf0fCdF375AD1bd81529e77d5326434079582B55` |

External references:
| Contract | Address |
|----------|---------|
| FtsoRewardManager | `0xc5738334b972745067fFa666040fdeADc66Cb925` |
| FtsoManager | `0xbfA12e4E1411B62EdA8B035d71735667422A6A9e` |
| FtsoRegistry | `0x6D222fb4544ba230d4b90BA1BfC0A01A94E6cB23` |

---

## DEX V3 (CLMM)

| Contract | Address |
|----------|---------|
| EnosysDexV3Factory | `0x416F1CcBc55033Ae0133DA96F9096Fe8c2c17E7d` |
| SwapRouter | `0x51bB58357c81523Dc7fc9D05f0C5921b122EE114` |
| NonfungiblePositionManager | `0xFC02eB1Ab5079627103357BfbB8167d3BBAc6f1e` |
| Quoter | `0x57fE57cd61FB6Ad44b87B2fA100e432b644b324B` |
| QuoterV2 | `0x4855f257B4BD4e5724E39D3938128Cd363EDD6C7` |
| EpochManager | `0xDD79E5b82948103273F6d63d566d97472a16f9e8` |
| RewardManagerBeacon | `0x92ee2471A5564af3F7BDEDE3CD73C8F2E544CCaF` |
| RewardManagerUpgradableFactory | `0x477B7dF75860D4a6857b1D1F704A7A25D5da4450` |
| RewardManager (active) | `0xa997E5FDd464b982d1Fbe51D8734A410744F3B7E` |
| ProxyAdmin | `0x3aa48b3127E3E05d2871576087d81d53E840B499` |
| PoolsProxyAdmin | `0x56035A7Ec075e45830311e550a56a261009FA2c4` |
| ProfitDistribution | `0x16bfB7Db0b4208dc72B918849108e56714430D58` |
| WNatRegistry | `0x1E3838C5Dc72B9a4f1D76E414e609652594d6096` |
| FtsoRewardRedistributor | `0x421294D3eb38c87390fc7f1442623f5b7DBc5b86` |
| DelegationHelper | `0xF7b5d3235C505e7D7C97175C036C92e27426971e` |
| EnosysDexV3InterfaceMulticall | `0x07D51F4d6EA34C7e3bb7A847a45039074c37c805` |

### Key Pools (Songbird)
- Wnat/EXFI/3000: `0x296D52ac39562458E4C0B42437eCA2dB9F750d0e`
- Wnat/exUSDT/3000: `0xA54b46E5Fd00FeaEa401eDd48E274B2bC2dd4Fea`
- Wnat/SFIN/3000: `0xbB3979Bf08821b9975A52A8214BC6f6ABFf18A2e`
- Wnat/FXRP/3000: `(see DEXV3_addresses_songbird.json)`

---

## Governance & APYCloud

| Contract | Address |
|----------|---------|
| EnosysGovernanceStakeManagerExfi | `0xc4D89F8f593F215f3636793E3353A36C196Cf87A` |
| EnosysGovernanceStakeManagerSfin | `0xA83E90337e2711b1c84df0AD7428403dBd0ce730` |
| EnosysGovernorSingle | `0x16DDC8552E71A5D83c56f3718A9861bCefDee9A7` |
| EnosysGovernorDual | `0x4EAA73fF20368cbEAC42F2D4406daf886479f831` |
| ProxyAdmin (Gov) | `0xEE8Cc71c23556B17e23218CDa507350C36879677` |
| ProfitDistribution_80-20_Dex | `0x16bfB7Db0b4208dc72B918849108e56714430D58` |
| ProfitDistribution_50-50_FtsoRewards | `0x95400A29408f6Bed0D045E9E88280cf52B4360A1` |
| ProxyAdmin (APYCloud) | `0x751882FDBCD75E20F5cA08a1Ec715810b1d442A6` |
