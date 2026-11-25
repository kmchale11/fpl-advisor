# Design: Data foundation and Opponent History

## Context
Two sources, different shapes:

| Need | Source | Endpoint / file |
|---|---|---|
| Players, prices, positions, teams, gameweeks, chip config | FPL API | `bootstrap-static/` |
| Fixtures (incl. future) | FPL API | `fixtures/` |
| Current-season per-fixture points | FPL API | `element-summary/{id}/` → `history` |
| Past-season per-fixture points | vaastav dataset | `data/<season>/gws/merged_gw.csv` |
| Past-season player ID → stable code | vaastav dataset | `data/<season>/players_raw.csv` (`id`, `code`) |
| Past-season team IDs | vaastav dataset | `data/<season>/teams.csv` (`id`, `code`, `name`) |

## Key decisions

### 1. Identity across seasons
FPL's `id` (called `element` in gameweek files) is re-issued every season. Team `id`s 1–20 also shift as
clubs are promoted and relegated. We key everything on the stable `code` fields:
- `player_code` from `players_raw.code` / `bootstrap-static.elements[].code`
- `team_code` from `teams.csv.code` / `bootstrap-static.teams[].code`

Each season's rows are joined `merged_gw.element → players_raw.id → code` and
`merged_gw.opponent_team → teams.id → code` **within that season** before storing.

### 2. Which seasons count as "past two"
Computed, not hard-coded: the two most recent seasons that are fully complete before the current one.
In 2026-27 that is 2024-25 and 2025-26.

### 3. Opponent History calculation
For player P and upcoming fixture vs opponent O:
1. Select P's appearances (by `player_code`) in the past two seasons where `opponent_team_code = O`.
2. That yields at most 4 rows (H + A per season). Double fixtures (e.g. cup replays) are not in FPL data,
   so 4 is a true max.
3. Rows with `minutes = 0` are shown as "Did not play" and **excluded** from the average.
4. Return:
   - each fixture: season, gameweek, home/away, player's club at the time, minutes, total points, goals,
     assists, clean sheet, bonus, (defensive contribution points if present in that season's data)
   - `avg_points_vs_opponent` over appearances
   - `avg_points_overall` across all of P's appearances in the same two seasons
   - `appearances` count (the sample size)
5. If O was not in the Premier League in a season, return a reason for that season ("not in PL"), not zeros.

### 4. Ingestion schedule
- Past seasons: one-off backfill, re-run only when a new season completes.
- Current season: `bootstrap-static` and `fixtures` daily; `element-summary` for all players nightly;
  hourly on match days. Respect FPL with low concurrency (≤ 2 in-flight requests) and backoff on 429/5xx.

### 5. Serving
Backend pre-computes Opponent History for every player × next-fixture pair after each ingest and stores it,
so the app reads a single row per player. Response is cached with an ETag.

## Data model (sketch)
```
season(id, label, is_complete)
team(team_code PK, name, short_name)
team_season(season_id, team_code, fpl_team_id)
player(player_code PK, web_name, first_name, second_name)
player_season(season_id, player_code, fpl_element_id, team_code, position, start_price)
player_fixture(season_id, player_code, gameweek, fixture_id, opponent_team_code, was_home,
               minutes, total_points, goals, assists, clean_sheet, bonus, raw jsonb)
fixture(season_id, fixture_id, gameweek, kickoff_utc, home_team_code, away_team_code, fdr_home, fdr_away)
opponent_history_cache(player_code, fixture_id, payload jsonb, computed_at)
```

## Open questions
- Weight 2025-26 above 2024-25 in the average? (Proposal: show both seasons separately; v1 uses a plain mean.)
- Include the current season's earlier meeting (if already played) as a 5th row? (Proposal: yes, shown
  separately and labelled "this season".)
