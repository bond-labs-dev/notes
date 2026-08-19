# Eight bugs that make a backtest profitable, and none of them crash

**Kind:** methodology, not market
**Outcome:** each one cost a full re-run; all eight are now static checks that
fire before a simulation starts

## What was claimed

Nothing. This is the list of mistakes that were made, found late, and converted
into checks.

They share one property, and it is the reason they are expensive: **they produce
plausible wrong results that pass the numeric gates.** A look-ahead leak has
excellent PnL. A thousand-fold time-unit error looks fine. Nothing raises, no
test fails, no chart looks odd — so the bug surfaces only after the run, which is
the most costly place to find it.

## The list

**Timestamps in the wrong unit.** One venue delivers fill times as
`timestamp[us]`. Taking the raw integer view returns microseconds, and any epoch
arithmetic written in nanoseconds is then silently off by 1000×. Windows of
"30 seconds" become 30 microseconds; the study still runs and still reports.

**As-of lookup that reaches into the future.** `searchsorted(T) - 1` returns the
bar *containing* T — a bar that closes after T. Every value read from it is
information the strategy did not have. An as-of bar has to be selected by
`open_time <= T - timeframe`.

**Pagination that stops on a short page.** Breaking the loop when a page comes
back shorter than the requested size truncates the pull at the venue's response
cap rather than at the true end of history. The result is a clean-looking dataset
that is quietly missing its tail. Break on an empty page, never a short one.

**Filling missing returns with zero.** `fillna(0)` fabricates a flat day for a
name that was delisted or not yet listed. That is survivorship bias plus an
invented zero — in a cross-sectional screen it shows up as a strategy that never
holds a loser. Mask instead.

**An unseeded stochastic fit.** Some fitting routines draw random starts from the
global RNG, so two identical runs disagree and the recorded trial cannot be
re-derived. Measured cost: the same configuration produced 70 and 65 gate-off
episodes on consecutive runs, and it was noticed by accident.

**An event study with no matched-anchor placebo.** A rank or correlation test
anchored on an event inherits the series' generic persistence for free, so a
significant result answers "does this series persist" rather than "is this event
informative". Measured cost: ρ = +0.415 with permutation p = 0.007 was read as a
real edge for two full runs. Matched against arbitrary anchors, the event anchor
was *worse* than a random one — paired difference −0.112. Three re-runs.

**The mean of a ratio.** One near-zero denominator dominates the average and
prints nonsense; a share statistic came out at −24.7%. Report the median of
ratio-shaped quantities.

**A rolling-rank gate whose warm-up is NaN.** `NaN > threshold` is False, so the
warm-up period is silently gated *off*. If one arm defaults its warm-up on and
another off, the difference being measured is the warm-up, not the sensor.

## What this actually cost to build

The checks were the easy part. Making checks that survive being read is not.

The path-layout check fired on every matching line in the corpus and every one
was a false positive — eleven spurious warnings across a single screen's edits.
The placebo check, in two earlier forms, fired eight times in one session on a
study whose nearest event word was seventeen lines from its nearest correlation
call, because the vocabulary it matched (`anchor`, `episode`) is also ordinary
statistical vocabulary.

**A check you learn to skip is worse than no check**, because it costs attention
and returns nothing, and it trains you to dismiss the one that matters. Both were
narrowed until they stopped crying wolf.

## The rule this produced

1. A mistake that a numeric gate cannot catch becomes a *static* check, run on
   the edited file, before any compute is spent.
2. The check is warn-only and silent when clean. It never blocks, so it is never
   worth disabling.
3. A check that produces a false positive is not left in place to be ignored — it
   is narrowed or removed. Precision matters more than coverage for anything a
   human reads on every edit.

## Why this is here

Every one of these is a mistake, not a technique. The list exists because the
alternative to writing it down is paying for the same run twice, and because a
falsification layer is only worth what its cheapest checks catch.
