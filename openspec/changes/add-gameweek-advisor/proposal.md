# Change: Week-by-week Gameweek Advisor

## Why
After the squad is built, managers face the same decisions every week before the deadline: who to captain,
whether to transfer (and whether a points hit is worth it), the starting XI and bench order, and when to play
a chip. These decisions are where mini-leagues are won.

## What Changes
- **ADDED** `team-import` capability: load a manager's squad from their public FPL team ID, then let them
  confirm bank and free transfers.
- **ADDED** `gameweek-advisor` capability: captain and vice-captain ranking, starting XI and bench order,
  transfer suggestions with hit analysis, and chip timing advice — each with Opponent History shown.
- **ADDED** a pre-deadline "This week" screen and an optional deadline reminder notification.

## Impact
- Affected specs: `team-import` (new), `gameweek-advisor` (new)
- Depends on: `add-data-foundation-and-opponent-history`, `add-squad-builder` (projection)
- Constraint: the FPL endpoint for a manager's current unsaved team requires their FPL login. v1 does **not**
  ask for FPL credentials. It imports the latest public picks (the previous gameweek after its deadline) and
  the user edits in any transfers made since.
