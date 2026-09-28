# Plain language — Preview Mesh + Aiken hello

**Scope:** community learning explanation for Cardano **Preview**.  
**Not claimed:** production-ready, battle-tested, official, CIP-30, oracle
integration, or mainnet. We verified Mesh send + Aiken hello lock/unlock on
Preview at the pins in `VERSIONS.md`.

For anyone who does not live in Cardano tooling all day.

## What this recipe is

A small, honest practice path:

1. Talk to Preview through **Blockfrost** (a hosted API — you keep your project
   id secret).
2. Use **Mesh** (TypeScript) to query the tip, list UTxOs, and send a little
   test ADA.
3. Write a tiny **Aiken** validator called **hello_world**.
4. Use Mesh again to **lock** 5 ADA at that script, then **unlock** it with the
   magic message and the owner’s signature.
5. Confirm on a Preview explorer.

That is the whole cake. No price feeds. No browser wallet UI in this spike.

## Two tools, two jobs

### Mesh — off-chain builder and courier

**Mesh** runs on your computer (or in a web app later). It helps you:

- Ask Blockfrost for tip / UTxOs
- Build a transaction
- Sign and submit
- Point you at an explorer

In practice we signed with **MeshWallet** loaded from a **CLI payment `.skey`**
(never commit that file). Mesh can also talk to browser wallets via **CIP-30**;
we did **not** run that path here.

### Aiken — on-chain referee

**Aiken** compiles a **validator**: a small program that answers “Is this spend
allowed?” when someone tries to spend money locked at a **script address**.

Our hello validator unlocks only if:

1. The **redeemer** (spender’s message) is exactly `Hello, World!`
2. The **owner** named in the **datum** (a note attached at lock time) also
   signed the transaction

Compile → `plutus.json` blueprint → Mesh lock → Mesh unlock → explorer shows a
valid contract spend. That is the proof this package cites.

## What we actually saw (Solid)

| Step | What happened |
|------|----------------|
| Mesh tip / UTxO | Worked on Preview |
| Mesh send | **2 ADA** to a practice receiver (send succeeded; **full tx hash not saved** in the spike tree — do not invent one) |
| Aiken build | `hello_world` Plutus V3, stdlib **v3**, Aiken **v1.1.23** |
| Lock | Tx `ebdc1565c39b6d295736317634bcb019a65860ce787669058010b005f6dd569c` (block **4697224**) — 5 ADA + inline datum + 5 ADA collateral out |
| Unlock | Tx `b7e23631f73db4a5a7913001dbbfd7cb57ad48d63ef043c4bd9d71f7f524d975` (block **4697227**, `valid_contract: true`) |

## Gotchas we hit (will bite you too)

1. **Enterprise vs base address** — After Mesh sends, change often lands at a
   **base** address with the same payment key. The CLI enterprise `payment.addr`
   can look empty. Prefer Mesh wallet UTxO fetch.
2. **Inline vs supplemental datum** — If the script UTxO already has an inline
   datum, do **not** also attach a separate datum on spend
   (`NotAllowedSupplementalDatums`). Use `txInInlineDatumPresent`.
3. **Collateral** — Script spends need a separate ~5 ADA collateral UTxO.
4. **stdlib pin** — `aiken new` may pull an old stdlib; pin **`v3`** for
   `cardano/*` imports.
5. **aikup rate limit** — Unauthenticated GitHub API can block `aikup`; we
   installed Aiken from the **v1.1.23** release tarball.
6. **TypeScript** — Pin **5.8.x** for `ts-node` (newer TypeScript broke our path).

## What this is not

- Not an **oracle** recipe (see sibling `cardano-packaging-glue-starter` for
  the packaging map only — that package also did **not** run a live oracle)
- Not **CIP-30** / Lace / browser-wallet UI
- Not **mainnet**
- Not a claim that Mesh’s official Aiken guides are wrong — they are the
  upstream docs; this adds **Preview-specific evidence and gotchas we hit**

## Confidence legend

- **Solid** — We ran it or re-fetched the doc.
- **Shaky** — Reasonable inference; do not treat as measured fact.
- **Unknown** — Not evidenced; do not invent numbers.

## One sentence for stakeholders

On Preview we sent ADA with Mesh, compiled an Aiken hello validator, locked and
unlocked 5 ADA with full explorer txs — a learning scaffold, not a product.
