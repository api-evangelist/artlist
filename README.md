# Artlist

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
