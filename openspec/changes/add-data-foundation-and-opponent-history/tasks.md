## 1. Project setup
- [ ] 1.1 Monorepo: `apps/mobile` (Expo + TypeScript), `apps/api` (Node + TypeScript), `packages/shared` (types)
- [ ] 1.2 Postgres instance and migration tooling
- [ ] 1.3 CI: lint, typecheck, unit tests on every PR

## 2. Data ingestion
- [ ] 2.1 Typed FPL API client with response-shape validation (zod) and rate limiter/backoff
- [ ] 2.2 Migrations for season, team, team_season, player, player_season, player_fixture, fixture
- [ ] 2.3 Current-season job: bootstrap-static + fixtures (daily)
- [ ] 2.4 Current-season job: element-summary for all players (nightly, hourly on match days)
- [ ] 2.5 Past-season backfill from pinned vaastav commit for 2024-25 and 2025-26, with ID resolution and rejects table
- [ ] 2.6 Alert on shape change / job failure
- [ ] 2.7 Tests: ID resolution across seasons (player transferred club, team promoted/relegated)

## 3. Opponent History
- [ ] 3.1 Query: player appearances vs opponent_team_code in past two seasons
- [ ] 3.2 Averages (excluding 0-minute rows), overall average, sample size, per-season "not in PL" reasons
- [ ] 3.3 Double/blank gameweek handling
- [ ] 3.4 Precompute cache after each ingest
- [ ] 3.5 API: `GET /players/:code/opponent-history?gameweek=N`
- [ ] 3.6 Tests against known historical fixtures (hand-checked against FPL site data)

## 4. Mobile
- [ ] 4.1 App shell, navigation, player search
- [ ] 4.2 Player detail screen with Opponent History panel
- [ ] 4.3 Compact badge component for lists
- [ ] 4.4 About screen crediting the vaastav dataset; non-affiliation notice

## 5. Verify
- [ ] 5.1 Spot-check 10 players against the official site's past results
- [ ] 5.2 `openspec validate add-data-foundation-and-opponent-history --strict`
