---
name: Find and download an Artlist track
description: >-
  Authenticate to the Artlist Enterprise API, search the music catalog by mood, genre,
  instrument, BPM, duration and vocal type, then mint a downloadable mp3 or wave URL for the
  chosen song.
api: openapi/artlist-search-openapi-original.yml, openapi/artlist-download-openapi-original.yml
operations:
  - song-controller-get-songs
  - song-controller-get-song
  - downloadable-controller-get-downloadable-url
generated: '2026-08-02'
method: generated
source: >-
  Grounded in the operationIds published in the Artlist OpenAPI documents plus
  https://developer.artlist.io/authentication, /general-terms, /responses-api and
  /dictionaries.
---

# Find and download an Artlist track

The marquee Artlist Enterprise flow: pick a track that fits a brief, then get a delivery URL
for it. Everything below uses real operationIds from the published OpenAPI.

## 0. Get an access token

The API is OAuth 2.0 client-credentials. `client_id` and `client_secret` are issued by an
Artlist account manager — there is no self-service signup yet.

```
POST https://artlist-business-api-prod-cognito.artlist.io/oauth2/token
Authorization: Basic base64(client_id:client_secret)
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials
```

The response is `{access_token, token_type: "Bearer", expires_in: 3600}`. Cache the token and
refresh it before 3600 seconds elapse — do not fetch a token per request, the token endpoint
counts against your quota. Send it on every call as `Authorization: Bearer <access_token>`.

## 1. Translate the brief into filters

`song-controller-get-songs` on `GET https://business.artlist.io/search/v1/song` is the only
list operation. Map the brief onto its parameters:

| Brief element | Parameter |
|---|---|
| free text ("driving cinematic build") | `query` |
| mood / genre / instrument / video theme | `categoryIds` (array) |
| vocal or instrumental | `vocalType` — `VOCAL_AND_INSTRUMENTS`, `VOCAL`, `INSTRUMENTAL`, `FEMALE_VOCAL`, `MALE_VOCAL`, `DUET`, `GROUP`, `ACAPELLA` |
| cut length | `durationMin` / `durationMax`, seconds, 0–420 |
| tempo | `bpmMin` / `bpmMax`, 0–200 |

`page` is **required** and starts at 1.

`categoryIds` are numeric ids from the Artlist song dictionary at
<https://developer.artlist.io/dictionaries> — 98 entries such as `62` cinematic, `13`
dramatic, `40` piano, `551` trailer. Resolve slugs to ids from that dictionary; do not guess
ids.

```
GET /search/v1/song?page=1&query=cinematic%20build&categoryIds=62&categoryIds=13&bpmMin=90&bpmMax=120&durationMax=180
```

## 2. Page through results

The response is `{songs: [...], total: n}` and returns **20 songs per request**. Page size is
fixed; increment `page` until you have covered `total` or found a match. There is no cursor
and no `has_more` flag.

Each `Song` carries `id`, `name`, `artist{id,name,slug}`, `album{id,name,slug}`, `url` (AAC
stream), `waveSurferUrl` (waveform), `imageUrl` / `thumbImageUrl` (artwork), `duration`,
`bpmRate` and `genreCategories[]`. Use `url` to audition and `waveSurferUrl` to render a
waveform — you do not need the Download API to preview.

## 3. Confirm the pick

If you only carried an id forward, re-read the full record with `song-controller-get-song`:

```
GET /search/v1/song/{id}
```

It returns `{song: Song}`.

## 4. Mint the download URL

```
GET https://business.artlist.io/download/v1/downloadable/song/{id}/mp3
```

`downloadable-controller-get-downloadable-url` takes three path parameters:
`assetType` (enum — currently only `song`), `id` (the Song id, numeric or UUID) and `format`
(enum — `mp3` or `wave`). It returns `{url: "..."}` — the delivery URL for the asset.

Treat this call as the licensing-relevant action, not a plain read: it delivers a licensed
asset against the client's contract and quota. Log who requested it and why. See
`agentic-access/artlist-agentic-access.yml`.

## Rules that apply to every step

- **Rate limits**: 100 req/min overall, 50 req/min on `/search`, 20 req/min on `/download`,
  rolling 60-second window. Watch `X-RateLimit-Limit`, `X-RateLimit-Remaining` and
  `X-RateLimit-Reset`; on `429` back off exponentially until reset.
- **Errors** come back as `{success: false, error: {message, code, status}}` — not RFC 9457
  problem+json. Documented statuses: 400, 401, 403, 404, 422, 429. A `401` almost always
  means an expired token; get a new one and retry once.
- **Idempotency**: every operation is a `GET` and is safe to retry. There is no
  idempotency-key contract because there are no write operations.
- **Spec caveat**: both published OpenAPI documents declare `servers: [https://host.com]` and
  embed the absolute URL in the path key. Apply `overlays/artlist-search-overlay.yaml` and
  `overlays/artlist-download-overlay.yaml` before generating a client.
