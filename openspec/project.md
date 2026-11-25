# Project Context

## Purpose
FPL Advisor is a mobile app that helps Fantasy Premier League managers make decisions:
1. **Season start** — build an initial 15-player squad within FPL rules and budget.
2. **Week to week** — captaincy, transfers, chip timing and bench order, backed by data.

The signature feature is **Opponent History**: for every player and their next opponent, show how that
player scored against that club in the previous two completed Premier League seasons (up to 4 fixtures:
home and away in each season).

The app advises; it never changes the user's real FPL team. The user makes moves in the official FPL app.

## Tech Stack
- Mobile: React Native with Expo, TypeScript (iOS + Android from one codebase)
- Backend: TypeScript (Node) API serving pre-computed data to the app
- Database: Postgres (managed, e.g. Supabase or Neon)
- Scheduled jobs: ingestion runs on a cron (daily, plus hourly on gameweek days)
- Agent: Claude Code runs OpenSpec stages (proposal → apply → archive)

## Data Sources
| Source | Used for | Notes |
|---|---|---|
| Official FPL API (`fantasy.premierleague.com/api/`) | Current players, prices, teams, fixtures, gameweeks, chips, live points, a manager's public picks | Unofficial/undocumented; no published rate limit. Never called from the mobile app directly — backend ingests and caches. |
| `vaastav/Fantasy-Premier-League` GitHub dataset | Gameweek-by-gameweek player points for past seasons (`data/<season>/gws/merged_gw.csv`) | The official API only returns past seasons as season totals (`history_past`), so this is the source for past per-fixture data. Credit the dataset in the app's About screen as the maintainer requests. |

## Domain Rules (FPL, read from config not hard-coded where the API exposes them)
- Squad: 15 players — 2 GK, 5 DEF, 5 MID, 3 FWD; max 3 per Premier League club; budget £100.0m at season start.
- Starting XI: 1 GK, min 3 DEF, min 2 MID, min 1 FWD.
- Chips, chip windows, transfer rules and scoring rules MUST be read from `bootstrap-static` where available,
  because FPL changes them between seasons.

## Conventions
- Requirements use SHALL/MUST; every requirement has at least one `#### Scenario:`.
- Player identity across seasons uses the FPL `code` field (stable), never the per-season `id`/`element`.
- Small samples are shown with their sample size; the app never presents 1–4 games as a prediction on its own.
- Money shown in £m to one decimal place, as in FPL.

## Out of Scope (v1)
- Making transfers or setting the team in the user's real FPL account
- Head-to-head or draft FPL formats
- Paid data feeds (Opta etc.)
