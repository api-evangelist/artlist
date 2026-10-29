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

Artlist is a creative-assets platform for video creators, marketers and brands, licensing royalty-free music, sound effects, stock footage, video templates, LUTs and editing plugins alongside a generative AI toolkit. The Artlist Enterprise API, published at developer.artlist.io, opens the company's music catalog to partner platforms: a Search API for songs, artists and albums with mood/genre/instrument category filters plus BPM, duration, vocal-type and free-text facets, and a Download API that mints signed MP3/WAV asset URLs for licensed assets. Access is OAuth 2.0 client-credentials against an Amazon Cognito authorization server with credentials issued by an account manager, and the developer portal publishes OpenAPI 3.1 definitions, an llms.txt index, an RFC 9727 /.well-known/api-catalog linkset and a hosted MCP server for documentation search.

**Areas:** Company · Music · Audio · Media · Stock Media · Content Licensing · Creative Tools · Search · Generative AI · Video

## Artlist on APIs.io

Everything profiled for Artlist is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/artlist/](https://apis.io/providers/artlist/)** — or go straight to one of the APIs or properties below.

### APIs (5)

- [Artlist Developer Docs MCP Server](https://apis.io/apis/artlist/docs-mcp/) — Hosted Model Context Protocol server for the Artlist Enterprise API documentation. Exposes a single anonymous read-only tool, searchDocs, that searches…
- [Artlist Album API](https://apis.io/apis/artlist/artlist-album-api/) — The album API from Artlist — 1 operation(s) for album.
- [Artlist Artist API](https://apis.io/apis/artlist/artlist-artist-api/) — The artist API from Artlist — 1 operation(s) for artist.
- [Artlist Downloadable API](https://apis.io/apis/artlist/artlist-downloadable-api/) — The downloadable API from Artlist — 1 operation(s) for downloadable.
- [Artlist Song API](https://apis.io/apis/artlist/artlist-song-api/) — The song API from Artlist — 2 operation(s) for song.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [artlist-developer-docs](https://apis.io/servers/artlist/artlist-developer-docs/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Artlist Agentic Access](https://apis.io/agentic-access/artlist/artlist-agentic-access/)

#### Collections (4)

Postman and Open Collection request collections.

- [Search Album API](https://apis.io/collections/artlist/open-artlist-album-api/)
- [Search Artist API](https://apis.io/collections/artlist/open-artlist-artist-api/)
- [Download Downloadable API](https://apis.io/collections/artlist/open-artlist-downloadable-api/)
- [Search Song API](https://apis.io/collections/artlist/open-artlist-song-api/)

#### Overlays (2)

OpenAPI overlays, including conversational phrasing for each operation.

- [Artlist Download Overlay](https://apis.io/overlays/artlist/artlist-download-overlay/)
- [Artlist Search Overlay](https://apis.io/overlays/artlist/artlist-search-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Artlist Authentication](https://apis.io/security/artlist/artlist-authentication/)
- [Artlist Domain Security](https://apis.io/security/artlist/artlist-domain-security/)

#### Rate limits (1)

Published request limits.

- [Artlist Rate Limits](https://apis.io/rate-limits/artlist/artlist-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://artlist.io/)
- [Documentation](https://developer.artlist.io/welcome)
- [Getting started](https://developer.artlist.io/welcome)
- [API reference](https://developer.artlist.io/search/song/song-controller-get-songs)
- [Pricing](https://artlist.io/pricing)
- [Sign up](https://artlist.io/start-now)
- [Support](https://help.artlist.io/hc/en-us)
- [Blog](https://artlist.io/blog/)
- [Compliance](https://help.artlist.io/hc/en-us/articles/29556821619101-Privacy-at-Artlist)
- [Terms of service](https://artlist.io/help-center/privacy-terms/terms-of-use/)
- [Privacy policy](https://artlist.io/help-center/privacy-terms/privacy-policy/)
- [GitHub](https://github.com/Artlist-LTD)

## API Evangelist rating

**Kin Score 45.1** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 75.0 |
| Contract quality | 39.0 |
| Contract governance | 18.2 |
| Operational transparency | 34.2 |
| Developer ergonomics | 63.7 |
| Access clarity | 35.5 |

**Agent readiness 34.8** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | na |
| MCP Server | platform (partial credit) |
| Auth Clarity | bearer (partial credit) |
| Idempotency | na |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | yes |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | na |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | distinct |

**Regulatory alignment 27.3** — AI & Biometrics

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/artlist/](https://apis.io/providers/artlist/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Artlist: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/artlist/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
