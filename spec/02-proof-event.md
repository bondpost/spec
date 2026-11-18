# 02 · Proof event

Status: Draft · Protocol version: 1

The proof is one addressable Nostr event ([NIP-01](https://nips.4rs.nl/nips/1)), signed by the identity the bond backs. It carries only what a verifier needs to rebuild the script.

## Format

```json
{
  "kind": 38421,
  "pubkey": "<nostr_pk, 64 hex characters>",
  "created_at": 1794870000,
  "tags": [
    ["d", "<txid>:<vout>"],
    ["v", "1"],
    ["network", "regtest"],
    ["base", "<base_pk, 66 hex characters>"],
    ["locktime", "1296"],
    ["amount", "500000"]
  ],
  "content": ""
}
```

The event's `pubkey` is the `nostr_pk` used in the key tweak.

## Tags

| Tag | Required | Rule |
| --- | --- | --- |
| `d` | Yes | Lower-case hex txid in display order (64 characters), a colon, then the output index in decimal with no leading zeros |
| `v` | Yes | Protocol version, the string `"1"` |
| `network` | Yes | `mainnet`, `signet` or `regtest` |
| `base` | Yes | `base_pk` as 66 lower-case hex characters, compressed encoding (prefix `02` or `03`) |
| `locktime` | Yes | `L` in decimal with no leading zeros |
| `amount` | No | Advisory amount in sats. Verifiers MUST NOT use it. See [03](03-verification.md) |
| `expiration` | No | [NIP-40](https://nips.4rs.nl/nips/40) timestamp: an estimate of when `L` arrives, so relays can drop stale proofs |

- Each required tag MUST appear exactly once. A verifier MUST treat a missing, duplicated or malformed required tag as `malformed`.
- Verifiers MUST ignore `content`. It MAY hold a human-readable note.
- Verifiers MUST ignore tags they do not recognise.

## Kind

Kind `38421` is **provisional**. It sits in the addressable range (30000 to 39999) but has not been checked against the live NIPs registry. See [D-3](../decisions/D-3-event-kind.md).

## Replacement and discovery

- Republishing an event with the same `d` replaces the earlier one, as with any addressable event.
- Clients find an identity's bonds with the filter `{"kinds":[38421],"authors":["<nostr_pk>"]}`.

## Referencing a bond from an offer

Offers need no change. Weight attaches to the public key that signed the offer.

- An offer MAY include `["a", "38421:<nostr_pk>:<txid>:<vout>", "<relay hint>"]` to help clients find the proof.
- A verifier MUST ignore an `a` reference whose `nostr_pk` differs from the offer's author.
- NIP-69 order events already use a tag named `bond` for a trade security deposit. Bondpost events and references MUST NOT use a tag named `bond`.
