---
name: kusama-read-account-state
description: Assemble a complete, internally consistent picture of a Kusama account — balances, locks, staking, vesting, proxies and assets — without reading across block boundaries.
api: kusama:sidecar-rest
base_url: https://kusama-public-sidecar.parity-chains.parity.io/
operations:
  - getNodeVersion
  - getValidationByAccountId
  - accountConvert
  - getAccountBalanceInfo
  - getStakingSummaryByAccountId
  - getStakingPayoutsByAccountId
  - getVestingSummaryByAccountId
  - getProxyInfo
  - getAssetBalances
  - getLatestBlockHeader
generated: '2026-07-19'
method: generated
source: openapi/kusama-sidecar-openapi.yaml, conventions/kusama-conventions.yml
---

# Read Kusama account state

There is no single "get account" operation on Kusama. An account is a set of separate derivations from
chain storage, and each one resolves against whatever block is at the head when you call it. If you fan
out naively you will read across block boundaries and produce a picture that never existed.

## Step 1 — confirm you are on Kusama

Call `getNodeVersion` (`GET /node/version`) before anything else.

Assert `chain == "Kusama"`. The same API shape serves Polkadot, Westend and every parachain; a
misconfigured base URL is silent otherwise.

## Step 2 — validate the address

Call `getValidationByAccountId` (`GET /accounts/{address}/validate`).

Kusama's SS58 prefix is **2**. A Polkadot address (prefix 0) is a well-formed SS58 string but a
different account — this is the most common integration bug against this API. If you were handed a
public key or an address in another network's prefix, normalize it with `accountConvert`
(`GET /accounts/{accountId}/convert`) rather than assuming.

## Step 3 — pin the block

Call `getLatestBlockHeader` (`GET /blocks/head/header`) and keep `number`.

Pass that value as `at` on every subsequent call. This is the whole point of the skill: without `at`,
each call resolves against a head that advances roughly every 6 seconds, and a balance read and a
staking read taken 200ms apart can legitimately disagree. With `at` pinned they are one coherent
snapshot, and the result is reproducible later.

## Step 4 — fan out with `at` pinned

All of these accept `at`:

- `getAccountBalanceInfo` — `GET /accounts/{accountId}/balance-info` — free, reserved, frozen, and `locks[]`
- `getStakingSummaryByAccountId` — `GET /accounts/{accountId}/staking-info` — ledger, nominations, unlocking chunks
- `getStakingPayoutsByAccountId` — `GET /accounts/{accountId}/staking-payouts` — era payouts; pass `unclaimedOnly=true` to narrow
- `getVestingSummaryByAccountId` — `GET /accounts/{accountId}/vesting-info` — pass `includeClaimable=true` for claimable amounts
- `getProxyInfo` — `GET /accounts/{accountId}/proxy-info` — delegated authority granted to or by this account
- `getAssetBalances` — `GET /accounts/{accountId}/asset-balances` — fungible assets (Asset Hub)

`getStakingSummaryByAccountId` returns a 400 or empty ledger for an account that has never staked. That
is a normal result, not an error to retry.

## Step 5 — interpret the numbers correctly

Two rules, both of which are silent-corruption bugs if broken:

1. **Every amount is a string, and it is in Planck.** 1 KSM = 10^12 Planck. Parse with `BigInt` or a
   decimal library. `JSON.parse` into a JavaScript number loses precision above 2^53 — well within
   normal KSM balances.
2. **Free balance is not spendable balance.** Subtract the locks. Staked, vesting and governance-locked
   funds all appear in `free` while being unavailable to transfer. Reporting `free` as "available" is
   wrong and users notice.

## Step 6 — record provenance

Every response carries `at: {height, hash}`. Store it alongside whatever you computed. It is the only
way to explain a number later, and re-querying with the same `at` reproduces it exactly — provided you
are pointed at an archive node, since non-archive nodes prune historical state and will fail rather
than return stale data.

## Error handling

| Condition | Meaning | Action |
|---|---|---|
| 400 | Malformed address, or wrong-network prefix | Validate and convert; do not retry unchanged |
| 404 | Pallet not present in this runtime | Treat as "unsupported on this chain", not "empty" |
| 500 | Usually the upstream node WebSocket dropped | Retry with backoff; re-probe `getNodeVersion` to confirm |

Errors return `{code, message, stack}` — not RFC 9457. There is no stable machine-readable error code
beyond the HTTP status, so branch on status, not on message text.

## Related

- `conventions/kusama-conventions.yml` — `at` pinning, numeric encoding, pagination
- `errors/kusama-problem-types.yml` — both error envelopes
- `data-model/kusama-data-model.yml` — why Account is a derived view rather than a record
