# 05 · Reclaim and recovery

Status: Draft · Protocol version: 1

Once `h ≥ L`, the bonder spends the output with one signature from the bond private key `k`.

## Reclaim transaction

| Field | Value |
| --- | --- |
| Version | 2 |
| Input | The bond outpoint, with `nSequence = 0xfffffffd` |
| `nLockTime` | `L`, or the highest `L` when sweeping several bonds |
| Output | Amount less fee, to any destination the bonder chooses |
| Witness | `<signature> <witness_script>` |

- The signature is ECDSA by `k` over the [BIP143](https://bips.dev/143/) digest, with `SIGHASH_ALL` and `witness_script` as the script code.
- `nSequence` MUST NOT be `0xffffffff`, because that disables `nLockTime` and makes `OP_CHECKLOCKTIMEVERIFY` fail. `0xfffffffd` is RECOMMENDED because it matches common wallet behaviour.
- `nLockTime` MUST be a block height of at least `L`, and below 500,000,000.
- The transaction is final, and so relayable, once the next block's height is greater than `nLockTime`. In practice that is when the tip height is `L` or above.
- Several expired bonds MAY be swept in one transaction, with `nLockTime` set to the highest `L` among them.

## Dry run before funding

Before a wallet shows a bond address, it MUST build and sign a reclaim transaction against a dummy outpoint and check the signature against `K`. If this fails, the wallet MUST NOT show the address. No option may skip this check.

## Recovery

The proof event contains no secret, so publishing it also backs up everything except the seed.

| What the bonder still has | How the bond is rebuilt |
| --- | --- |
| Seed and a local bond record | Read the index, `nostr_pk`, `L` and outpoint from the record |
| Seed and Nostr access | Fetch the identity's proof events and match each `base` tag to a derived key index |
| Seed and `nostr_pk` only | Scan the default range below against the UTXO set |
| No seed | Not recoverable |

**Default scan range (Proposal):** key indexes 0 to 19, and every multiple of 144 for `L` from one year before to one year after the current tip. That is about 20 × 730 candidate scripts. A wallet that uses indexes or locktimes outside the defaults cannot rely on this scan.

## Limit in version 1

Signing needs the raw tweaked private key, and hardware wallets do not expose it. Version 1 is therefore hot-wallet only. Decision [D-2](../decisions/D-2-identity-commitment.md) describes routes to hardware wallet support.
