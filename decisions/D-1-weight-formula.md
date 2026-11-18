# D-1 · Weight formula

Status: Open · Affects: [04](../spec/04-weight.md)

## Context

The default weight has to turn amount and remaining time into one number that clients can compare.

## Options

1. **Linear (proposed):** `w = a × min(L − h, 52560)`. Simple, and splitting coins across identities neither helps nor hurts.
2. **Squared in amount**, as JoinMarket does: `w ∝ a² × f(time)`. Discourages splitting one large holding into many identities, but favours large holders even more. See [Belcher's bond mathematics](https://gist.github.com/chris-belcher/87ebbcbb639686057a389acb9ab3e25b).

For short terms JoinMarket's time term is close to linear, so the two options agree on time and differ only on amount.

## Decision

Not yet made.
