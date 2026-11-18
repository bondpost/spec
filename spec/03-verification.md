# 03 · Verification

Status: Draft · Protocol version: 1

A verifier needs three things: the proof event, the current tip height, and a way to look up one unspent output.

## Steps

The steps run in order, and the first failure ends the check with that step's code. Exactly one code is returned for a failed proof.

| Step | Check | Failure code |
| --- | --- | --- |
| 1 | The event id and signature are valid under NIP-01 | `bad_event` |
| 2 | The kind is the bond kind, `v` is `1`, and every required tag is present exactly once and well formed ([02](02-proof-event.md)) | `malformed` |
| 3 | `network` equals the verifier's own network | `wrong_network` |
| 4 | `0 < L < 500,000,000` | `bad_locktime` |
| 5 | Recompute `t` and `K` from `base` and the event's `pubkey` ([01](01-bond-construction.md)): `t` is in range, `base` is a valid point, and `K` is not infinity | `bad_key` |
| 6 | Rebuild the witness script and scriptPubKey `S` from `K` and `L` | (none) |
| 7 | The outpoint in `d` exists and is unspent | `not_found_or_spent` |
| 8 | The output's scriptPubKey equals `S` | `script_mismatch` |
| 9 | The output has at least `min_conf` confirmations ([06](06-networks.md)) | `unconfirmed` |
| 10 | `L > h` | `expired` |

The order is normative, so that every implementation returns the same code for the same input. For example, a proof for an expired bond whose output has already been spent returns `not_found_or_spent`, not `expired`.

## Result

On success a verifier returns:

| Field | Source |
| --- | --- |
| `outpoint` | The `d` tag |
| `amount` | The chain. Never the `amount` tag |
| `locktime` | `L` |
| `confirmations` | The chain |
| `remaining` | `L − h` |
| `weight` | As defined in [04](04-weight.md) |

## Rules that follow from the design

- The amount MUST come from the chain. The `amount` tag is advisory only.
- Consensus forbids spending the output before height `L`. So a bond verified once at `min_conf` stays valid until `L`, unless a reorganisation deeper than `min_conf` removes it.
- A verifier MAY cache a success until `h ≥ L`. It does not need to poll for spends.
- A verifier that caches MUST re-run steps 7 to 9 for bonds whose funding was confirmed in blocks a reorganisation has replaced.
- Once `h ≥ L`, the bond is expired whether or not it has been spent, and it contributes no weight.
- Every field of an event received from a relay is untrusted. Verifiers MUST check lengths and formats before decoding.

## Chain sources (non-normative)

Verification needs two operations: the tip height, and a lookup of `(script_pubkey, amount, confirmations)` for an unspent outpoint. Any source that provides them will do.

| Source | Lookup | Trust |
| --- | --- | --- |
| Bitcoin Core RPC | `gettxout` with the txid and index, plus `getblockcount` | Your own node, no third party |
| Esplora HTTP API | Transaction, output spend status, tip height | The server operator. Query two or more servers and fail closed if they disagree |

Bitcoin Core reports values in BTC. Convert them to sats with an integer-exact parser.
