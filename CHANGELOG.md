# Changelog

A high‑level history of developer‑facing changes to the EmnexAnime API.
We follow a stability promise: documented response fields are **additive** —
existing fields won't be removed or renamed without a deprecation notice here.

> Endpoint and response details are shared with customers — DM
> **[@emnextech](https://t.me/emnextech)** on Telegram for the full docs.

## 2026‑11
- **Multi‑server streaming** — sources now expose **two servers** (HD‑1, HD‑2)
  for both SUB and DUB, with the option to pin a specific server.
- **Wider stream coverage** via a second resolver — more titles return a playable
  source.
- **Skip data** — source responses include intro / outro timestamps for
  Skip‑Intro / Skip‑Outro buttons.

## 2026‑10
- Public launch: normalized anime metadata (discovery, detail, seasons, episodes,
  characters, schedule, search) + ad‑free resolved HLS sources with subtitle
  tracks and preview thumbnails.
- API keys, monthly quotas, and a free trial.
