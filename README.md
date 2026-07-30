# Kusama

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
