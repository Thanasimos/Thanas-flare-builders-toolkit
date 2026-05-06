---
name: flare-builders-toolkit
description: >
  Master skill for building, testing, auditing, and deploying smart contracts on
  the Flare-family EVM chains (Flare, Songbird, Coston2). Use this skill as the
  entry point for any Flare-related development task. It bundles 16 specialized
  sub-skills covering: chain registry and addresses (Permit2, Multicall3, DEXes,
  routers, tokens), Flare protocols (FTSO, FDC, FAssets, Smart Accounts), Enosys
  contracts and rewards, security-first Solidity standards, Flare-specific audit
  checklists (Permit2 chain availability, fee-on-transfer detection,
  blacklistable token surface, basefee floors, FTSO redistributor proxy
  upgradeability), the cross-project lessons-learned from real Flare builds, and
  the audit/gas/test workflow (`audit`, `audit-contract`, `gas-optimize`,
  `test-foundry`, `test-hardhat`). Trigger on: any Flare/Songbird/Coston2
  development question, "how do I X on Flare", security review request, audit
  kickoff, contract verification on flare-explorer or flarescan, address lookup,
  RPC selection, FTSO reward claim, FAssets minting, Permit2 integration on
  Flare, or "I'm starting a new Flare contract — what do I need to know".
  Security is the default: any contract under discussion is assumed hostile-attacked.
---

# Flare Builders Toolkit — Master Skill

This skill is the **entry point** to a 16-skill bundle for Flare-family EVM
development. When invoked, route to the right sub-skill(s) based on what the
task needs.

**Security is the default.** Every contract under discussion is assumed
hostile-attacked. Patterns that aren't safe-by-default (`Ownable` instead of
`Ownable2Step`, raw `transfer` instead of `SafeERC20`, unbounded loops,
push-style ETH transfers, missing reentrancy guards) are flagged as findings,
not described as alternatives. Read [`flare-security`](flare-security/SKILL.md)
first for any contract design or review work.

## How to use this skill

When the user asks a Flare-development question, **route them to the most
specific sub-skill** that fits. Multiple sub-skills can apply to one question.
Don't try to answer from this skill alone — it's the router, not the source of
truth.

### Routing table

| User asks about… | Use sub-skill(s) |
|---|---|
| Chain ID, RPC URL, block explorer URL, faucet, faster RPC | [`flare-network`](flare-network/SKILL.md) |
| Token address (WFLR, WSGB, USDT0, USDC.e, sFLR, FXRP, HLN, APS, …) | [`flare-network`](flare-network/SKILL.md) |
| Permit2 / Multicall3 deployed address per chain | [`flare-network`](flare-network/SKILL.md) |
| Enosys V3 / SparkDEX V3.1 / SparkDEX V4 / BlazeSwap / OpenOcean address | [`flare-network`](flare-network/SKILL.md) |
| Contract verification on `flare-explorer` or `flarescan` | [`flare-network`](flare-network/SKILL.md) |
| What is FTSO / FDC / FAssets / Smart Accounts | [`flare-general`](flare-general/SKILL.md), then the specific protocol skill |
| Reading FTSO price feeds, anchor feeds, volatility | [`flare-ftso`](flare-ftso/SKILL.md) |
| Cross-chain proofs, BTC/XRPL state into Flare | [`flare-fdc`](flare-fdc/SKILL.md) |
| FXRP minting, redemption, agents, gasless payments | [`flare-fassets`](flare-fassets/SKILL.md) |
| ERC-4337 smart accounts on Flare | [`flare-smart-accounts`](flare-smart-accounts/SKILL.md) |
| Enosys DEX V3 NFT positions, reward managers, FlareDrops, rFLR, APS incentives | [`enosys-dex-v3`](enosys-dex-v3/SKILL.md) |
| Enosys Loans/CDP, governance staking, contract addresses | [`enosys-contracts`](enosys-contracts/SKILL.md) |
| Why X is silently failing on Flare (RPC log range, MetaMask SDK hang, claim OOG, basefee, EIP-3855, WFLR transfer hook) | [`flare-dapp-pitfalls`](flare-dapp-pitfalls/SKILL.md) |
| Solidity coding standards (general) | [`solidity`](solidity/SKILL.md) |
| Security review of a Flare contract before deploy | [`flare-security`](flare-security/SKILL.md) |
| Run a checklist-style audit against a contract | [`audit`](audit/SKILL.md) |
| Run an adversarial multi-agent audit against a contract | [`audit-contract`](audit-contract/SKILL.md) |
| Optimize gas | [`gas-optimize`](gas-optimize/SKILL.md) |
| Generate a Foundry test suite | [`test-foundry`](test-foundry/SKILL.md) |
| Generate a Hardhat test suite | [`test-hardhat`](test-hardhat/SKILL.md) |
| "I'm building a new Flare contract" | Sequence below ↓ |

### Standard flow for a new Flare contract

When the task is "build and ship a Flare contract", walk these in order. Read
the linked skills as you go — don't try to remember everything.

1. **Plan** — read [`flare-general`](flare-general/SKILL.md) plus any
   protocol-specific skills the contract will touch
   (`flare-ftso`, `flare-fdc`, `flare-fassets`, `flare-smart-accounts`,
   `enosys-contracts`, `enosys-dex-v3`).
2. **Address resolution** — every concrete address goes through
   [`flare-network`](flare-network/SKILL.md). Don't guess; don't trust
   training-data addresses; look them up.
3. **Coding standards** — [`solidity`](solidity/SKILL.md) Cyfrin standards as
   the baseline.
4. **Flare-specific security overlays** — [`flare-security`](flare-security/SKILL.md)
   covers what `solidity` doesn't: Permit2 chain availability, FoT detection,
   blacklist surface, basefee floors, FTSO redistributor proxy quirks. Add
   [`flare-dapp-pitfalls`](flare-dapp-pitfalls/SKILL.md) for the gotcha catalog.
5. **Test** — [`test-foundry`](test-foundry/SKILL.md) for unit + fork + fuzz
   coverage. (Hardhat alternative in [`test-hardhat`](test-hardhat/SKILL.md);
   Foundry is preferred.)
6. **Optimize gas** — [`gas-optimize`](gas-optimize/SKILL.md) for ranked
   savings.
7. **Audit** — [`audit`](audit/SKILL.md) (systematic, 115+ items) +
   [`audit-contract`](audit-contract/SKILL.md) (adversarial, multi-agent).
   After both run, **reread [`flare-security`](flare-security/SKILL.md) Part
   2** — Flare-specific overlays aren't in the generic audit checklists.
8. **Deploy + verify** — verification commands in
   [`flare-network`](flare-network/SKILL.md) work for both Blockscout
   (`flare-explorer`) and Routescan/Etherscan-style (`flarescan`).

### Pre-deploy checklist (mandatory)

Before broadcasting to Flare mainnet, every item below must be green. Full
detail in [`flare-security`](flare-security/SKILL.md).

- [ ] `forge build` clean; all contracts under 24 KB runtime
- [ ] `forge test` (unit) + `forge test --fork-url flare` (fork) all pass
- [ ] Slither no high/medium findings
- [ ] Owner is NOT the deployer EOA — `Ownable2Step` + Ledger/multisig handoff
- [ ] `Ownable2Step` (not `Ownable`); reentrancy guards on every state-changing
      external function; CEI in every flow
- [ ] `SafeERC20` + `forceApprove` for every token interaction
- [ ] No `tx.origin` for auth; no unbounded loops over user-controlled data
- [ ] Pause / emergency-stop present and tested
- [ ] Custom errors prefixed with contract name + `__`
- [ ] Permit2 chain availability verified if the contract depends on Permit2
- [ ] FoT and blacklistable-token surface explicitly designed for
- [ ] Basefee floor present in any incentive-math
- [ ] `audit` + `audit-contract` both run; findings fixed or accepted
- [ ] Contract verifies cleanly on both `flare-explorer` and `flarescan`

## Conventions

- **Sub-skill folders** are flat. Each contains a `SKILL.md` (frontmatter +
  body) plus optional `references/`, `scripts/`, or `*.md` supporting docs.
- **Protocol skills** (`flare-ftso`, `flare-fassets`) include reference
  scripts in TypeScript and Solidity. They're illustrative; the user is
  responsible for security review before signing or broadcasting.
- **Address registry** (`flare-network`) is the single source of truth for
  any deployed-contract or token address. If a sub-skill mentions a different
  address than `flare-network`, raise it as a discrepancy and verify.

## When NOT to use this skill

- For non-EVM chain interactions outside the FAssets / FDC bridge surface —
  use chain-specific tooling.
- For Flare validator-node operation or staking management — see Flare's
  validator docs.
- For executing transactions — this skill is documentation/reference only.
  Wallet signing happens in user-controlled environments.

## Contract under review?

If the user is asking you to review a specific contract, **start with
[`flare-security`](flare-security/SKILL.md)** to set the security baseline,
then run [`audit`](audit/SKILL.md) and [`audit-contract`](audit-contract/SKILL.md)
against the file. After those complete, reread `flare-security` Part 2 to
verify the Flare-specific overlays — Permit2 availability, FoT detection,
blacklist surface, basefee floor, FTSO redistributor handling — were caught.
The generic audit skills are not chain-aware.

For more detailed orientation, the [README.md](README.md) in this same
directory is a human-readable map of the toolkit.
