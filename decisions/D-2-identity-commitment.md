# D-2 · Identity commitment

Status: Open · Affects: [01](../spec/01-bond-construction.md), [05](../spec/05-reclaim-and-recovery.md)

## Context

The output has to commit to one Nostr key. Version 1 tweaks the spending key (pay-to-contract). Signing then needs the raw tweaked private key, which hardware wallets do not expose, so version 1 is hot-wallet only.

## Options

1. **Key tweak (proposed, current draft):** `K = P + t·G`. Compact, and the script reveals nothing about the commitment.
2. **Hash inside the script:** commit to `nostr_pk` with a hash that the script drops or checks. The key stays an ordinary wallet key, which suits hardware wallets that sign Miniscript (Ledger, Coldcard, Jade).
3. **BIP46-style certificate:** the cold key signs a certificate once, and a hot key acts for the bond until the certificate expires.

## Related

The derivation purpose `8421'` is a placeholder. A BIP or SLIP number, if one is ever assigned, would replace it in a new protocol version.

## Decision

Not yet made.
