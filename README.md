# Flare Builders Toolkit

Comprehensive skill bundle for building, auditing, and operating production
contracts on the Flare-family chains (Flare, Songbird, Coston2, Coston). Every
skill in this toolkit is read-only documentation/reference — none of them
execute, sign, or broadcast transactions on your behalf.

**Security is the default.** Every skill in this toolkit assumes the contract
under discussion will be attacked. Patterns that aren't safe-by-default
(`Ownable` instead of `Ownable2Step`, raw `transfer` instead of `SafeERC20`,
unbounded loops, push-style ETH transfers) are flagged as findings, not
described as alternatives.

## Quick map

| Goal | Start here |
|---|---|
| Find an address (token, DEX, router, Permit2, Multicall3) | [`flare-network`](flare-network/) |
| Look up an RPC endpoint or block-explorer URL | [`flare-network`](flare-network/) |
| Verify a deployed contract on flare-explorer / flarescan | [`flare-network`](flare-network/) — "Verification command" |
| Understand what Flare's protocols do (FTSO, FDC, FAssets, Smart Accounts) | [`flare-general`](flare-general/) |
| Read FTSO price feeds, anchor feeds | [`flare-ftso`](flare-ftso/) |
| Cross-chain proofs / Flare Data Connector | [`flare-fdc`](flare-fdc/) |
| FAssets / FXRP minting + redemption | [`flare-fassets`](flare-fassets/) |
| ERC-4337 smart accounts on Flare | [`flare-smart-accounts`](flare-smart-accounts/) |
| Interact with Enosys DeFi suite (DEX V3, Loans/CDP, Governance) | [`enosys-contracts`](enosys-contracts/) |
| Build tools against Enosys V3 CLMM positions + reward managers | [`enosys-dex-v3`](enosys-dex-v3/) |
| Avoid known Flare gotchas (RPC limits, claim-OOG, MetaMask SDK hangs) | [`flare-dapp-pitfalls`](flare-dapp-pitfalls/) |
| Write production-grade Solidity | [`solidity`](solidity/) — Cyfrin's Solidity standards |
| Apply Flare-specific security checks before deploy | [`flare-security`](flare-security/) |
| Run a checklist-driven audit on a contract | [`audit`](audit/) |
| Run an adversarial multi-agent audit on a contract | [`audit-contract`](audit-contract/) |
| Optimize gas | [`gas-optimize`](gas-optimize/) |
| Generate a Foundry test suite | [`test-foundry`](test-foundry/) |
| Generate a Hardhat test suite | [`test-hardhat`](test-hardhat/) |

## How the toolkit fits together

The 16 skills fall into five layers. Use the layers below the one you're
working in as references; use the layer above when you need broader context.

```
┌─────────────────────────────────────────────────────────────┐
│  AUDIT WORKFLOW (run before every mainnet deploy)           │
│    • audit              (checklist, 115+ items)             │
│    • audit-contract     (adversarial, multi-agent)          │
│    • gas-optimize       (impact-ranked findings)            │
│    • test-foundry       (extend the suite to fuzz/inv/fork) │
│    • test-hardhat       (alternate path; this toolkit       │
│                          recommends Foundry)                │
├─────────────────────────────────────────────────────────────┤
│  FLARE-FLAVORED SECURITY                                    │
│    • flare-security     (defaults + Flare overlays:         │
│                          Permit2 / FoT / blacklist /        │
│                          basefee / FTSO proxy)              │
│    • flare-dapp-pitfalls (cross-project lessons learned)    │
├─────────────────────────────────────────────────────────────┤
│  CODING STANDARDS                                           │
│    • solidity           (Cyfrin Solidity standards;         │
│                          generic but rigorously applied)    │
├─────────────────────────────────────────────────────────────┤
│  PROTOCOLS (Flare-native + ecosystem)                       │
│    • flare-general          (chains, FSP, dev tooling)      │
│    • flare-ftso             (price feeds)                   │
│    • flare-fdc              (data connector)                │
│    • flare-fassets          (FXRP minting/redemption)       │
│    • flare-smart-accounts   (ERC-4337 on Flare)             │
│    • enosys-contracts       (Enosys DeFi addresses + ABIs)  │
│    • enosys-dex-v3          (Enosys V3 CLMM + rewards)      │
├─────────────────────────────────────────────────────────────┤
│  REGISTRY (concrete addresses, endpoints, token tables)     │
│    • flare-network                                          │
└─────────────────────────────────────────────────────────────┘
```

**A typical "build and ship a Flare contract" workflow uses all five layers:**

1. **Plan**: read [`flare-general`](flare-general/) + the protocol-specific
   skills you'll touch (`flare-ftso`, `flare-fassets`, etc.).
2. **Address resolution**: look up every concrete address in
   [`flare-network`](flare-network/).
3. **Write the contract**: apply [`solidity`](solidity/) Cyfrin standards as
   defaults; layer [`flare-security`](flare-security/) Part 1 patterns on top.
4. **Apply Flare-specific overlays**: review [`flare-security`](flare-security/)
   Part 2 — Permit2 chain availability, FoT detection, blacklist surface,
   basefee floor, etc. Add [`flare-dapp-pitfalls`](flare-dapp-pitfalls/) for
   the cross-project gotcha list.
5. **Test**: [`test-foundry`](test-foundry/) for unit + fork + fuzz coverage.
6. **Optimize**: [`gas-optimize`](gas-optimize/) for ranked savings.
7. **Audit**: [`audit`](audit/) (systematic) + [`audit-contract`](audit-contract/)
   (adversarial). Re-read [`flare-security`](flare-security/) Part 2 to verify
   nothing chain-specific slipped through.
8. **Deploy + verify**: deploy script (Foundry preferred); use the verification
   commands in [`flare-network`](flare-network/) to publish source on both
   `flare-explorer` (Blockscout) and `flarescan` (Routescan/Etherscan-style).

## Audit invocation

Both audit skills are designed to run against a specific Solidity file:

```
/audit src/MyContract.sol
/audit-contract src/MyContract.sol
```

The `audit` skill is a checklist methodology. The `audit-contract` skill is an
adversarial multi-agent system that auto-selects specialist attackers based on
the contract's surface and writes Foundry PoC tests for any CRITICAL / HIGH
findings it identifies.

For a combined "Full Audit" workflow that runs both plus gas optimization and
test extension against the same target, see your project's `CLAUDE.md` for
the project-specific orchestration. The toolkit's individual skills are the
building blocks.

**After running the generic audit skills, ALWAYS reread
[`flare-security`](flare-security/) Part 2.** The generic audits don't catch
Permit2 chain-availability, FoT detection requirements, blacklist surface, or
FTSO redistributor proxy upgradeability — those are Flare-specific and must
be verified separately.

## Pre-deploy security checklist (mandatory)

Reproduced verbatim from [`flare-security`](flare-security/) for fast lookup.
Don't ship to Flare mainnet without all green:

- [ ] `forge build` clean, all contracts under 24 KB runtime
- [ ] `forge test` (unit) + `forge test --fork-url flare` (fork) all pass
- [ ] `slither src/ --exclude-informational` no high or medium findings
- [ ] Owner is NOT the deployer EOA (`Ownable2Step` + Ledger / multisig handoff)
- [ ] `Ownable2Step` (not `Ownable`); reentrancy guards on every state-changing
      external function; CEI in every flow
- [ ] `SafeERC20` + `forceApprove` for every token interaction
- [ ] No `tx.origin` for auth; no unbounded loops over user-controlled data
- [ ] Pause / emergency-stop present and tested
- [ ] Custom errors prefixed with contract name + `__`
- [ ] Permit2 chain availability verified if your contract depends on Permit2
- [ ] FoT-token + blacklistable-token surface explicitly designed for
- [ ] Basefee floor present in any incentive-math
- [ ] `audit` + `audit-contract` both run; findings fixed or accepted with
      documented reasoning
- [ ] Contract verifies cleanly on both `flare-explorer` and `flarescan`

## Conventions

- **Skill folders** are flat (`flare-builders-toolkit/<skill-name>/SKILL.md`).
  Each `SKILL.md` has YAML frontmatter with `name` and `description` fields;
  the `name` is what the skill is invoked as.
- **Reference docs** (long-form supporting material) live alongside the
  `SKILL.md` in a `references/` subfolder when present.
- **Scripts** in some Flare-AI skills (`flare-fassets`, `flare-ftso`) are
  example TypeScript/Solidity files. Treat them as illustrative — you are
  responsible for security review before running anything that signs or
  broadcasts.

## Scope

This toolkit covers the Flare-family **EVM** surface — building and operating
smart contracts on Flare, Songbird, and Coston2. Non-EVM chains enter only
through `flare-fdc` (state proofs into Flare) and `flare-fassets` (XRP →
FXRP minting); for native interactions on those chains, use chain-specific
tooling.

It is documentation-and-reference only. None of the skills execute, sign, or
broadcast transactions on the user's behalf.

## Sources

- **Flare AI skills** (`flare-general`, `flare-ftso`, `flare-fdc`,
  `flare-fassets`, `flare-smart-accounts`) — Flare Foundation's official skill
  set.
- **Cyfrin Solidity standards** (`solidity`) — published by Cyfrin's security
  team.
- **Solidity Suite** (`audit`, `audit-contract`, `gas-optimize`, `test-foundry`,
  `test-hardhat`) — checklist + adversarial audit + gas + test scaffolds.
- **`flare-network`, `flare-security`** — source-of-truth registry +
  Flare-flavored security checklist.
- **`flare-dapp-pitfalls`** — cross-project lessons learned.
- **`enosys-contracts`, `enosys-dex-v3`** — Enosys protocol addresses + reward
  manager mechanics for integration work.
