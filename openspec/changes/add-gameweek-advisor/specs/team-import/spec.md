## ADDED Requirements

### Requirement: Import by team ID
The system SHALL import a manager's most recent public squad, bank value, chips used and transfer history
using their FPL team ID, without asking for FPL login details.

#### Scenario: Valid team ID
- **WHEN** the user enters a valid team ID
- **THEN** the app shows the 15 players from the latest public picks, the bank, and which chips have been used this season

#### Scenario: Unknown team ID
- **WHEN** the FPL API returns not found for the ID
- **THEN** the app explains where to find the team ID in the FPL site URL

### Requirement: Confirm current state
The app SHALL ask the user to confirm free transfers available and any transfers already made since the
imported picks, before giving advice.

#### Scenario: Transfer made after last deadline
- **WHEN** the user has already transferred a player this gameweek
- **THEN** they can swap that player in the imported squad and the bank updates

### Requirement: Remember the team
The app SHALL store the team ID on the device and refresh the import after each gameweek deadline.

#### Scenario: New gameweek
- **WHEN** a deadline passes
- **THEN** the next app open refreshes the squad from the new public picks
