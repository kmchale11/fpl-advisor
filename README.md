# FPL Advisor

A mobile app that helps fantasy football managers make better decisions for the Premier League game:

- **Season start**: build a valid 15-player squad within budget, with reasons for every pick.
- **Week to week**: captain, starting XI, transfers (including whether a points hit is worth it) and chip timing.
- **Opponent History**: how each player scored against their next opponent over the previous two seasons (up to 4 games, home and away).

The app gives advice only. You make changes in the official game.

## How this repo is built
Spec-driven with [OpenSpec](https://github.com/Fission-AI/OpenSpec). Start with `openspec/project.md`, then the changes in build order:

1. `openspec/changes/add-data-foundation-and-opponent-history`
2. `openspec/changes/add-squad-builder`
3. `openspec/changes/add-gameweek-advisor`

```bash
npx @fission-ai/openspec list
npx @fission-ai/openspec validate --all --strict
```

## Planned stack
- Mobile: React Native (Expo), TypeScript
- Backend: Node, TypeScript, Postgres
- Data: the public Fantasy Premier League API (current season) and the
  [vaastav/Fantasy-Premier-League](https://github.com/vaastav/Fantasy-Premier-League) dataset (past seasons)

## Disclaimer
Not affiliated with or endorsed by the Premier League or Fantasy Premier League.
