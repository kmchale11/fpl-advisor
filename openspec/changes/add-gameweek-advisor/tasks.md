## 1. Team import
- [ ] 1.1 Ingest `entry/{id}/`, `entry/{id}/history/`, `entry/{id}/event/{gw}/picks/`, `entry/{id}/transfers/` via backend
- [ ] 1.2 Selling-price calculation from purchase price and current price (FPL sell-on rule from config)
- [ ] 1.3 Confirm-state screen: free transfers, transfers made since last deadline

## 2. Advisor engine
- [ ] 2.1 Captain/vice ranking from projections (double/blank aware)
- [ ] 2.2 XI + bench optimiser with formation rules
- [ ] 2.3 Transfer search: best 0/1/2-transfer options over horizon with hit cost from config, per-club and budget checks
- [ ] 2.4 Chip evaluator using chip config from `bootstrap-static` and blank/double detection from fixtures
- [ ] 2.5 Tests: chip availability across chip windows; transfer options always affordable

## 3. Mobile
- [ ] 3.1 "This week" screen: captain, XI, transfers, chips — each with Opponent History badges
- [ ] 3.2 Accept-transfer flow updating local squad with "make this change in FPL" reminder
- [ ] 3.3 Opt-in deadline notifications (Expo Notifications)

## 4. Verify
- [ ] 4.1 Replay 5 past gameweeks of a real team and compare advice with what happened
- [ ] 4.2 `openspec validate add-gameweek-advisor --strict`
