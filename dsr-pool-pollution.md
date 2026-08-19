# The gate was wrong, and the strategy still failed

**Kind:** methodology, not market
**Outcome:** the stated kill reason was refuted; the verdict held anyway

## What was claimed

A textbook daily trend strategy had been marked down earlier with a Deflated
Sharpe Ratio of 0.372, and the stated reason was that the Sharpe was too low to
clear the multiple-testing haircut. A re-defend asked a narrow question: was the
stop we had dropped hiding something better?

It answered no. It also showed the original reason for the kill was an artifact
of our own gate.

## What the numbers said

The Deflated Sharpe Ratio discounts a result by the dispersion across the trials
you ran — the more you searched, the higher the bar a winner has to clear. Our
registry pooled trials by the dataset they touched, so a different, already-dead
strategy family that happened to use the same price series landed in the same
pool.

| pool | n_eff | sqrt(V̂[SR]) ann | SR0 ann | DSR |
|---|---|---|---|---|
| as run — all signals, including the dead family | 12 | 0.595 | 0.99 | **0.216** |
| own family only | 3 | 0.137 | 0.12 | **0.992** |
| no haircut (PSR) | 1 | — | 0 | 0.998 |

The dead family's dispersion inflated the cross-trial variance until the implied
benchmark Sharpe reached 0.99 annualised — higher than the candidate's own
Sharpe. The gate was asking the strategy to beat a threshold manufactured by
unrelated failures. 0.216 was not a measurement; it was contamination.

## What killed it in the end

Nothing was rescued. Scored against its own family the alpha is statistically
real, but the configuration that would actually be deployed still misses, at
DSR 0.913, and it is decaying and dominated by buy-and-hold. The verdict stayed
exactly where it was.

Only the reason changed: not "the signal is too weak to survive the haircut" but
"the economics do not clear". Those are different findings, and only one of them
was true.

## The rule this produced

1. Any result sharing a dataset with a dead sibling prints the own-family DSR
   beside the pooled one. Two numbers, always, so contamination is visible
   rather than inferred.
2. Promotion is denied on the deploy configuration's own DSR — never on the best
   cell of the grid.
3. A homogeneity guard on the pool: trials whose Sharpe distributions are not
   comparable do not belong in the same deflation.

## Why this is here

A falsification layer that is never wrong is a layer nobody checked. This one was
wrong in the direction that kills live results, it was caught by re-deriving a
verdict instead of trusting it, and the correction did not conveniently produce a
winner.
