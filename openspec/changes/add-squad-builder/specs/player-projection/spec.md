## ADDED Requirements

### Requirement: Explainable projection
The system SHALL produce an expected-points value for each player for each upcoming gameweek, and SHALL show
the contribution of each input to that value.

#### Scenario: Breakdown shown
- **WHEN** the user opens a player's projection for GW14
- **THEN** the app shows the projected points and the contribution from form, minutes security, fixture difficulty and Opponent History

### Requirement: Projection inputs
The projection SHALL use: recent form (points per 90 over the last N appearances), expected minutes,
fixture difficulty for the gameweek, and Opponent History adjusted for sample size, so that Opponent History
moves the projection less when there are fewer appearances.

#### Scenario: Strong history, small sample
- **GIVEN** a player averaged 12 points vs the opponent over 1 appearance
- **WHEN** the projection is calculated
- **THEN** Opponent History moves the projection less than it would for the same average over 4 appearances

#### Scenario: Pre-season
- **WHEN** no current-season fixtures have been played
- **THEN** form is taken from the previous season's per-90 output
- **AND** the projection is labelled "Based on last season"

### Requirement: Availability
The projection SHALL be zero for players flagged as unavailable and scaled by chance of playing for players
flagged as doubtful, using the FPL availability fields.

#### Scenario: Doubtful player
- **WHEN** a player has a 50% chance of playing
- **THEN** the projection is scaled to reflect it and the reason is shown

### Requirement: Backtesting
The projection model SHALL be backtested against at least one completed season before release, with the
error metric recorded.

#### Scenario: Backtest report
- **WHEN** the backtest runs on 2025-26
- **THEN** it reports mean absolute error per position and compares it with a "last 5 games average" baseline
