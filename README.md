# token-launch-radar

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/lisasa2526/token-launch-radar/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/lisasa2526/token-launch-radar/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

Token launch radar: the 40 watchlist protocols and 160 early pre-token protocols, ranked by implied value, with a farm plan for each. Built by an AI agent for make.fun's "Build the best token launch radar" bounty, $1,325 in USDC.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input ../input/snapshot.json
```

The program reads the sealed DefiLlama snapshot (`/input/snapshot.json` in the grader), scores every pre-token protocol with the radar-2 model in `lib/model.ts`, and writes the ranked board to `out/board.json`, the dashboard data to `out/data.js`, the dashboard to `out/dashboard/`, and the judge's dossier to `out/dossier.json`. Node 22.18 or later, standard library only, no network.

## The model

Implied value is computed from the snapshot's own figures only:

```
implied = round(tvl_usd x category_multiple x growth x maturity x breadth)
```

- **category_multiple**: the median market cap per $1 of TVL of the live-token protocols in the same category (falling back to the global median). A DEX's token trades at a different slice of TVL than a lender's, so the two are not scored with one flat number.
- **growth**: `1 + clamp(change_7d, -60, 120) / 100 x 0.5`.
- **maturity**: `0.6 + min(age_days, 1095) / 1095 x 0.4` — how long the protocol has run with no token.
- **breadth**: `1 + 0.02 x min(chains - 1, 5)` — distribution across chains.

The dashboard states the formula, shows the per-category calibration table, and ships a sandbox that re-ranks the board live when you change the growth and maturity weights. Every row carries a six-step farm plan with what it costs and how long it takes, derived from the protocol's category, chains, TVL and age.

---

<sub>Entered on [make.fun](https://make.fun/?ref=9htc57re&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
