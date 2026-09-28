# cardano-preview-mesh-aiken-hello

**Community learning / scaffolding recipe** for a concrete **Preview**
walkthrough: Mesh tip / UTxO / ADA send, then Aiken **hello_world** (Plutus V3)
lock + unlock with Mesh + Blockfrost. Full practice tx hashes included.

**Who this is for:** builders who want a Preview-first hello path
after (or instead of) reading Mesh’s Aiken docs; maintainers packaging a clear
community recipe for peer review.

> **Not official Cardano, Mesh, or Aiken docs.**  
> **Not** an oracle kit. **Not** CIP-30. **Not** mainnet. **Not** production-audited.  
> **Docs-only / no CI** here (recipe of a past Preview practice).

## Verified scope (2026-09-27 PT)

| | |
|---|---|
| **Solid (practice, 2026-09-25 PT)** | Mesh tip / UTxO / **2 ADA send** on **Preview**; Aiken **hello_world** build (stdlib **v3**, Plutus V3); Mesh lock **5 ADA** + inline datum + collateral out; Mesh unlock with redeemer `Hello, World!` + owner signature (`valid_contract: true`) |
| **Solid (docs existence, re-fetched 2026-09-27 PT)** | https://meshjs.dev/aiken · https://meshjs.dev/aiken/getting-started · https://aiken-lang.org/installation-instructions |
| **Not verified** | CIP-30 browser wallet; oracle reference inputs / pull updates; price-checking validator; mainnet; any TVL / user counts |
| **Not claimed** | production-ready, battle-tested, audited, official, “ships a finished product” |

Exact pins: [`VERSIONS.md`](VERSIONS.md). Evidence matrix: [`STATUS.md`](STATUS.md).

### Practice transactions (Preview — full hashes)

| Step | Tx hash | Block | Explorer |
|------|---------|-------|----------|
| Lock | `ebdc1565c39b6d295736317634bcb019a65860ce787669058010b005f6dd569c` | 4697224 | [Cardanoscan Preview](https://preview.cardanoscan.io/transaction/ebdc1565c39b6d295736317634bcb019a65860ce787669058010b005f6dd569c) |
| Unlock | `b7e23631f73db4a5a7913001dbbfd7cb57ad48d63ef043c4bd9d71f7f524d975` | 4697227 | [Cardanoscan Preview](https://preview.cardanoscan.io/transaction/b7e23631f73db4a5a7913001dbbfd7cb57ad48d63ef043c4bd9d71f7f524d975) |

Script address (Preview): `addr_test1wp83hd4eqa05da4tumggd6vj668huhyvsurma72ejrvrg6q8urlm4`  
Validator hash: `4f1bb6b9075f46f6abe6d086e992d68f7e5c8c8707bef95990d83468`

**Mesh send note:** practice recorded a successful **2 ADA** Preview send, but
the full send tx hash was **not** persisted in the spike tree. Do not invent
one — cite send as verified-in-practice without a package artifact hash.

## Quick start (reading order)

1. Scope + honesty: this README
2. Everyday explanation: [`PLAIN-LANGUAGE.md`](PLAIN-LANGUAGE.md)
3. Copy-paste walkthrough: [`RECIPE.md`](RECIPE.md)
4. Pins + evidence: [`VERSIONS.md`](VERSIONS.md) · [`STATUS.md`](STATUS.md)
5. Peer review note: [`audits/peer-review-pass.md`](audits/peer-review-pass.md)

## Package contents

| Path | Role |
|------|------|
| `README.md` | This front door |
| `PLAIN-LANGUAGE.md` | Non-expert explanation of Mesh send + Aiken hello |
| `RECIPE.md` | Preview walkthrough (send → build → lock → unlock) |
| `VERSIONS.md` | Exact practice pins |
| `STATUS.md` | What was run vs not run; link checks |
| `SECURITY.md` | How to report problems |
| `audits/peer-review-pass.md` | Peer review pass (correctness / presentation) |
| `LICENSE` | MIT |

## How this differs from packaging-glue

Sibling package `cardano-packaging-glue-starter` maps **oracle + Aiken + Mesh**
roles and documents a packaging gap. **This package does not repeat that
narrative.** It is the **concrete hello recipe** (send + lock/unlock) that
packaging-glue *cites* as baseline evidence.

## Honesty about limits

This is a **learning scaffold** grounded in one Preview practice path at the
pins in `VERSIONS.md`. Signing used **MeshWallet + CLI payment skey**, not a
browser CIP-30 wallet. Prefer Preview / testnet framing. Re-run lock/unlock on
publish day if Aiken or Mesh pins drift.

## License

**MIT** — see [`LICENSE`](LICENSE). Copyright (c) 2026 Brady Sheldon.

## How to cite

- Package folder: `cardano-preview-mesh-aiken-hello`
- Doc verification date: **2026-09-27** (America/Los_Angeles)
- Practice run date: **2026-09-25** PT (lock/unlock ~01:33–01:34 UTC 2026-09-26 → evening 2026-09-25 PT)
- Network: **Cardano Preview** only
- Do not cite as “production Mesh/Aiken kit” or “oracle starter”
