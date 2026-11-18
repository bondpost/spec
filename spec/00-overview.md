# 00 · Overview

Status: Draft · Protocol version: 1

## Abstract

A Bondpost bond is a P2WSH output, timelocked to an absolute block height, whose spending key commits to one Nostr public key. The owner of that Nostr key publishes a signed proof event. Anyone holding the event and a view of the Bitcoin chain can confirm that the stated identity has a given number of sats locked until height L. They can then turn that into a comparable number, the bond weight.

## Goals

- Anyone can create a bond, publish its proof, and get every satoshi back after expiry without trusting a third party.
- Anyone can verify a proof using only the published event and a view of the Bitcoin chain.
- Verified bonds convert to one comparable number by a published default formula.
- The format is specified well enough to build a second, independent implementation.

## Non-goals

- No token, no new chain, no change to Bitcoin consensus.
- No custody, slashing, escrow, matching or trade settlement.
- No judgement of offer content. A bond shows committed cost. It says nothing about honesty.

## Conventions

The key words MUST, MUST NOT, REQUIRED, SHOULD, SHOULD NOT, RECOMMENDED, MAY and OPTIONAL are to be read as described in [RFC 2119](https://www.rfc-editor.org/rfc/rfc2119) and [RFC 8174](https://www.rfc-editor.org/rfc/rfc8174) when, and only when, they appear in capitals.

Everything in documents 01 to 07 is normative unless it is marked **(non-normative)** or **Proposal**.

- `||` is byte concatenation.
- Hex is lower case, with no `0x` prefix, unless stated otherwise.
- Txids are written in display order: the byte-reversed form that block explorers and RPC interfaces use.
- Amounts are integer satoshis. Implementations MUST NOT use floating point for amounts.
- `G` is the secp256k1 generator, and `n` is the curve order.

## Terminology

| Term | Meaning |
| --- | --- |
| Bond | One output paying a Bondpost script, identified by its outpoint `txid:vout` |
| Bonder | The person who locks coins and controls the Nostr identity the bond backs |
| Nostr key, `nostr_pk` | The bonder's 32-byte x-only public key ([BIP340](https://github.com/bitcoin/bips/blob/master/bip-0340.mediawiki)) |
| Base key, `P` / `base_pk` | A 33-byte compressed secp256k1 public key from the bonder's wallet |
| Bond key, `K` | `P` tweaked by a commitment to `nostr_pk`. See [01](01-bond-construction.md) |
| Locktime, `L` | The absolute block height before which the bond cannot be spent |
| Tip height, `h` | The height of the best block known to the verifier |
| Proof event | The Nostr event that publishes a bond. See [02](02-proof-event.md) |
| Verifier | Any software that checks a proof under [03](03-verification.md). It is a role, not a server |
| Weight | The default measure of committed cost, in sat-blocks. See [04](04-weight.md) |
| Expired | A bond with `h ≥ L`, whether or not it has been spent |

## Lifecycle (non-normative)

1. **Derive.** The bonder's wallet derives `K` from `P` and `nostr_pk`, builds the script for `L`, and produces an address.
2. **Fund.** Any transaction pays the address. Each output that pays it is a separate bond.
3. **Publish.** The Nostr identity signs a proof event naming the outpoint, `P` and `L`, and sends it to relays.
4. **Verify.** A verifier rebuilds the script from the event, looks up the outpoint, and computes the weight.
5. **Reclaim.** Once `h ≥ L`, the bonder spends the output with one signature from the tweaked private key.

Steps 4 and 5 are independent. The bonder needs only a chain source to reclaim, not relays or verifiers.

## Versioning

- The protocol version is the `v` tag of the proof event. This document set describes version `1`.
- A verifier MUST reject a proof whose version it does not implement (failure code `malformed`). This leaves version 2 free to change the script, the tweak or the tags.
- Changing a derivation path, a tag string or the script changes every address. Any such change needs a new protocol version.
- Releases of this repository are tagged `spec-vMAJOR.MINOR.PATCH`. Implementations SHOULD state the spec release they implement.

## Normative references

BIP32, BIP39, BIP65, BIP141, BIP143, BIP173, BIP340, Miniscript (BIP379), NIP-01, NIP-40.

## Informative references

BIP43, BIP44, BIP46, NIP-46, NIP-69, NIP-99, defiads, OrangeCheck. Links are in the [README](../README.md) and in each document.
