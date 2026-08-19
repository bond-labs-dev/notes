# A premium that is real, large, robust every year — and still untradeable

**Kind:** market
**Outcome:** DEAD as a strategy; the signal is confirmed

## What was claimed

Implied volatility systematically exceeds subsequently-realized volatility, so a
short-volatility position should earn a persistent, carry-like premium. This is
one of the best-documented effects in finance, and it is not controversial.

The screen was not asking whether the premium exists. It was asking whether it
survives contact with execution.

## What the numbers said

Daily, 2021-03-24 → 2026-06, a 30-day implied-vol index against subsequently
realized vol on the corresponding perpetuals.

The premium is real and it is large:

- implied minus forward-realized, BTC: **+8.8 volatility points**, hit rate
  **73%**
- the same on ETH: **+4.4 volatility points**
- positive in every year of the sample

The tradeable stream looked strong too. The best close-to-close gross
construction was **robust in every year, Sharpe 2.51**.

Then three things killed it, independently:

| bind | number |
|---|---|
| crash tail | skew **−5.4** → **DSR 0.18** |
| execution cost | at a realistic fill assumption, Sharpe **−0.14**, five materially losing years |
| breakeven | costs of **~20–25% of theta** flip the sign |
| venue | none available |

The Deflated Sharpe Ratio is the decisive one. A Sharpe of 2.51 that arrives
with a skew of −5.4 is a strategy that collects small amounts steadily and hands
it back in one move; the deflation is not a technicality, it is the tail being
priced in.

## What killed it in the end

Not the signal. The signal passed everything asked of it and was upgraded from
"unknown" to "confirmed".

What failed is the distance between a premium existing and a premium being
collectable by us: the payoff shape is short-convexity, the cost sensitivity is
steep enough that a fifth of theta reverses it, and there is no venue in reach to
express it on.

## The rule this produced

1. A premium being real, large and per-year robust is not evidence that it is
   tradeable, and a Sharpe reported without its skew is not a description of the
   payoff.
2. Any short-convexity stream is graded on the tail — deflated Sharpe, drawdown,
   CVaR — never on the mean.
3. Cost sensitivity is reported as the fraction of the edge at which the sign
   flips, not as a single assumed fee. "Dies at 20–25% of theta" is a decision;
   "Sharpe 2.51 net of 5bp" is a hope.
4. Where the effect is real but the access is missing, the verdict is recorded
   against *our* constraints and says so, so that a later change in access
   reopens the row rather than requiring the work again.

## Why this is here

It is the cheapest illustration of the wall most of the corpus dies on. Almost
nothing in the log died because the effect was imaginary. Things die because the
effect is smaller than the cost of reaching it, or lives at a horizon, venue or
size we cannot occupy. A research process that only knows how to ask "is it
real?" will keep producing findings like this one and mistaking them for
opportunities.
