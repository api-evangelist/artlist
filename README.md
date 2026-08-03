# Artlist

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

Artlist is a creative-assets platform for video creators, marketers and brands, licensing
royalty-free music, sound effects, stock footage, video templates, LUTs and editing plugins
alongside a generative AI toolkit.

The **Artlist Enterprise API** at <https://developer.artlist.io/> opens the company's music
catalog to partner platforms — a **Search API** for songs, artists and albums (mood, genre,
instrument and video-theme category filters plus BPM, duration, vocal-type and free-text
facets) and a **Download API** that mints signed mp3/wave delivery URLs for licensed assets.

- Website — <https://artlist.io/>
- Developer portal — <https://developer.artlist.io/>
- API base URLs — `https://business.artlist.io/search/v1`, `https://business.artlist.io/download/v1`
- Support — enterprise-api-support@artlist.io

## What is in this repo

| Artifact | Notes |
|---|---|
| `openapi/` | Two OpenAPI 3.1.0 documents harvested verbatim from `developer.artlist.io/openapi/{search,download}.yaml` |
| `overlays/` | API Evangelist enhancements — real base URLs, the missing OAuth 2.0 securityScheme, parameter enums and bounds |
| `mcp/` | The hosted docs MCP server (`/_mcp/server`, anonymous, one `searchDocs` tool) and the tool crosswalk |
| `well-known/` | RFC 9727 `/.well-known/api-catalog` linkset, captured verbatim, plus the full probe index |
| `llms/` | `llms.txt` fetched verbatim from the docs host |
| `authentication/` | OAuth 2.0 client-credentials against an Amazon Cognito token endpoint |
| `conventions/` | Pagination, filtering vocabulary, versioning, error envelope, rate-limit signaling |
| `rate-limits/` | 100 req/min global, 50 on `/search`, 20 on `/download`, `X-RateLimit-*` headers |
| `errors/` | The documented `{success, error{message, code, status}}` envelope and status table |
| `conformance/` | OpenAPI 3.1, RFC 9727, OAuth 2.0, llms.txt and MCP conform; RFC 9457 and OIDC do not |
| `lifecycle/` | URI-path v1; no changelog, status page, SLA or deprecation policy |
| `data-model/` | Song / Artist / Album / GenreCategory entity graph derived from the specs |
| `security/` | Probed TLS, HSTS, DNSSEC, CAA, SPF and DMARC posture |
| `agentic-access/` | Recommended `x-agentic-access` contracts for all five operations |
| `skills/` | Three packaged Agent Skills grounded in real operationIds |
| `packages/` | Verified negative — no first-party SDK in any public registry |

## Notable findings

- Artlist serves a real **RFC 9727 `/.well-known/api-catalog`** linkset pointing at both
  OpenAPI documents — rare across the catalog.
- Both published specs declare a placeholder `servers: [https://host.com]` while embedding
  the absolute URL in each path key, and neither declares a `securityScheme` despite a fully
  documented OAuth 2.0 flow. The overlays correct both without mutating the originals.
- The MCP server is a **documentation** search server, not an API wrapper: zero overlap with
  the five REST operations. See `mcp/artlist-tool-crosswalk.yml`.
- No A2A agent card, no security.txt, no status page, no changelog, no sandbox, no CLI.
- The docs carry leftover template text referring to "the Primer REST API" and a "Primer
  account manager".
