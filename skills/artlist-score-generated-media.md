---
name: Score generated media with Artlist music
description: >-
  The generative-media use case Artlist documents: given a produced clip's mood, pacing and
  length, pick a matching royalty-free track from the Artlist catalog and attach a licensed
  audio file to the output.
api: openapi/artlist-search-openapi-original.yml, openapi/artlist-download-openapi-original.yml
operations:
  - song-controller-get-songs
  - downloadable-controller-get-downloadable-url
generated: '2026-08-02'
method: generated
source: >-
  https://developer.artlist.io/use-cases (Generative Media Applications) and
  https://developer.artlist.io/dictionaries, grounded in the operationIds published in the
  Artlist OpenAPI documents.
---

# Score generated media with Artlist music

Artlist documents this as its first-class API use case: an AI video, animation or
mixed-media tool that automatically pairs a track to the clip it just produced. This skill
is the agent-side recipe.

## 1. Turn the clip into filter values

You have three signals from the generated clip. Convert each into a concrete parameter of
`song-controller-get-songs`:

- **Length** → `durationMin` / `durationMax`. Ask for a track at least as long as the clip,
  with headroom for a tail: `durationMin = clip_seconds`, `durationMax = clip_seconds + 60`,
  clamped to the documented 0–420 second range.
- **Pacing / energy** → `bpmMin` / `bpmMax`, 0–200. A rough map: ambient/contemplative
  60–90, narrative 90–110, upbeat 110–130, high-energy 130+.
- **Mood, genre, instrumentation and theme** → `categoryIds`. Resolve every descriptor
  through the Artlist song dictionary at <https://developer.artlist.io/dictionaries>: for
  example `78` hopeful, `13` dramatic, `15` tense, `92` dark, `62` cinematic, `64`
  electronic, `40` piano, `86` orchestra, `553` vlog, `552` commercial, `551` trailer.
  **Never invent a category id** — they are numeric and unguessable; look them up.

Almost always set `vocalType=INSTRUMENTAL` when the clip carries narration or dialogue, so
lyrics do not fight the voice track. Use `VOCAL` or `VOCAL_AND_INSTRUMENTS` only for
music-forward output.

Keep `query` for anything the taxonomy cannot express ("slow build", "sparse").

```
GET https://business.artlist.io/search/v1/song
  ?page=1
  &categoryIds=62&categoryIds=78
  &vocalType=INSTRUMENTAL
  &bpmMin=90&bpmMax=115
  &durationMin=45&durationMax=120
```

## 2. Rank the candidates yourself

The API returns 20 songs per page with a `total`, and no relevance score. Rank client-side on
what the `Song` object gives you: how close `duration` is to the clip length, how close
`bpmRate` is to the target, and how many of the requested `categoryIds` appear in
`genreCategories[]`. Do not page deeper than you need — `/search` is capped at 50 requests
per minute.

Preview with the `url` (AAC stream) and render `waveSurferUrl` if you are showing the user a
choice. Neither requires the Download API.

## 3. Deliver the audio

Once a track is chosen:

```
GET https://business.artlist.io/download/v1/downloadable/song/{songId}/wave
```

`downloadable-controller-get-downloadable-url` returns `{url}`. Use `wave` when the audio
will be re-encoded during render (avoids generation loss) and `mp3` when it is being
delivered straight to a viewer. `assetType` currently accepts only `song`.

`/download` is capped at **20 requests per minute** — the tightest budget on the API. Mint a
URL once per rendered asset, not once per preview, and cache it for the render job.

## 4. Attribute and record

Carry `song.name`, `song.artist.name` and `song.album.name` into the render's metadata. Use
`artist.slug` and `album.slug` to deep-link back to artlist.io. Record which client
credential minted which download URL — the download is the licensed act, and the Artlist
license terms at <https://artlist.io/help-center/privacy-terms/artlist-license/> govern how
the resulting output may be used.

## Operational rules

- Bearer token from the client-credentials flow, refreshed before its 3600-second expiry.
- On `429`, honour `X-RateLimit-Reset` — a render pipeline that retries tight will starve
  itself out of the 20/min download budget.
- Errors are `{success: false, error: {message, code, status}}`, not problem+json.
- Every operation in this flow is a `GET` and safe to retry after a transport failure.
