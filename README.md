# Bondpost protocol specification

Bondpost lets a Nostr identity prove it has bitcoin locked until a future block height, so clients and relays can rank or filter its offers by a real, consensus-enforced cost. Nostr relays carry the proof and Bitcoin enforces the lock.

> **Status: Draft, protocol version 1.** Nothing here is stable. Event kind `38421` and derivation purpose `8421'` are placeholders that nothing has registered. Do not lock mainnet coins under this draft.

This repository holds the **protocol**: the rules that any implementation, in any language, has to follow to interoperate. The reference implementation (the `bondpost-core` library, the CLI, the verifier service, the relay plugin and the board) lives in the separate `bondpost` repository.

## Documents

| # | Document | Covers | Status |
| --- | --- | --- | --- |
| 00 | [Overview](spec/00-overview.md) | Scope, terminology, conventions, lifecycle, versioning | Draft |
| 01 | [Bond construction](spec/01-bond-construction.md) | Key tweak, witness script, address, funding | Draft |
| 02 | [Proof event](spec/02-proof-event.md) | The Nostr event that publishes a bond, and how offers reference it | Draft |
| 03 | [Verification](spec/03-verification.md) | The ten verification steps, failure codes, caching, chain sources | Draft |
| 04 | [Bond weight](spec/04-weight.md) | The default weight formula and how it may be used | Draft |
| 05 | [Reclaim and recovery](spec/05-reclaim-and-recovery.md) | Spending an expired bond, rebuilding bonds from a seed | Draft |
| 06 | [Network parameters](spec/06-networks.md) | Per-network constants | Draft |
| 07 | [Security and privacy](spec/07-security-and-privacy.md) | Threat model, privacy, implementation safety rules | Draft |

Supporting material:

- [`test-vectors/`](test-vectors/): fixed inputs and expected outputs. Every conforming implementation must pass them.
- [`decisions/`](decisions/): open and settled design decisions (D-1 to D-6), with the reasoning behind each one.
- [`CHANGELOG.md`](CHANGELOG.md): spec versions and breaking changes.
- [`CONTRIBUTING.md`](CONTRIBUTING.md): how changes are proposed and how a document moves from Draft to Final.

A draft NIP will be extracted from documents 02 to 04 for community review before the signet release.

## What this repository is not

- **Not the implementation.** CLI commands, the verifier HTTP API, relay plugin configuration and wallet storage formats are documented in the `bondpost` repository.
- **Not the project plan.** Business requirements, user stories and milestones also live with the implementation.

## Prior art and credit

The design comes from [defiads](https://github.com/defiads/defiads) by Tamas Blummer (2019), with input from ZmnSCPxj on the bitcoin-dev list, and from Chris Belcher's JoinMarket fidelity bonds ([BIP46](https://bips.dev/46/)). [OrangeCheck](https://github.com/orangecheck/oc-attest-protocol) is a neighbouring protocol: it attests to unspent coins rather than timelocked ones. See [decision D-6](decisions/D-6-orangecheck.md).

## Licence

Not yet chosen; see [decision D-5](decisions/D-5-licence.md).
