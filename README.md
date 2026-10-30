# Kusama

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Kusama is the permissionless, unaudited canary network of the Polkadot ecosystem, running the same Polkadot SDK (Substrate/FRAME) runtime as Polkadot but with faster governance, lower barriers to entry, and real economic conditions — it is where runtime upgrades, parachain/coretime mechanics, and governance changes ship first before reaching Polkadot. Its developer surface is a chain interface rather than a hosted SaaS API: a public Substrate JSON-RPC 2.0 endpoint at kusama-rpc.polkadot.io exposing 130 live methods (chain, state, author, system, payment, plus the newer chainHead/archive/transaction v1 families) over HTTP and WebSocket, and a REST projection of the same chain state via Substrate API Sidecar at kusama-public-sidecar.parity-chains.parity.io, documented by a 119-operation OpenAPI 3.0 specification. Access is unauthenticated and public-read; write access happens by submitting SCALE-encoded, key-signed extrinsics. The native token is KSM (12 decimals, SS58 prefix 2).

**Areas:** Company · Cryptocurrency · Blockchain · Web3 · Polkadot · Substrate · JSON-RPC · Blockchain Data · Staking · Governance · Real-Time

## Kusama on APIs.io

Everything profiled for Kusama is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/kusama/](https://apis.io/providers/kusama/)** — or go straight to one of the APIs or properties below.

### APIs (21)

- [Kusama JSON-RPC API](https://apis.io/apis/kusama/json-rpc/) — The public Substrate JSON-RPC 2.0 interface to the Kusama relay chain. Probed live on 2026-07-19: 130 methods are advertised by rpc_methods, spanning chain…
- [Kusama Accounts API](https://apis.io/apis/kusama/kusama-accounts-api/) — The accounts API from Kusama — 11 operation(s) for accounts.
- [Kusama Ahm API](https://apis.io/apis/kusama/kusama-ahm-api/) — Asset Hub Migration information
- [Kusama Blocks API](https://apis.io/apis/kusama/kusama-blocks-api/) — The blocks API from Kusama — 8 operation(s) for blocks.
- [Kusama Contracts API](https://apis.io/apis/kusama/kusama-contracts-api/) — The contracts API from Kusama — 1 operation(s) for contracts.
- [Kusama Coretime API](https://apis.io/apis/kusama/kusama-coretime-api/) — The coretime API from Kusama — 6 operation(s) for coretime.
- [Kusama Node API](https://apis.io/apis/kusama/kusama-node-api/) — node connected to sidecar
- [Kusama Pallets API](https://apis.io/apis/kusama/kusama-pallets-api/) — pallets employed in the runtime
- [Kusama Paras API](https://apis.io/apis/kusama/kusama-paras-api/) — The paras API from Kusama — 9 operation(s) for paras.
- [Kusama rc accounts API](https://apis.io/apis/kusama/kusama-rc-accounts-api/) — The rc accounts API from Kusama — 5 operation(s) for rc accounts.
- [Kusama Rc API](https://apis.io/apis/kusama/kusama-rc-api/) — relay chain specific endpoints for asset hub
- [Kusama rc blocks API](https://apis.io/apis/kusama/kusama-rc-blocks-api/) — The rc blocks API from Kusama — 8 operation(s) for rc blocks.
- [Kusama rc node API](https://apis.io/apis/kusama/kusama-rc-node-api/) — The rc node API from Kusama — 3 operation(s) for rc node.
- [Kusama rc pallets API](https://apis.io/apis/kusama/kusama-rc-pallets-api/) — The rc pallets API from Kusama — 13 operation(s) for rc pallets.
- [Kusama rc runtime API](https://apis.io/apis/kusama/kusama-rc-runtime-api/) — The rc runtime API from Kusama — 5 operation(s) for rc runtime.
- [Kusama rc staking API](https://apis.io/apis/kusama/kusama-rc-staking-api/) — The rc staking API from Kusama — 4 operation(s) for rc staking.
- [Kusama rc transaction API](https://apis.io/apis/kusama/kusama-rc-transaction-api/) — The rc transaction API from Kusama — 5 operation(s) for rc transaction.
- [Kusama Runtime API](https://apis.io/apis/kusama/kusama-runtime-api/) — The runtime API from Kusama — 5 operation(s) for runtime.
- [Kusama Staking API](https://apis.io/apis/kusama/kusama-staking-api/) — The staking API from Kusama — 4 operation(s) for staking.
- [Kusama Trace API](https://apis.io/apis/kusama/kusama-trace-api/) — The trace API from Kusama — 4 operation(s) for trace.
- [Kusama Transaction API](https://apis.io/apis/kusama/kusama-transaction-api/) — The transaction API from Kusama — 6 operation(s) for transaction.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Kusama JSON-RPC Subscription API](https://apis.io/asyncapis/kusama/kusama-jsonrpc-asyncapi/)

#### Event channels (10)

Individual events and webhooks the provider emits.

- [author_submitAndWatchExtrinsic](https://apis.io/channels/kusama/author-submitandwatchextrinsic/)
- [beefy_subscribeJustifications](https://apis.io/channels/kusama/beefy-subscribejustifications/)
- [chain_subscribeAllHeads](https://apis.io/channels/kusama/chain-subscribeallheads/)
- [chain_subscribeFinalizedHeads](https://apis.io/channels/kusama/chain-subscribefinalizedheads/)
- [chain_subscribeNewHeads](https://apis.io/channels/kusama/chain-subscribenewheads/)
- [chainHead_v1_follow](https://apis.io/channels/kusama/chainhead-v1-follow/)
- [grandpa_subscribeJustifications](https://apis.io/channels/kusama/grandpa-subscribejustifications/)
- [state_subscribeRuntimeVersion](https://apis.io/channels/kusama/state-subscriberuntimeversion/)
- [state_subscribeStorage](https://apis.io/channels/kusama/state-subscribestorage/)
- [transactionWatch_v1_submitAndWatch](https://apis.io/channels/kusama/transactionwatch-v1-submitandwatch/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [kusama](https://apis.io/servers/kusama/kusama/)

#### Examples (2)

Example requests and responses.

- [Kusama Jsonrpc Examples](https://apis.io/examples/kusama/kusama-jsonrpc-examples/)
- [Kusama Rpc Methods](https://apis.io/examples/kusama/kusama-rpc-methods/)

#### Collections (20)

Postman and Open Collection request collections.

- [Substrate API Sidecar accounts API](https://apis.io/collections/kusama/open-kusama-accounts-api/)
- [Substrate API Sidecar accounts ahm API](https://apis.io/collections/kusama/open-kusama-ahm-api/)
- [Substrate API Sidecar accounts blocks API](https://apis.io/collections/kusama/open-kusama-blocks-api/)
- [Substrate API Sidecar accounts contracts API](https://apis.io/collections/kusama/open-kusama-contracts-api/)
- [Substrate API Sidecar accounts coretime API](https://apis.io/collections/kusama/open-kusama-coretime-api/)
- [Substrate API Sidecar accounts node API](https://apis.io/collections/kusama/open-kusama-node-api/)
- [Substrate API Sidecar accounts pallets API](https://apis.io/collections/kusama/open-kusama-pallets-api/)
- [Substrate API Sidecar accounts paras API](https://apis.io/collections/kusama/open-kusama-paras-api/)
- [Substrate API Sidecar accounts rc accounts API](https://apis.io/collections/kusama/open-kusama-rc-accounts-api/)
- [Substrate API Sidecar accounts rc API](https://apis.io/collections/kusama/open-kusama-rc-api/)
- [Substrate API Sidecar accounts rc blocks API](https://apis.io/collections/kusama/open-kusama-rc-blocks-api/)
- [Substrate API Sidecar accounts rc node API](https://apis.io/collections/kusama/open-kusama-rc-node-api/)
- [Substrate API Sidecar accounts rc pallets API](https://apis.io/collections/kusama/open-kusama-rc-pallets-api/)
- [Substrate API Sidecar accounts rc runtime API](https://apis.io/collections/kusama/open-kusama-rc-runtime-api/)
- [Substrate API Sidecar accounts rc staking API](https://apis.io/collections/kusama/open-kusama-rc-staking-api/)
- [Substrate API Sidecar accounts rc transaction API](https://apis.io/collections/kusama/open-kusama-rc-transaction-api/)
- [Substrate API Sidecar accounts runtime API](https://apis.io/collections/kusama/open-kusama-runtime-api/)
- [Substrate API Sidecar accounts staking API](https://apis.io/collections/kusama/open-kusama-staking-api/)
- [Substrate API Sidecar accounts trace API](https://apis.io/collections/kusama/open-kusama-trace-api/)
- [Substrate API Sidecar accounts transaction API](https://apis.io/collections/kusama/open-kusama-transaction-api/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Kusama Sidecar Overlay](https://apis.io/overlays/kusama/kusama-sidecar-overlay/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Kusama Authentication](https://apis.io/security/kusama/kusama-authentication/)
- [Kusama Domain Security](https://apis.io/security/kusama/kusama-domain-security/)
- [Kusama Vulnerability Disclosure](https://apis.io/security/kusama/kusama-vulnerability-disclosure/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://kusama.network/)
- [Documentation](https://docs.polkadot.com/chain-interactions/)
- [Getting started](https://wiki.polkadot.com/kusama/kusama-getting-started/)
- [API reference](https://paritytech.github.io/json-rpc-interface-spec/)
- [Support](https://docs.polkadot.com/get-support/)
- [Terms of service](https://kusama.network/terms)
- [Privacy policy](https://kusama.network/privacy)
- [GitHub](https://github.com/paritytech)

## API Evangelist rating

**Kin Score 48.6** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 73.2 |
| Contract quality | 53.9 |
| Contract governance | 4.5 |
| Operational transparency | 44.7 |
| Developer ergonomics | 78.0 |
| Access clarity | 21.1 |

**Agent readiness 26.1** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | derived (partial credit) |
| Error Semantics | verified |
| OpenAPI Examples | partial |
| Rate Limit Signal | — |
| Event Surface Described | derived (partial credit) |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | mostly |

**Regulatory alignment 30.4** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/kusama/](https://apis.io/providers/kusama/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Kusama: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/kusama/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
