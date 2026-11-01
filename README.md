<div align="center">

# EmnexAnime API

**A fast, ad‑free anime API for developers.**
Rich AniList‑powered metadata + fully‑resolved streaming sources (multi‑server, SUB & DUB), delivered as clean JSON.

[![Status](https://img.shields.io/badge/status-live-brightgreen?style=flat-square)](https://api.emnexanime.site/health)
[![Base URL](https://img.shields.io/badge/base-api.emnexanime.site-blue?style=flat-square)](https://api.emnexanime.site)

</div>

> This repository is **documentation only** — there is no source code here.
> It explains what the EmnexAnime API offers and how to get access. To request
> an API key, a higher plan, or a custom/enterprise integration, see
> **[Get access](#-get-access--contact)**.

---

## What it is

EmnexAnime is a hosted HTTP API that gives your app everything it needs to build
an anime streaming or discovery product, without running any scrapers yourself:

- **Metadata** — trending, popular, seasonal, genres, full anime detail, seasons,
  episodes, characters and the airing schedule (powered by AniList, normalized).
- **Streaming** — resolved, **ad‑free** HLS streams with **multiple servers**
  (HD‑1, HD‑2…), **SUB & DUB**, subtitle tracks, intro/outro skip timestamps and
  preview thumbnails — all served through our proxy so they play anywhere.
- **Clean, stable JSON** — one consistent shape, CORS‑enabled, ready for web and
  mobile clients.

**Base URL**

```
https://api.emnexanime.site
```

Health check: `GET /health`

---

## Authentication

Send your API key in a header (recommended) or query param:

```
x-api-key: YOUR_API_KEY
```
```
GET /api/trending?api_key=YOUR_API_KEY
```

No key? Every IP gets a small **free trial** so you can evaluate the API before
signing up. For production usage, [get a key](#-get-access--contact).

---

## Quickstart

```bash
# Trending anime
curl "https://api.emnexanime.site/api/trending" -H "x-api-key: YOUR_API_KEY"

# Search
curl "https://api.emnexanime.site/api/search/solo%20leveling" -H "x-api-key: YOUR_API_KEY"

# Full detail (AniList id)
curl "https://api.emnexanime.site/api/anime/151807" -H "x-api-key: YOUR_API_KEY"

# Available servers for episode 1
curl "https://api.emnexanime.site/api/servers/151807/1" -H "x-api-key: YOUR_API_KEY"

# Resolve an ad-free stream (SUB)
curl "https://api.emnexanime.site/api/sources/151807/1?type=sub" -H "x-api-key: YOUR_API_KEY"
```

---

## Endpoints

All endpoints return JSON. `{id}` is an **AniList** id; a MAL id can be used on
streaming routes via the `/mal/{malId}` form.

### Discover
| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/home` | Curated homepage rails (trending, popular, seasonal, …) |
| GET | `/api/trending` | Trending now |
| GET | `/api/popular` | All‑time popular |
| GET | `/api/recent` | Recently released |
| GET | `/api/seasonal?season=WINTER&year=2024` | A specific anime season |
| GET | `/api/genres` | All genres |
| GET | `/api/genre/{genre}` | Anime in a genre |
| GET | `/api/schedule` | Airing schedule (next episodes) |

### Anime
| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/anime/{id}` | Full detail — synopsis, genres, studios, score, relations, recommendations, characters |
| GET | `/api/anime/{id}/seasons` | Season chain (prequels/sequels) |
| GET | `/api/anime/{id}/episodes` | Episode list |
| GET | `/api/anime/{id}/characters` | Characters + voice actors |

### Search
| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/search/{query}` | Search by title (URL‑encode the query) |

### Streaming
| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/servers/{id}/{ep}` | Servers available for an episode (per SUB/DUB) |
| GET | `/api/servers/mal/{malId}/{ep}` | Same, keyed by MAL id |
| GET | `/api/sources/{id}/{ep}?type=sub\|dub[&provider=]` | Resolve a playable ad‑free stream |
| GET | `/api/sources/mal/{malId}/{ep}?type=sub\|dub` | Same, keyed by MAL id |

`type` defaults to `sub`. Pin a specific server with `&provider=` (e.g. the
`provider` value returned by `/api/servers`).

---

## Response shapes

**Anime (metadata card)** — returned by discover/search/detail lists:

```jsonc
{
  "id": 151807,
  "idMal": 52299,
  "title": "Solo Leveling",
  "titles": { "romaji": "Ore dake Level Up na Ken", "english": "Solo Leveling", "native": "…" },
  "type": "anime",
  "format": "TV",
  "status": "FINISHED",
  "episodes": 12,
  "duration": 24,
  "season": "WINTER",
  "year": 2024,
  "rating": 83,
  "poster": "https://…",
  "genres": ["Action", "Adventure", "Fantasy"]
}
```

**Servers** — `GET /api/servers/{id}/{ep}`:

```jsonc
{ "servers": [
  { "provider": "megaplay", "type": "sub" },
  { "provider": "megaplay", "type": "dub" },
  { "provider": "anikoto",  "type": "sub" },
  { "provider": "anikoto",  "type": "dub" }
] }
```

**Source** — `GET /api/sources/{id}/{ep}?type=sub`:

```jsonc
{
  "provider": "megaplay",
  "type": "sub",
  "hls": "https://api.emnexanime.site/hls?u=…",      // ad-free, proxied HLS master
  "subtitles": [ { "lang": "English", "url": "https://…", "default": true }, … ],
  "intro": { "start": 0,    "end": 83  },             // seconds — for a Skip Intro button
  "outro": { "start": 1325, "end": 1414 },
  "thumbnailsVtt": "https://…",                        // scrubbing thumbnails (WebVTT)
  "image": "https://…"                                 // episode still
}
```

Play the `hls` URL directly in `hls.js`, Video.js, ExoPlayer, AVPlayer, etc. —
it is already proxied and CORS‑enabled, so no extra headers are needed.

---

## Plans & pricing

| Plan | Price | Requests / month | Rate | Support |
|---|---|---|---|---|
| **Free** | $0 | 1,000 | 2 req/s | Community |
| **Starter** | $5 / mo | 10,000 | 8 req/s | Standard + usage dashboard |
| **Pro** | $15 / mo | 100,000 | 20 req/s | Priority |

Need more (dedicated throughput, SLA, custom providers, white‑label)? **Enterprise
plans are available — [contact us](#-get-access--contact).** Billing is handled
securely via crypto (USDC on Polygon); keys activate automatically on payment.

---

## Rate limits & errors

- Each plan has a monthly request quota and a per‑second rate.
- Errors use standard HTTP status codes with a JSON body:

```jsonc
{ "error": "quota exceeded" }
```

| Status | Meaning |
|---|---|
| `401` | Missing / invalid API key |
| `402` | Quota exceeded — upgrade your plan |
| `404` | Unknown endpoint or resource |
| `429` | Rate limited — slow down |
| `502` | Upstream temporarily unavailable — retry |

---

## 📩 Get access & contact

To request an **API key**, upgrade a plan, or discuss a **custom / enterprise**
integration:

- **Email:** `contact@emnexanime.site`
- **Website:** https://api.emnexanime.site
- Or open a **[Discussion](../../discussions)** / **[Issue](../../issues)** on this repo.

Tell us your use case (app type, expected volume, SUB/DUB needs) and we'll get you set up quickly.

---

<div align="center">
<sub>EmnexAnime API · metadata via AniList · streams resolved ad‑free · built for developers.</sub>
</div>
