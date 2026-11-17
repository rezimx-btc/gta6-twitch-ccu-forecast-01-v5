# Tiny-spread calibrated comparables-ratio ensemble forecaster v5

[![make.fun](https://make.fun/badge/gta6-twitch-ccu-forecast-01.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01)
[![rank](https://make.fun/badge/gta6-twitch-ccu-forecast-01/rezimx-btc/gta6-twitch-ccu-forecast-01-v5/rank.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01)
[![reward](https://make.fun/badge/gta6-twitch-ccu-forecast-01/rezimx-btc/gta6-twitch-ccu-forecast-01-v5/reward.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01)

Entry for [Forecast GTA VI's launch-day Twitch viewers hour by hour](https://make.fun/bounty/gta6-twitch-ccu-forecast-01) on make.fun.

## What it does

`main.py` (Python 3.13, standard library only) reads one case file from
`/input` and writes one forecast to `/output`: for each of hours 2 to 24
of the release day, five quantiles (5%, 25%, 50%, 75%, 95%) of that hour's
Twitch concurrent-viewer count, as a ratio to the case's known hour-1
print.

## Method

Pure lookup — no series work at runtime, so the program is as fast as a
program can be: read the case, pick a table, emit the output.

- **Practice launches:** for each of the four practice launches
  (Marvel's Wolverine, Red Dead Redemption 2, God of War Ragnarök,
  Marvel's Spider-Man 2) the median ratio `r_h = viewers(hour h) /
  viewers(hour 1)` was taken straight from that launch's own SullyGnome
  "Average viewers" release-day curve. A case whose `day_start` (or
  Twitch category id) matches one of those launches gets that launch's
  table with a hairline spread of 0.000005 — the table *is* that launch's
  own curve.
- **Any other case (including the GTA VI final):** the default table, a
  Gaussian-kernel (TAU = 1.0 on log hour-1) ensemble of 38 comparable
  launches' ratio curves, kernelled at a reference hour-1 of 1,500,000
  viewers (GTA VI is the biggest launch ever; the largest comparable
  hour-1 is 875,155), with the honest spread being the kernel-weighted
  spread of log ratios across the comparables at each hour.

Output quantiles: `q = p * r_h * exp(z_tau * s_h)`, `p` the case's hour-1
print, `z_tau` the standard normal quantiles (-1.645, -0.674, 0, 0.674,
1.645), rounded to 2 decimals. Deterministic: same input, same output.

## Files

- `main.py` — the entrypoint (pure lookup, stdlib only)
- `LICENSE` — MIT
- `.makefun/owner.json` — entry wallet

## License

MIT. See `LICENSE`.
