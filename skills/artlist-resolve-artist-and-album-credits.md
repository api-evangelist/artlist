---
name: Resolve Artlist artist and album credits
description: >-
  Take a song from Artlist search and expand its embedded artist and album summaries into
  full records — bios, imagery, slugs, and primary/featured artist credits — for attribution
  or a native catalog UI.
api: openapi/artlist-search-openapi-original.yml
operations:
  - song-controller-get-songs
  - song-controller-get-song
  - artist-controller-get-artist
  - album-controller-get-album
generated: '2026-08-02'
method: generated
source: >-
  Grounded in the operationIds and schemas published in
  https://developer.artlist.io/openapi/search.yaml, plus
  https://developer.artlist.io/use-cases.
---

# Resolve Artlist artist and album credits

Artlist search embeds only a three-field summary of the artist and album on each song. When
you need bios, cover art or full credits — for an attribution line, a native music-catalog
UI, or a rights record — you resolve them with two follow-up calls.

## 1. Start from a song

Either `song-controller-get-songs` (`GET /search/v1/song?page=1&...`) or
`song-controller-get-song` (`GET /search/v1/song/{id}`) gives you a `Song` carrying:

```json
{
  "id": "...", "name": "...",
  "artist": { "id": "...", "name": "...", "slug": "..." },
  "album":  { "id": "...", "name": "...", "slug": "..." },
  "genreCategories": [ { "id": "...", "name": "..." } ]
}
```

The `slug` fields are URL slugs — use them to deep-link back to artlist.io rather than
constructing paths from names.

## 2. Expand the artist

```
GET https://business.artlist.io/search/v1/artist/{id}
```

`artist-controller-get-artist` returns `{artist: Artist}` with `id`, `name`, `slug`, `bio`,
`profileImage`, `bioImage` and `coverImage`. Use `bio` for an artist panel and `coverImage`
for a hero; `profileImage` is the avatar-sized asset.

## 3. Expand the album

```
GET https://business.artlist.io/search/v1/album/{id}
```

`album-controller-get-album` returns `{album: Album}` with `id`, `name`, `description`,
`slug`, `coverImage`, `insetImage`, a single `artist` summary, and two credit arrays:

- `primaryArtists[]` — the artists credited as principal on the album
- `featuredArtists[]` — collaborating / featured artists

Both are `{id, name}` pairs. Render `primaryArtists` first, then `featuredArtists` prefixed
with "feat." — and resolve each id through `artist-controller-get-artist` only when you
actually need the bio, since each expansion is another request against a 50 req/min budget.

## 4. Cache, do not re-walk

Artist and album records change far less often than search results. Cache them keyed on id
for the life of your token or longer. A catalog UI that expands credits for all 20 songs on
a results page costs 40 extra calls — that is 80% of the `/search` minute budget in one
screen. Expand on selection, not on render.

## Notes and limits

- There is **no reverse navigation**: no operation lists an artist's songs or an album's
  tracks. To build an artist page, search with `query` set to the artist name and filter the
  results client-side on `artist.id`.
- There is no `expand`/`include` parameter — expansion is always a second call.
- Errors use the Artlist envelope `{success: false, error: {message, code, status}}`; a `404`
  on an artist or album id that came from a song response usually means a stale cache.
- All four operations are `GET` and safe to retry.
