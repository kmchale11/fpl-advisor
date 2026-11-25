# Change: Data foundation and Opponent History

## Why
Every decision the app helps with (squad building, captaincy, transfers, chips) depends on reliable player,
fixture and historical data. The headline feature — "how has this player scored against their next opponent
over the last two seasons?" — is not available from the official FPL API alone: its `element-summary`
endpoint returns the current season per fixture but only **season totals** for past seasons
(`history_past`). Past per-fixture data has to come from an archived dataset.

## What Changes
- **ADDED** `data-ingestion` capability: scheduled backend jobs that pull current-season data from the FPL API
  and past-season gameweek data from the `vaastav/Fantasy-Premier-League` dataset, normalise IDs across
  seasons, and store everything in Postgres.
- **ADDED** `opponent-history` capability: for a player and an upcoming fixture, return up to 4 past fixtures
  against that opponent (home and away, previous two completed seasons) with points and the underlying stats,
  plus the player's normal average for comparison.
- **ADDED** a backend read API the mobile app calls (the app never calls FPL directly).
- **ADDED** Opponent History panel on the player screen in the mobile app.

## Impact
- Affected specs: `data-ingestion` (new), `opponent-history` (new)
- New systems: Postgres schema, ingestion jobs, backend API, Expo app shell
- Risks:
  - FPL API is unofficial and can change without notice → ingestion validates response shape and alerts on change.
  - The vaastav dataset is community-maintained → pin to a commit per season and cache locally; the two
    seasons needed (2024-25, 2025-26) are complete and won't change.
  - Branding: "Fantasy Premier League", "FPL" and the Premier League marks are trademarks. The app name and
    store listing must not use them as the product name; describe compatibility instead.
