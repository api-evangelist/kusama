---
name: kusama-submit-transaction
description: Construct, rehearse and submit a signed Kusama extrinsic safely — offline signing material, fee estimation, dry run, submission, and confirming on-chain success rather than trusting HTTP 200.
api: kusama:sidecar-rest
base_url: https://kusama-public-sidecar.parity-chains.parity.io/
operations:
  - getNodeVersion
  - getTransactionMaterial
  - getTransactionMetadataBlob
  - feeEstimateTransaction
  - dryrunTransaction
  - submitTransaction
  - getNodeTransactionPool
  - getBlockById
  - getExtrinsicByTimepoint
generated: '2026-07-19'
method: generated
source: openapi/kusama-sidecar-openapi.yaml, conventions/kusama-conventions.yml, authentication/kusama-authentication.yml
---

# Submit a Kusama transaction

**This spends real KSM and cannot be undone.** Kusama is a canary network, not a testnet. Rehearse on
Westend (`https://westend-rpc.polkadot.io/`, faucet at https://faucet.polkadot.io/) or against a
Chopsticks fork before you run this against Kusama for the first time.

This API never holds your key. You build a payload, sign it yourself, and submit already-signed bytes.

## Step 1 — confirm the target chain

`getNodeVersion` (`GET /node/version`). Assert `chain == "Kusama"`. Make this a hard precondition on the
write path, not a debug check — submitting to the wrong network is unrecoverable.

## Step 2 — get signing material

`getTransactionMaterial` (`GET /transaction/material`) returns everything needed to construct a signing
payload offline: genesis hash, block hash and number for mortality, `specVersion`, `transactionVersion`,
and the runtime metadata.

For offline or hardware signers that support RFC-0078 merkleized metadata, call
`getTransactionMetadataBlob` (`POST /transaction/metadata-blob`) instead — it returns the minimal
metadata blob and the metadata hash for the `CheckMetadataHash` signed extension, so the signing device
can decode and display what it is signing. This requires V15 metadata on the connected chain.

## Step 3 — choose the nonce deliberately

The nonce is the idempotency key. The runtime executes a given (account, nonce) pair **at most once**,
and the nonce is bound inside the signature so no intermediary can alter it.

Fetch the next nonce via `account_nextIndex` on the JSON-RPC endpoint, and account for any of your own
extrinsics still sitting in the pool — check `getNodeTransactionPool` (`GET /node/transaction-pool`).
Two extrinsics signed with the same nonce means only one executes; two signed with sequential nonces
means both do.

Keep the extrinsic **mortal**. The payload commits to a starting block hash and a validity era, so a
stuck transaction expires instead of landing hours later against changed state. Immortal extrinsics
remove that bound and should be avoided.

## Step 4 — estimate the fee

`feeEstimateTransaction` (`POST /transaction/fee-estimate`) with the signed extrinsic. Returns the fee in
Planck as a string. Check the sender's spendable balance — free minus locks — covers fee plus the
transfer amount plus the existential deposit.

## Step 5 — dry run before you commit

`dryrunTransaction` (`POST /transaction/dry-run`) executes the extrinsic against current state and
returns the dispatch outcome **without submitting it**.

Do not skip this. A dry run turns a class of irreversible on-chain failures — insufficient balance,
wrong proxy permissions, a pallet call that would revert — into a local error you can act on. If the
dry run returns a `DispatchError`, resolve it with `GET /pallets/{palletId}/errors` for the human-readable
meaning, and stop.

## Step 6 — submit

`submitTransaction` (`POST /transaction`) with the hex-encoded signed extrinsic. Returns the extrinsic
hash.

**HTTP 200 here means accepted into the transaction pool. It does not mean the transaction succeeded.**
This is the single most important thing to get right on the write path.

## Step 7 — confirm on-chain success

The extrinsic hash is your correlation id. Poll `getBlockById` (`GET /blocks/{blockId}`) from the
submission height forward, or subscribe to `transactionWatch_v1_submitAndWatch` over
`wss://kusama-rpc.polkadot.io` for a push stream.

Two separate things must both be true:

1. **Included** — the extrinsic appears in a block.
2. **Succeeded** — that extrinsic emitted `ExtrinsicSuccess`, not `ExtrinsicFailed`.

An `ExtrinsicFailed` event carries a pallet-specific `DispatchError`. Resolve it against
`GET /pallets/{palletId}/errors`. The fee is still charged on failure.

Then wait for **finality**. Inclusion in the best chain is not settlement — a best-chain block can be
reorged away. Only treat a transaction as settled once its block is finalized by GRANDPA
(`chain_subscribeFinalizedHeads`).

Once you know the block and index, `getExtrinsicByTimepoint`
(`GET /blocks/{blockId}/extrinsics/{extrinsicIndex}`) is the permanent addressable record.

## Retry rule

On a network timeout with no response, **re-broadcast the identical signed payload**. Do not re-sign
with a fresh nonce — that is how double-submits happen. The original may already be in the pool; the
nonce guarantees only one of two identical payloads executes, but two *differently nonced* payloads both
execute.

If the mortality era has expired, the original can never be included. Only then construct a new
extrinsic, with a fresh nonce, after confirming the original did not land.

## Agent guidance

`submitTransaction` is the only state-changing operation in this API. It must require explicit human
confirmation and must never be exposed to an autonomous agent holding key material. The safe agent
surface is `dryrunTransaction` and `feeEstimateTransaction` — build and rehearse, then hand the payload
to a human to sign.

## Related

- `authentication/kusama-authentication.yml` — signature schemes, proxies, offline signing
- `conventions/kusama-conventions.yml` — idempotency and mortality in full
- `sandbox/kusama-sandbox.yml` — Westend, Chopsticks, dry-run surfaces
