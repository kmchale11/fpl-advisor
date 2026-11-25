## ADDED Requirements

### Requirement: Opponent History lookup
The system SHALL return, for a player and an upcoming fixture, the player's FPL performances against that
fixture's opponent in the two most recently completed Premier League seasons — up to four fixtures, home and
away in each season.

#### Scenario: Full history available
- **GIVEN** the player played all four league games against the opponent in 2024-25 and 2025-26
- **WHEN** the app requests Opponent History for the player's next fixture
- **THEN** four rows are returned, each with season, gameweek, home/away, the player's club at the time, minutes, total points, goals, assists, clean sheet and bonus

#### Scenario: Player did not feature
- **WHEN** the player had 0 minutes in one of those fixtures
- **THEN** that row is returned marked "Did not play"
- **AND** it is excluded from the average

#### Scenario: Opponent not in the Premier League
- **GIVEN** the next opponent was promoted for 2026-27 and was not in the PL in 2024-25 or 2025-26
- **WHEN** Opponent History is requested
- **THEN** no fixture rows are returned
- **AND** each season shows the reason "Opponent not in Premier League"

#### Scenario: Player new to the Premier League
- **WHEN** the player has no Premier League appearances in the past two seasons
- **THEN** the response states there is no history rather than returning zeros

### Requirement: Context for small samples
The system SHALL show the sample size and the player's overall average points per appearance over the same two
seasons alongside the average against the opponent.

#### Scenario: Comparison shown
- **WHEN** Opponent History has 3 appearances averaging 8.0 points and the player's overall average is 5.2
- **THEN** the app shows "8.0 pts avg vs this opponent (3 games)" and "5.2 pts avg overall"

#### Scenario: One appearance
- **WHEN** only one appearance exists
- **THEN** the app labels the average "Low sample"

### Requirement: Double and blank gameweeks
The system SHALL return Opponent History for every fixture a player has in the next gameweek.

#### Scenario: Double gameweek
- **WHEN** the player's team has two fixtures in the next gameweek
- **THEN** a separate Opponent History block is returned for each opponent

#### Scenario: Blank gameweek
- **WHEN** the player's team has no fixture in the next gameweek
- **THEN** the app shows "No fixture" and offers the following gameweek's opponent instead

### Requirement: Opponent History on the player screen
The mobile app SHALL show Opponent History on each player's detail screen and as a compact indicator in squad
and transfer lists.

#### Scenario: Compact indicator
- **WHEN** a player appears in the squad list
- **THEN** a small badge shows average points vs the next opponent and the appearance count, tappable to open the full panel
