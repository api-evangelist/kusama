---
name: kusama-follow-finalized-blocks
description: Build a reliable Kusama event consumer over WebSocket subscriptions — choosing finalized over best-chain heads, surviving reconnects without gaps, and reacting to runtime upgrades.
api: kusama:json-rpc
base_url: wss://kusama-rpc.polkadot.io
operations:
  - chain_subscribeFinalizedHeads
  - chain_subscribeNewHeads
  - state_subscribeStorage
  - state_subscribeRuntimeVersion
  - chainHead_v1_follow
  - chain_getBlockHash
  - chain_getHeader
  - system_health
generated: '2026-07-19'
method: generated
source: examples/kusama-rpc-methods.json (live probe 2026-07-19), asyncapi/kusama-jsonrpc-asyncapi.yml
---

# Follow Kusama blocks and events

Kusama has no webhooks. There is no vendor to register a callback URL with. Push delivery means holding
a WebSocket subscription open against a node at `wss://kusama-rpc.polkadot.io`.

## Step 1 — subscribe to finalized heads, not new heads

Use `chain_subscribeFinalizedHeads`.

`chain_subscribeNewHeads` fires earlier and is tempting for latency, but best-chain blocks can be
reorged away — a block you saw and acted on can cease to exist. For anything financial, accounting, or
user-visible, only finalized blocks are real. Kusama finalizes within seconds under normal conditions,
so the latency you trade away is small and the correctness you buy is total.

Subscribe to `chain_subscribeNewHeads` in addition only if you want optimistic UI, and label that state
as provisional everywhere it surfaces.

For new integrations, prefer `chainHead_v1_follow` — the spec-versioned subscription that reports new,
best and finalized blocks on one channel and **pins** reported blocks so their state stays queryable
until you `chainHead_v1_unpin` them. That pinning solves a real problem: with the legacy subscriptions,
a block's state can be pruned out from under you between notification and query.

## Step 2 — subscribe to storage for domain events

`state_subscribeStorage` takes an array of hex-encoded storage keys and emits a change set whenever any
of them changes. This is the closest thing Kusama has to a domain-event feed — balance changes, staking
era rollovers, referendum transitions are all storage changes.

Resolve keys from `GET /pallets/{palletId}/storage` on the Sidecar rather than hardcoding them; storage
key derivation changes across runtime upgrades.

## Step 3 — subscribe to runtime version

`state_subscribeRuntimeVersion`. This is operational, not optional.

Kusama is the canary network: runtime upgrades land here first, faster, and with less notice than on
Polkadot. When `transactionVersion` bumps, extrinsic encoding has changed and any cached metadata is
stale — a transaction-constructing service that ignores this starts emitting invalid payloads. On this
event, re-fetch metadata (`state_getMetadata`, or `GET /runtime/metadata`) before signing anything else.

A `specVersion` bump alone may be non-breaking, but decoded event and call shapes can still shift, so
re-fetch on either.

## Step 4 — track your own high-water mark

Persist the height and hash of the last finalized block you fully processed. Not the last one you
received — the last one you finished handling. This is the only thing that makes the consumer restartable.

## Step 5 — backfill the gap on reconnect

WebSockets drop. When yours does, the subscription does not resume where it left off; it resumes at the
current head, and everything in between is simply never delivered.

On reconnect:

1. Read your persisted high-water mark, `H`.
2. Get the current finalized head.
3. For every height from `H+1` to the current head, fetch it explicitly — `chain_getBlockHash` then
   `chain_getHeader`, or `GET /blocks/{blockId}` on the Sidecar for full bodies and events.
4. Only after the gap is closed, resume live subscription processing.

Skipping this step is the most common way a Kusama indexer silently loses data. It fails invisibly:
nothing errors, the feed just has holes.

## Step 6 — make processing idempotent

You will reprocess blocks. Reconnect backfill overlaps with in-flight live notifications, and restarts
replay from the high-water mark.

Key your downstream writes on `(block hash, extrinsic index)` — the timepoint. Not block height: heights
are reused across forks, hashes are not. An upsert on that key makes replay harmless.

## Step 7 — monitor the connection

Poll `system_health` periodically. It returns `{peers, isSyncing, shouldHavePeers}` — observed live as
`{"peers":74,"isSyncing":false,"shouldHavePeers":true}` on 2026-07-19.

If `isSyncing` is true, the node is behind and your "finalized head" is stale. If `peers` drops toward
zero, the node is isolated and you should fail over. A silent WebSocket that stops emitting is
indistinguishable from a quiet chain unless you actively health-check.

## Operational reality

`kusama-rpc.polkadot.io` is best-effort community infrastructure with no SLA, no published rate limits,
and no notice before it throttles or drops you. It is fine for development and for low-stakes
consumption. Anything you depend on should run its own node or use a commercial RPC provider, and should
be able to fail over between endpoints without losing its high-water mark.

For deep history, note that non-archive nodes prune state — use an archive node
(`--pruning=archive`) or the `archive_v1_*` method family.

## Related

- `asyncapi/kusama-jsonrpc-asyncapi.yml` — all 13 subscription channels with message shapes
- `conventions/kusama-conventions.yml` — historical state pinning and numeric encoding
- `lifecycle/kusama-lifecycle.yml` — runtime versioning and what a transactionVersion bump breaks
