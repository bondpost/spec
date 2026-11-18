# 07 · Security and privacy

Status: Draft · Protocol version: 1

Fund safety outranks every other property. No conforming wallet may send coins to a script it cannot later spend.

## Threat model

| Threat | Defence |
| --- | --- |
| One bond claimed for two identities | The tweak binds `K` to one `nostr_pk` ([03](03-verification.md) step 5) |
| Proof for coins the claimant does not control | Harmless: whoever funded the script locked real coins for that identity |
| Regtest or signet proof replayed on mainnet | The `network` tag check. The outpoint does not exist on the other chain either |
| Proof kept alive after spending | Impossible before `L` by consensus. At `L` and after, the bond is expired by rule |
| Unconfirmed or double-spent funding | `min_conf`, plus re-checking after a reorganisation |
| Inflated `amount` tag | Verifiers take the amount from the chain |
| Malformed event crashing a verifier | Treat all input as hostile, check lengths, fuzz the parser |
| Relay withholds proof events | Query several relays. The bonder can hand over the event directly |
| Lying Esplora server | Several servers that must agree, or a full node |
| Stolen machine with a hot wallet | Seed encryption, and the timelock itself: a thief cannot move bonded coins before `L` |

## What a bond does not prove

A bond proves that an identity committed capital for a period of time. It does not prove honesty, and nobody can claim the coins if the bonder misbehaves. Interfaces MUST NOT suggest otherwise ([04](04-weight.md)).

Weight scales with wealth, so a large holder can outrank smaller ones cheaply. Clients SHOULD combine weight with their own signals.

## Privacy

A proof publicly links a Nostr identity to an output, and through it to the coins that funded it.

- Wallets MUST warn the bonder of this before funding.
- Documentation SHOULD advise funding from coins the bonder is content to have associated with that identity.

## Implementation safety rules

These rules bind wallets that create bonds. They do not bind verifiers.

- Run the reclaim dry run before showing any address, with no way to skip it ([05](05-reclaim-and-recovery.md)).
- Check `k·G = K` before signing.
- Push `L` with the library's minimal integer encoding. Never encode it by hand.
- Use single SHA256 for the witness program.
- Use integer sats everywhere.
- Use audited libraries for all curve, hash and signature operations.
