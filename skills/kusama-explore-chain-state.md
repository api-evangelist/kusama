---
name: kusama-explore-chain-state
description: Discover what a Kusama runtime actually exposes — its pallets, storage items, constants, calls, events and errors — by introspecting the chain rather than hardcoding against documentation.
api: kusama:sidecar-rest
base_url: https://kusama-public-sidecar.parity-chains.parity.io/
operations:
  - getNodeVersion
  - getLatestBlockHeader
generated: '2026-07-19'
method: generated
source: openapi/kusama-sidecar-openapi.yaml, data-model/kusama-data-model.yml
---

# Explore Kusama chain state

Kusama's runtime describes itself. Rather than reading documentation that may lag a runtime upgrade, you
can ask the chain what it currently exposes. This is the right way to build anything that must survive
Kusama's fast upgrade cadence.

Note: the introspection operations below carry no `operationId` in Parity's published specification, so
they are referenced by path.

## Step 1 — establish context

- `getNodeVersion` (`GET /node/version`) — confirm `chain == "Kusama"`
- `GET /runtime/spec` — record `specName`, `specVersion`, `transactionVersion`

Observed live on 2026-07-19: `specName: kusama`, `specVersion: 2003000`, `transactionVersion: 26`.

Record `specVersion` with anything you cache. Introspection results are only valid for the runtime
version that produced them.

## Step 2 — walk a pallet's surface

For any pallet, five parallel catalogs:

| What | Path |
|---|---|
| Storage items | `GET /pallets/{palletId}/storage` |
| One storage value | `GET /pallets/{palletId}/storage/{storageItemId}` |
| Constants | `GET /pallets/{palletId}/consts` |
| One constant | `GET /pallets/{palletId}/consts/{constantItemId}` |
| Callable extrinsics | `GET /pallets/{palletId}/dispatchables` |
| Emitted events | `GET /pallets/{palletId}/events` |
| Error variants | `GET /pallets/{palletId}/errors` |

`GET /pallets/{palletId}/errors` deserves specific attention: it is the authoritative catalog of
`DispatchError` variants for that pallet, which is the real error surface for write paths. HTTP status
codes tell you whether the *request* worked; this tells you why an *extrinsic* failed.

## Step 3 — read storage safely

`GET /pallets/{palletId}/storage/{storageItemId}` accepts:

- `at` — pin to a block height or hash. Always set this when comparing values.
- `keys` — restrict to specific map keys.
- `metadata` — include the storage item's type metadata.

Add `metadata=true` on first read to learn the value's type, then drop it — it materially inflates every
subsequent response.

## Step 4 — the deeper surfaces

- `GET /runtime/metadata` — full runtime metadata as decoded JSON. Large. This is what SDK codegen
  consumes (`subxt metadata`, `polkadot-api add`).
- `GET /runtime/metadata/versions` — which metadata versions this runtime supports. Check before assuming
  V15, which RFC-0078 merkleized metadata requires.
- `GET /runtime/code` — the runtime WASM blob itself.

## Step 5 — handle 404 correctly

A 404 on a pallet route means **that pallet is not present in this runtime**, not "no results".

The Sidecar mounts controllers based on the connected chain's metadata, so pallet-specific routes simply
do not exist on chains lacking the pallet. Kusama relay chain and Kusama Asset Hub expose different
pallet sets — assets and asset-conversion live on Asset Hub, staking and coretime governance on the relay
chain. Treat 404 as a capability signal and branch on it.

## Step 6 — cache against specVersion

Introspection results are stable within a runtime version and can change across one. Key any cache on
`specVersion`, and invalidate when `state_subscribeRuntimeVersion` fires. On Kusama this happens more
often than on Polkadot by design — it is the canary network.

## What not to do

- **Do not hardcode storage keys.** Key derivation can change across upgrades. Resolve them.
- **Do not assume Polkadot's pallet set.** Kusama runs ahead; it gains and loses pallets first.
- **Do not model against `/paras`.** Eight operations there are marked "PHASED OUT ENDPOINT IN FAVOR OF
  CORETIME". Build against the `coretime` tag — cores, regions, workloads, renewals.
- **Do not walk large storage maps through this API.** There is no pagination anywhere in the 119
  operations. Bulk analytical queries belong in an indexer (Subscan, SubQuery, Subsquid).

## Related

- `data-model/kusama-data-model.yml` — the entity graph, including why Pallet is the reflective surface
- `errors/kusama-problem-types.yml` — the extrinsic-failure vs HTTP-status distinction
- `conventions/kusama-conventions.yml` — `at` pinning and the absence of pagination
