# Leo1

LEO1 is a Mumbai-based education fintech founded in 2017 that operates an all-in-one campus and financial platform for Indian educational institutions — co-branded student and alumni prepaid cards, institutional fee collection and reconciliation, and fee-financing (education loan) journeys that convert lump-sum tuition into instalments.

LEO1 publishes the **LEO1 Fees SDK** at [docs.leo1.in](https://docs.leo1.in/): a server-to-server transaction start signed with SHA-512, a browser checkout SDK, and payment-gateway plus fee-finance webhooks. The production API serves a public **OpenAPI 3.0.2** description at `https://api.leo1.in/openapi.json` — 152 paths, 184 operations, 121 schemas across students, fee dues and structures, fee collections, transactions, payments and settlements, refunds, penalties, waivers, NACH/eNACH mandates, documents and institute administration.

Backed by: qed-investors — https://www.leo1.in/

## Artifacts

| Artifact | Path |
|---|---|
| OpenAPI (verbatim) | `openapi/leo1-leofees-openapi-original.json` |
| Overlay | `overlays/leo1-leofees-overlay.yaml` |
| Authentication | `authentication/leo1-authentication.yml` |
| Conventions | `conventions/leo1-conventions.yml` |
| Error catalog | `errors/leo1-problem-types.yml` |
| Webhook catalog | `asyncapi/leo1-fees-webhooks.yml` |
| Data model | `data-model/leo1-data-model.yml` |
| Sandbox / environments | `sandbox/leo1-sandbox.yml` |
| Embedded components | `components/leo1-components.yml` |
| Packages | `packages/leo1-packages.yml` |
| Lifecycle | `lifecycle/leo1-lifecycle.yml` |
| Conformance | `conformance/leo1-conformance.yml` |
| Domain security | `security/leo1-domain-security.yml` |
| Well-known probe | `well-known/leo1-well-known.yml` |
| MCP (candidate) | `mcp/leo1-mcp.yml` |
| Agent skills | `skills/_index.yml` |
| llms.txt | `llms/leo1-llms.txt` |

## Recorded gaps

No `.well-known` documents, security.txt, vulnerability-disclosure program, trust center or named certifications, status page, changelog, roadmap, CLI, public GitHub organisation, registry-published SDKs, AsyncAPI document, or MCP server. These are captured as honest negatives in the artifacts above rather than filled in.
