# Changelog

All notable, developer‑facing changes to the EmnexAnime API are documented here.
We follow a stability promise: documented response fields are **additive** —
existing fields won't be removed or renamed without a deprecation notice here.

## 2026‑11
- **Multi‑server streaming.** `/api/servers` and sources now expose **two servers**
  (HD‑1, HD‑2) for both SUB and DUB. Pin one with `?provider=` on `/api/sources`.
- **Wider stream coverage** via a second resolver — more titles return a playable
  source.
- **Skip data.** Source responses include `intro` / `outro` timestamps for
  Skip‑Intro / Skip‑Outro buttons.

## 2026‑10
- Public launch: AniList‑powered metadata (home, trending, popular, seasonal,
  genres, detail, seasons, episodes, characters, schedule, search) + ad‑free
  resolved HLS sources with subtitle tracks and preview thumbnails.
- API keys, monthly quotas, and a free per‑IP trial.
