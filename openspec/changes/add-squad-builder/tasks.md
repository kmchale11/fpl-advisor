## 1. Projection
- [ ] 1.1 Feature pipeline: form per 90, expected minutes, fixture difficulty, Opponent History (shrunk by sample size)
- [ ] 1.2 Availability scaling from `status` / `chance_of_playing_next_round`
- [ ] 1.3 Per-input contribution breakdown in the API response
- [ ] 1.4 Backtest on 2025-26 vs "last 5 average" baseline; record MAE per position

## 2. Optimiser
- [ ] 2.1 Integer linear programme (e.g. `glpk.js` or `highs` in the API) with quota, budget and club constraints read from config
- [ ] 2.2 Objective: best-XI projected points over horizon, bench weighted
- [ ] 2.3 Locks, exclusions, minimum bank, bench style
- [ ] 2.4 Infeasibility explanation
- [ ] 2.5 Tests: constraints always satisfied across 1,000 random lock/exclude sets

## 3. Mobile
- [ ] 3.1 Build settings screen (horizon, locks, exclusions, bench style)
- [ ] 3.2 Pitch view of generated squad with per-player reasons
- [ ] 3.3 Swap flow with live validity, budget and projection

## 4. Verify
- [ ] 4.1 `openspec validate add-squad-builder --strict`
