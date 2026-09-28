# VERSIONS — what we actually tested / pinned

**Scope:** honest pin list for the Preview Mesh + Aiken hello practice path.  
**Do not assume** these pins match every Mesh / Aiken release. Re-check on the
day you publish or re-run lock/unlock.

**As-of:** 2026-09-27 evening PT — publish-day re-check confirmed pins below
**unchanged** (no pin bumps this pass).

| Component | Version / value | Notes |
|-----------|-----------------|-------|
| Network | **Cardano Preview** | On-chain practice claims only |
| Provider | **Blockfrost Preview** | Project id never committed |
| Node.js | **v20.19.2** | Mesh docs say 18+; practice used 20 |
| TypeScript | **5.8.x** (`^5.8.3`) | Pinned for `ts-node` |
| `@meshsdk/core` | **1.9.1** (`^1.9.1`) | mesh-hello + aiken-hello offchain |
| Aiken CLI | **v1.1.23+8949565** | musl release binary (`aikup` rate-limited) |
| Aiken stdlib | **v3** (`aiken-lang/stdlib`) | Do not keep default old `1.5.0` |
| Plutus | **V3** | hello_world spend |
| cardano-cli | **11.2.3.0** | Key gen; not required for Mesh submit path |
| Wallet signing (practice) | MeshWallet + CLI payment **skey** | CIP-30 **not** exercised |
| Oracle live call | **none** | Out of scope for this package |

## Practice transaction artifacts (Preview)

| Step | Tx hash (full) | Block |
|------|----------------|-------|
| Lock | `ebdc1565c39b6d295736317634bcb019a65860ce787669058010b005f6dd569c` | 4697224 |
| Unlock | `b7e23631f73db4a5a7913001dbbfd7cb57ad48d63ef043c4bd9d71f7f524d975` | 4697227 |

Validator hash: `4f1bb6b9075f46f6abe6d086e992d68f7e5c8c8707bef95990d83468`  
Script address (Preview): `addr_test1wp83hd4eqa05da4tumggd6vj668huhyvsurma72ejrvrg6q8urlm4`

Mesh **2 ADA send**: verified in practice spike STATUS; **full tx hash not
persisted** in the spike tree — omitted on purpose (no invented hash).

## Public Preview addresses from practice (OK to cite)

| Role | Address |
|------|---------|
| Script (hello_world) | `addr_test1wp83hd4eqa05da4tumggd6vj668huhyvsurma72ejrvrg6q8urlm4` |
| Payment (enterprise) | `addr_test1vzj77ufxhzlzn55zvzv2ep059usu5v2m949r5z3rqjcxjuqpnt5sc` |
| Mesh change (base, same payment key) | `addr_test1qzj77ufxhzlzn55zvzv2ep059usu5v2m949r5z3rqjcxju9g500utfac3r6wvsygpnvt57a5ht0edjs0n6ejlwvuytns2lkwem` |
| Receiver (send target) | `addr_test1vz877n8avll56tsv377ulazda2h4mgsq6wl9q7vtvkkenyq3qdsq7` |

No private keys or project ids belong in this file.

## Doc verification date

External Mesh / Aiken install URLs (and Preview faucet docs / UI) re-checked
**2026-09-27 ~9:10 PM PT** (America/Los_Angeles) — see `STATUS.md`. Practice
runs dated **2026-09-25** PT (lock/unlock artifacts timestamped **2026-09-26**
UTC ≈ evening PT prior day).
