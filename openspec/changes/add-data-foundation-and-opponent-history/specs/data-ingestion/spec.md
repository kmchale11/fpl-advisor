## ADDED Requirements

### Requirement: Current-season ingestion
The system SHALL ingest players, teams, gameweeks, fixtures, chip configuration and per-fixture player
history for the current season from the FPL API on a schedule, and the mobile app SHALL NOT call the FPL
API directly.

#### Scenario: Daily refresh
- **WHEN** the daily ingestion job runs
- **THEN** players, prices, teams, gameweeks, fixtures and chip configuration are upserted from `bootstrap-static` and `fixtures`
- **AND** the job records its start time, end time and row counts

#### Scenario: Match-day refresh
- **WHEN** a gameweek is in progress
- **THEN** per-player fixture history is refreshed at least hourly

#### Scenario: FPL API changes shape
- **WHEN** a response is missing a field the schema requires
- **THEN** the job stops without overwriting existing data
- **AND** raises an alert naming the endpoint and missing field

### Requirement: Past-season ingestion
The system SHALL load gameweek-by-gameweek player data for the two most recently completed seasons from the
`vaastav/Fantasy-Premier-League` dataset, pinned to a specific commit.

#### Scenario: Backfill on setup
- **WHEN** the backfill job runs for season 2025-26
- **THEN** every row of `merged_gw.csv` is stored as a `player_fixture` with `player_code` and `opponent_team_code` resolved via that season's `players_raw.csv` and `teams.csv`

#### Scenario: Unresolvable row
- **WHEN** a gameweek row's player or opponent ID has no match in that season's lookup files
- **THEN** the row is written to a rejects table with the reason
- **AND** the backfill reports the reject count

### Requirement: Stable identity across seasons
The system SHALL identify players and teams across seasons by their stable FPL `code`, never by the
per-season `id`.

#### Scenario: Player changed club
- **WHEN** a player was at Club A in 2024-25 and Club B in 2026-27
- **THEN** both seasons' fixtures are linked to the same `player_code`
- **AND** each fixture records the club the player was at when it was played

### Requirement: Rate limiting and backoff
The system SHALL limit concurrent FPL API requests to a configurable maximum (default 2) and back off
exponentially on HTTP 429 or 5xx responses.

#### Scenario: Throttled
- **WHEN** the FPL API returns 429
- **THEN** the request is retried after an increasing delay, up to a configured retry limit
