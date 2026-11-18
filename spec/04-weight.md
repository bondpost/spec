# 04 · Bond weight

Status: Draft · Protocol version: 1 · Depends on decision [D-1](../decisions/D-1-weight-formula.md)

Weight is the protocol's output. What to do with it is policy, and each client or relay decides that for itself.

## Bond weight

The default weight of a verified bond is its amount times the blocks it still has locked, capped at one year:

```
w = a × min(L − h, 52560)
```

Here `a` is the amount in sats read from the chain, `L` is the locktime and `h` is the tip height. The unit is **sat-blocks**. Implementations MUST compute it in integer arithmetic of at least 64 bits.

Bonds below the minimum amount of 10,000 sats have weight 0 (**Proposal**).

## Identity weight

An identity's weight is the sum of the weights of its verified bonds, where each bond is a distinct outpoint whose proof event is signed by that identity.

## Display (non-normative)

To convert sat-blocks to BTC-days, divide by 14,400,000,000 (10⁸ sats × 144 blocks).

Worked example: 500,000 sats with 4,320 blocks remaining (about 30 days) gives 2,160,000,000 sat-blocks, or 0.15 BTC-days.

## Properties

- Weight falls every block and reaches zero at expiry. A bonder renews to keep their rank.
- The cap stops a very long lock from buying outsized weight.
- The formula is linear in amount, so splitting coins across identities neither helps nor hurts. Decision D-1 considers squaring the amount, as JoinMarket does.

## Use

- A relay MAY reject or rate-limit events from identities below a minimum weight. It SHOULD always accept bond proof events themselves.
- A client MAY sort by weight or blend it with other signals.
- Interfaces MUST describe weight as cost committed. They MUST NOT present it as a trust, reputation or safety score.
