## ADDED Requirements

### Requirement: Captaincy ranking
The system SHALL rank the user's starting players for captaincy by projected points for the gameweek, and show
the top five with Opponent History, fixture, and a recommended captain and vice-captain.

#### Scenario: Weekly captain pick
- **WHEN** the user opens "This week"
- **THEN** the top five captain options are listed with projected points, next opponent, home/away and Opponent History average with sample size

#### Scenario: Double gameweek captain
- **WHEN** a player has two fixtures
- **THEN** their projection sums both fixtures and both Opponent History blocks are shown

### Requirement: Starting XI and bench order
The system SHALL recommend a valid starting XI and bench order that maximise projected points, respecting
formation rules.

#### Scenario: Injured starter
- **WHEN** a player in the current XI is flagged unavailable
- **THEN** the recommendation benches them and explains why

### Requirement: Transfer suggestions
The system SHALL suggest the best 0, 1 and 2-transfer options over a user-chosen horizon (default 3
gameweeks), showing projected gain, the points-hit cost for transfers beyond the free ones, and the net gain.
Hit cost SHALL be read from configuration.

#### Scenario: Roll the transfer
- **WHEN** no transfer beats keeping the squad by more than a configurable threshold
- **THEN** the top recommendation is "Save your transfer" with the reason

#### Scenario: Hit worth taking
- **WHEN** a second transfer's projected gain over the horizon exceeds the hit cost
- **THEN** the option is shown with gross gain, hit cost and net gain

#### Scenario: Budget respected
- **WHEN** suggestions are generated
- **THEN** every option is affordable using the bank plus selling prices, and respects the per-club limit

### Requirement: Chip advice
The system SHALL show, for each chip still available to the user, whether this gameweek is a good time to use
it, based on the chip rules and windows ingested for the current season and on upcoming blank and double
gameweeks.

#### Scenario: Bench Boost in a double gameweek
- **WHEN** Bench Boost is available and most of the user's 15 have two fixtures
- **THEN** the app shows the projected bench points and flags this gameweek as a strong option

#### Scenario: Chip already used
- **WHEN** a chip has been used in the current chip window
- **THEN** it is shown as unavailable with the gameweek it was used

#### Scenario: Save the chip
- **WHEN** a better gameweek for the chip exists within the visible fixture horizon
- **THEN** the app recommends waiting and names that gameweek

### Requirement: Advice, not action
The app SHALL NOT make changes to the user's FPL team. Every recommendation ends with the user making the
change in the official FPL app or site.

#### Scenario: Apply a transfer
- **WHEN** the user accepts a suggested transfer
- **THEN** the app updates its local copy of the squad and reminds the user to make the transfer in FPL before the deadline

### Requirement: Deadline reminder
The app SHALL offer an opt-in push notification a configurable time before each gameweek deadline.

#### Scenario: Reminder set
- **WHEN** the user opts in with a 3-hour reminder
- **THEN** a notification arrives 3 hours before each deadline in the user's local time zone
