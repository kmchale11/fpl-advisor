## ADDED Requirements

### Requirement: Valid squad generation
The system SHALL generate a 15-player squad that satisfies the current season's FPL squad rules — position
quotas, budget and maximum players per club — read from configuration ingested from the FPL API.

#### Scenario: Default build
- **WHEN** the user taps "Build my squad" with default settings
- **THEN** a squad of 2 GK, 5 DEF, 5 MID, 3 FWD is returned
- **AND** total cost is within the budget
- **AND** no more than 3 players come from any one club

### Requirement: Optimisation objective
The system SHALL choose the squad that maximises total projected points of the best starting XI over a
user-chosen horizon (default 6 gameweeks), with bench players weighted lower than starters.

#### Scenario: Horizon change
- **WHEN** the user changes the horizon from 6 to 3 gameweeks
- **THEN** the squad is rebuilt using projections for the next 3 gameweeks only

### Requirement: User constraints
The system SHALL let the user lock players in, exclude players, set a minimum bank to leave, and choose a
bench budget style (cheap bench vs playable bench).

#### Scenario: Locked player
- **WHEN** the user locks a player in
- **THEN** every generated squad includes that player

#### Scenario: Impossible constraints
- **WHEN** locks and exclusions make a valid squad impossible
- **THEN** the app says which constraint cannot be met instead of returning an invalid squad

### Requirement: Explain the squad
The app SHALL show why each player was picked: projected points over the horizon, price, upcoming fixtures and
Opponent History for the first fixture.

#### Scenario: Tap a pick
- **WHEN** the user taps a player in the generated squad
- **THEN** the reasons and the next-best alternative at that position and price are shown

### Requirement: Manual editing
The user SHALL be able to swap any player and see the squad's validity, remaining budget and projected points
update immediately.

#### Scenario: Swap breaks a rule
- **WHEN** a swap would put 4 players from one club in the squad
- **THEN** the swap is blocked with the reason
