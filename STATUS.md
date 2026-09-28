# STATUS — verification log (publish package)

**As-of:** 2026-09-27 ~9:10 PM PT (publish-day URL re-check)  
**Practice runs:** 2026-09-25 PT (Mesh send + Aiken hello lock/unlock)  
**Label:** community learning / scaffolding recipe — **Preview** framing  
**Not claimed:** battle-tested, production-ready, audited, mainnet, official,
CIP-30, live oracle

## Matrix

| Check | Result | Notes |
|-------|--------|-------|
| Mesh tip / UTxO on Preview | **PASS** (practice) | `mesh-hello/`; spike `STATUS.md` |
| Mesh 2 ADA send on Preview | **PASS** (practice) | Full send tx hash **not** in spike tree |
| Aiken hello build (Plutus V3, stdlib v3) | **PASS** (practice) | Aiken `v1.1.23` |
| Lock 5 ADA + inline datum + collateral | **PASS** | Tx `ebdc1565c39b6d295736317634bcb019a65860ce787669058010b005f6dd569c` block 4697224 |
| Unlock / `valid_contract: true` | **PASS** | Tx `b7e23631f73db4a5a7913001dbbfd7cb57ad48d63ef043c4bd9d71f7f524d975` block 4697227 |
| CIP-30 browser wallet path | **NOT RUN** | MeshWallet + CLI skey only |
| Oracle read in unlock | **NOT RUN** | Out of scope (see packaging-glue sibling) |
| Price-checking Aiken validator | **NOT RUN** | |
| Mainnet | **NOT RUN** | |
| TVL / users | **NOT CLAIMED** | Unknown |

## External URL re-fetch (2026-09-27 ~9:10 PM PT)

| URL | Result |
|-----|--------|
| https://meshjs.dev/aiken | **200** |
| https://meshjs.dev/aiken/getting-started | **200** |
| https://aiken-lang.org/installation-instructions | **200** |
| https://docs.cardano.org/cardano-testnets/tools/faucet | **200** |
| https://faucet.preview.world.dev.cardano.org/basic-faucet | **200** (Preview faucet UI) |
| https://preview.cardanoscan.io/transaction/ebdc1565…569c (lock) | **403** to bots — OK; human-verify explorer |
| https://preview.cardanoscan.io/transaction/b7e23631…d975 (unlock) | **403** to bots — OK; human-verify explorer |

Pins in `VERSIONS.md` **unchanged** as of this re-check (2026-09-27 evening PT).

## Artifacts in this folder

| Path | Role |
|------|------|
| `README.md` | Front door / scope |
| `PLAIN-LANGUAGE.md` | Non-expert explanation |
| `RECIPE.md` | Copy-paste Preview walkthrough |
| `VERSIONS.md` | Exact practice pins |
| `STATUS.md` | This evidence log |
| `SECURITY.md` | How to report problems |
| `audits/peer-review-pass.md` | Peer review pass (correctness / presentation) |
| `LICENSE` | MIT |

## Secrets

No wallet seeds, mnemonics, private keys, or Blockfrost project ids in this
folder. Practice addresses / lock-unlock tx hashes below are **public Preview**
artifacts only.

## Publishing stance

Community learning package under **MIT**. Docs-only / no CI. No upstream issues
are filed from this package unless the maintainer asks.

**Sibling note:** `cardano-packaging-glue-starter` and
`orcfax-preview-consume-sketch` are separate sibling packages (named in prose
only; not required to use this recipe).

**Published:** 2026-09-27 PT — public MIT under booper-labs (first push).
