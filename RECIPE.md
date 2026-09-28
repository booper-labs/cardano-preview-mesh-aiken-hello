# RECIPE — Preview Mesh send + Aiken hello lock/unlock

**Type:** community scaffolding recipe (learning walkthrough).  
**Verified practice base:** Mesh send + Aiken hello lock/unlock on **Preview**
(pins in `VERSIONS.md`).  
**Not claimed:** CIP-30, oracle attach, mainnet, or production readiness.

Official Mesh Aiken path (upstream): https://meshjs.dev/aiken  
This recipe adds **Preview-specific steps and gotchas we hit**, with full lock /
unlock tx hashes.

## Goal

After following this, you can:

```
faucet fund (Preview)
  → Mesh tip / UTxO / send ADA
  → Aiken hello_world build → plutus.json
  → Mesh lock (inline datum + collateral out)
  → Mesh unlock (redeemer + owner signature)
  → explorer confirm (valid_contract)
```

## 0. Preconditions (Preview only)

- Cardano **Preview** Blockfrost project id — **never commit it**
  (env `BLOCKFROST_PROJECT_ID` or a mode-600 local file)
- Node.js **20.x** (practice; Mesh docs say 18+)
- TypeScript **5.8.x** + `ts-node` (practice pin)
- Aiken CLI **v1.1.23** with stdlib **`v3`**
- `@meshsdk/core` **1.9.1**
- Preview faucet access: https://faucet.preview.world.dev.cardano.org/basic-faucet  
  Docs: https://docs.cardano.org/cardano-testnets/tools/faucet
- Signing path documented here: **MeshWallet + CLI payment skey**  
  (CIP-30 is a separate later recipe — **not** this one)

## 1. Mesh hello send (Solid — practice)

Practice tree (local spike, not shipped in this publish folder):
`preview-practice-spike/mesh-hello/`.

Typical flow:

```bash
# generate Preview payment + receiver keys (cardano-cli); never log .skey
# fund payment.addr via Preview faucet
cd mesh-hello
npm install
npx ts-node src/query-tip.ts
npx ts-node src/query-utxo.ts
npx ts-node src/build-send.ts   # 2 ADA → receiver, or exit 0 if unfunded
```

Pattern (conceptual — do not paste secrets):

- `BlockfrostProvider(projectId)`
- `MeshWallet({ networkId: 0, key: { type: "cli", payment: cborHex } })`
- `MeshTxBuilder` → `txOut(receiver, 2 ADA)` → `changeAddress` → `complete`
- `wallet.signTx` → `wallet.submitTx`

**Evidence:** spike STATUS marks Mesh tip / UTxO / build-send **done**.  
**Gap:** full send **tx hash was not persisted** — cite send success without a
hash artifact.

**Gotcha:** Mesh `getChangeAddress()` may be a **base** address; enterprise
`payment.addr` can show **0** after the send while funds sit at the base
change address with the **same payment key**. Prefer wallet UTxO fetch.

## 2. Aiken hello_world build (Solid — practice)

Practice tree: `preview-practice-spike/aiken-hello/`.

Validator idea (Plutus V3 spend):

1. Redeemer message == `Hello, World!`
2. Tx signed by `owner` VerificationKeyHash stored in the datum

```bash
export PATH="$HOME/.aiken/bin:$PATH"
cd aiken-hello
# aiken.toml: aiken-lang/stdlib version = "v3"  (do not keep old 1.5.0)
aiken check && aiken build
aiken blueprint address   # Preview script address
```

Practice outputs:

| Artifact | Value |
|----------|-------|
| Validator hash | `4f1bb6b9075f46f6abe6d086e992d68f7e5c8c8707bef95990d83468` |
| Script address (Preview) | `addr_test1wp83hd4eqa05da4tumggd6vj668huhyvsurma72ejrvrg6q8urlm4` |

**Install gotcha:** `aikup` hit GitHub API rate limit in practice — install
Aiken **v1.1.23** from the release tarball if needed. Upstream install docs:
https://aiken-lang.org/installation-instructions

## 3. Mesh lock (Solid — practice)

Practice offchain: `aiken-hello/offchain/src/lock.ts`.

What the lock tx does:

1. Resolve script from CIP-57 `plutus.json` (`applyParamsToScript`, Plutus **V3**)
2. Datum = Constr 0 with owner payment key hash from change address
3. Output **5 ADA** to script with **`txOutInlineDatumValue(datum)`**
4. Also emit a **5 ADA** self-output for later **collateral**
5. Sign + submit via MeshWallet + Blockfrost Preview

Practice result:

| | |
|---|---|
| Tx | `ebdc1565c39b6d295736317634bcb019a65860ce787669058010b005f6dd569c` |
| Block | **4697224** |
| Explorer | https://preview.cardanoscan.io/transaction/ebdc1565c39b6d295736317634bcb019a65860ce787669058010b005f6dd569c |
| Submitted (UTC) | 2026-09-26T01:33:40.975Z (~2026-09-25 evening PT) |

## 4. Mesh unlock (Solid — practice)

Practice offchain: `aiken-hello/offchain/src/unlock.ts`.

What the unlock tx does:

1. Wait for / fetch the script UTxO
2. Pick ~5 ADA **collateral** (distinct from fee inputs)
3. Build spend:

```ts
txBuilder
  .spendingPlutusScriptV3()
  .txIn(lockedTxHash, lockedIndex)
  .txInInlineDatumPresent()          // inline already on UTxO
  .txInRedeemerValue({ alternative: 0, fields: ["Hello, World!"] })
  .txInScript(script.code)
  .requiredSignerHash(ownerHash)
  .txInCollateral(...)
  .changeAddress(changeAddress)
  .selectUtxosFrom(feeUtxos)
  .complete();
```

4. `wallet.signTx(unsignedTx, true)` (partial sign for required signer) → submit

**Critical gotcha:** do **not** also call `txInDatumValue` when an inline datum
is present — Preview returned `NotAllowedSupplementalDatums`.

Practice result:

| | |
|---|---|
| Tx | `b7e23631f73db4a5a7913001dbbfd7cb57ad48d63ef043c4bd9d71f7f524d975` |
| Block | **4697227** |
| Notes | `valid_contract: true`, fee **805633** lovelace |
| Explorer | https://preview.cardanoscan.io/transaction/b7e23631f73db4a5a7913001dbbfd7cb57ad48d63ef043c4bd9d71f7f524d975 |
| Submitted (UTC) | 2026-09-26T01:34:13.969Z |

## 5. Suggested DIY order

1. Fund Preview payment key; confirm Mesh tip + UTxO
2. Complete one Mesh ADA send until change-address behavior is boring
3. Build Aiken hello with stdlib **v3**; note script address
4. Lock with inline datum + collateral out; save the lock tx hash
5. Unlock with redeemer + `requiredSignerHash` + collateral
6. Confirm both txs on Preview Cardanoscan
7. **Stop** — do not bolt on oracles or CIP-30 in the same first pass

## 6. Explicit non-goals for this recipe

- No oracle statement / feed / pull update
- No CIP-30 browser signing (document separately later)
- No mainnet addresses or TVL claims
- No assertion that Midnight Compact fills this path (different island)
- No seeds, mnemonics, `.skey` contents, or Blockfrost project ids in git
