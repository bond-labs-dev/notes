# notes

Write-ups from an autonomous quant research loop — what was claimed, the
numbers, and the rule each one produced.

The loop runs unattended: hypotheses are pre-registered, screened, put through a
statistical gate, argued against by an independent reviewer, and given one
verdict that is appended to a log nothing is removed from. Most entries do not
survive it.

Nothing here describes a strategy that is being traded, which is why these can be
shown whole. They are a sample, chosen because the lesson transfers past the
market they were run in.

## Write-ups

| | What it is about |
|---|---|
| [The gate was wrong, and the strategy still failed](dsr-pool-pollution.md) | A deflation pool contaminated by an unrelated dead family manufactured a benchmark higher than the candidate's own Sharpe, and killed two live verdicts |
| [Eight bugs that make a backtest profitable, and none of them crash](silent-backtest-bugs.md) | The mistakes that pass every numeric gate — and what it cost to turn them into checks nobody learns to ignore |
| [A premium that is real, large, robust every year — and still untradeable](real-premium-untradeable.md) | A textbook risk premium confirmed on every axis asked of it, killed by its own tail, its cost sensitivity, and the absence of a venue |

## Related

- [sharpe-gate](https://github.com/bond-labs-dev/sharpe-gate) — the trial
  registry that keeps the trial count honest, and the gates that spend it
- [explicit-backtest](https://github.com/bond-labs-dev/explicit-backtest) — the
  execution engine, where every assumption it cannot derive from the data is
  stated rather than implied
- [hyperliquid-data](https://github.com/bond-labs-dev/hyperliquid-data),
  [lighter-data](https://github.com/bond-labs-dev/lighter-data) — ingest
