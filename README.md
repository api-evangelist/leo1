# Leo1

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
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
