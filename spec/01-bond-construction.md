# 01 · Bond construction

Status: Draft · Protocol version: 1

A bond is one P2WSH output whose spending key is tweaked with the owner's Nostr public key and whose script is timelocked to a block height.

## Inputs

| Name | Type | Meaning |
| --- | --- | --- |
| `nostr_pk` | 32 bytes | The owner's Nostr public key (x-only, BIP340) |
| `base_pk` | 33 bytes | A compressed secp256k1 public key `P` from the bonder's wallet |
| `locktime` | integer | Absolute block height `L`, with `0 < L < 500,000,000` |
| `network` | text | `mainnet`, `signet` or `regtest` |

`L` MUST be below 500,000,000. Values at or above that are timestamps under [BIP65](https://bips.dev/65/) and are not allowed in version 1.

## Key tweak

1. Compute `t = tagged_hash("Bondpost/bond/v1", base_pk || nostr_pk)`, with `tagged_hash` as defined in BIP340. The input is 65 bytes: 33 for `base_pk`, then 32 for `nostr_pk`.
2. Read `t` as a 256-bit big-endian integer. If `t = 0` or `t ≥ n`, this `base_pk` is unusable and the wallet MUST pick another one.
3. Compute `K = P + t·G`. If `K` is the point at infinity, this `base_pk` is unusable and the wallet MUST pick another one.
4. The bond private key is `k = (p + t) mod n`, where `p` is the private key of `P`. A wallet MUST check that `k·G = K` before it uses `k`.

This is a pay-to-contract construction. Because `K` depends on `nostr_pk`, an output verifies for one identity only.

## Witness script

```
witness_script = <K> OP_CHECKSIGVERIFY <L> OP_CHECKLOCKTIMEVERIFY
scriptPubKey   = OP_0 <SHA256(witness_script)>
descriptor     = wsh(and_v(v:pk(K),after(L)))
```

- `K` MUST be pushed as its 33-byte compressed encoding.
- `L` MUST be pushed as a minimally encoded script number. A non-minimal push is non-standard and can make the output unspendable through normal relay.
- The script hash is a **single** SHA256, as BIP141 specifies. Double SHA256 is wrong here.
- The script is valid Miniscript. An implementation SHOULD cross-check its script against the descriptor above.

## Address

The address is the bech32 ([BIP173](https://bips.dev/173/)) encoding of the version 0 witness program for the network's human-readable part, as listed in [06](06-networks.md).

## Funding

- Any transaction MAY pay the address. Each output that pays it is a separate bond, identified by `txid:vout`.
- Two outputs that pay the same script are two bonds that share the same `K` and `L`.

## Base key derivation (wallet recommendation)

Verifiers never need this section. Wallets SHOULD follow it, because the seed-only recovery in [05](05-reclaim-and-recovery.md) depends on it.

- **Proposal:** derive `base_pk` at `m/8421'/coin_type'/account'/index`, with `coin_type` 0 on mainnet and 1 on every test network (BIP44). Putting it under its own purpose keeps it apart from the BIP44, BIP84 and BIP46 branches.
- `8421` is a placeholder that nothing has registered. If this protocol is ever assigned a BIP or SLIP number, the path will change in a new protocol version. See [D-2](../decisions/D-2-identity-commitment.md).
- Wallets SHOULD round `L` up to a multiple of 144. This keeps the seed-only recovery scan small.
