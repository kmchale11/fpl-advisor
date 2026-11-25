# Change: Season-start Squad Builder

## Why
Building the initial 15 is the single biggest decision of the season and the one new managers find hardest:
balancing a £100m budget, position quotas, the 3-per-club limit, and early fixtures. Doing it once with good
reasoning saves transfers later.

## What Changes
- **ADDED** `player-projection` capability: an explainable expected-points estimate per player per gameweek,
  combining recent form, minutes security, fixture difficulty and Opponent History.
- **ADDED** `squad-builder` capability: generate a valid 15-player squad that maximises projected points over a
  chosen horizon (default first 6 gameweeks), with locks, exclusions and a playing-style setting.
- **ADDED** squad builder screens in the mobile app; the user can then edit the squad by hand.

## Impact
- Affected specs: `player-projection` (new), `squad-builder` (new)
- Depends on: `add-data-foundation-and-opponent-history`
- Before GW1 there is no current-season form, so projections lean on last season's per-90 output and
  Opponent History; this must be visible to the user.
