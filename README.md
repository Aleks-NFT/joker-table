# The Table — Joker V4 Hook Market Dashboard

Static dashboard for the Uniswap V4 hook market, dealt daily by DJ.
Hosted on GitHub Pages. Data is pushed hourly by `joker_export_dashboard.py`
from `dj-hermes-prod-01` (Session 2 of the rollout).

## Data contract (`/data`)

All files carry `schema_version` and are regenerated atomically by the exporter.

| File | Purpose | Key fields |
|---|---|---|
| `meta.json` | Freshness | `generated_at` (ISO UTC), `mock` flag |
| `pools.json` | Market pulse | `canonical[]` (pinned tokens), `top_pools[]`, deploy counters, `liq_class`/`vol_class` |
| `digest.json` | DJ Daily Take | `entries[]`: `date`, `narrative`, `take`, `prediction_id` |
| `predictions.json` | Track record | `stats`, `items[]`: `status` ∈ open, hit, miss, void |
| `ticker.json` | Event feed | `items[]`: `ts`, `suit` ∈ spade, heart, diamond, club, `source`, `text`, `url` |

Suit semantics: ♠ infra, ♥ community, ♦ market, ♣ code.

## Rules

- Frontend is static, no build step, vanilla JS.
- If `generated_at` is older than 3 hours the UI shows a "stale deck" badge.
- No price predictions ever appear in digest or predictions — exporter enforces it.
- Visual identity is owned by the Claude Design pipeline; this repo carries the structural baseline.
