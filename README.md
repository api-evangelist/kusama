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

Kusama is the permissionless canary network of the Polkadot ecosystem — the same Polkadot SDK
(Substrate/FRAME) runtime as Polkadot, but with faster governance, lower barriers to entry, and real
economic stakes. Runtime upgrades, coretime mechanics, and governance changes ship here first. Native
token KSM (12 decimals, SS58 prefix 2).

This is an [API Evangelist](https://apievangelist.com) network profile. It indexes Kusama's public API
surface as an [APIs.json](apis.yml) document plus supporting artifacts.

## APIs

| API | Base URL | Spec |
|---|---|---|
| Kusama JSON-RPC | `https://kusama-rpc.polkadot.io/` (WSS for subscriptions) | [130 methods probed live](examples/kusama-rpc-methods.json) |
| Kusama Sidecar REST | `https://kusama-public-sidecar.parity-chains.parity.io/` | [OpenAPI 3.0, 119 operations](openapi/kusama-sidecar-openapi.yaml) |

Both are unauthenticated, public-read, and best-effort community infrastructure with no published SLA.

## Artifacts

- [`openapi/`](openapi/) — Substrate API Sidecar OpenAPI 3.0.0 (v20.14.1), 119 operations, 156 schemas
- [`asyncapi/`](asyncapi/) — the 13 WebSocket subscription channels (Kusama publishes no webhooks)
- [`overlays/`](overlays/) — non-destructive enhancements over Parity's spec, plus spec-quality findings
- [`conventions/`](conventions/) — idempotency via account nonce, `at` block pinning, numeric encoding
- [`authentication/`](authentication/) — why there is no HTTP auth, and how extrinsic signing works
- [`errors/`](errors/) — both error envelopes, and why HTTP 200 does not mean success
- [`data-model/`](data-model/) — entity graph derived from the spec's schema references
- [`lifecycle/`](lifecycle/) — runtime versioning, the Sidecar deprecation, liveness probes
- [`changelog/`](changelog/) — dated release history
- [`conformance/`](conformance/) — standards this does and does not implement, with evidence
- [`sandbox/`](sandbox/) — Westend, Chopsticks, and the dry-run surfaces
- [`packages/`](packages/) — client libraries across JavaScript, Python and Rust
- [`cli/`](cli/) — `polkadot`, `subxt-cli`, `polkadot-api`
- [`mcp/`](mcp/) — candidate read-only MCP tool surface (no official server exists)
- [`skills/`](skills/) — four packaged Agent Skills for the common flows
- [`security/`](security/) — Parity's disclosure program and domain security probe results
- [`llms/`](llms/) — llms.txt for agent consumption
- [`examples/`](examples/) — verbatim live request/response captures
- [`well-known/`](well-known/) — probe results (no `.well-known` surface is published)

## Integration notes

Kusama is a blockchain, not a hosted SaaS API. A few things bite integrators repeatedly:

- **Kusama is not a testnet.** KSM has real market value and transactions are irreversible. Test on
  Westend or a Chopsticks fork.
- **Numbers are strings, in Planck.** 1 KSM = 10^12 Planck; balances exceed IEEE-754 safe integer range.
- **Pin `at`.** Without it, two reads seconds apart are not consistent with each other.
- **HTTP 200 on submission means pooled, not executed.** Inclusion, success and finality are separate.
- **Idempotency is the account nonce**, not a header. On timeout, re-broadcast the same signed payload.

See [`llms/kusama-llms.txt`](llms/kusama-llms.txt) for the full agent-facing briefing.

## Links

- Website: https://kusama.network/
- Developer docs: https://docs.polkadot.com/
- JSON-RPC spec: https://paritytech.github.io/json-rpc-interface-spec/
- Getting started: https://wiki.polkadot.com/kusama/kusama-getting-started/
- GitHub: https://github.com/paritytech
- Governance: https://kusama.subsquare.io/ · https://kusama.polkassembly.io/
- Security: https://security.parity.io/ · https://parity.io/bug-bounty

Backed by: pantera-capital
