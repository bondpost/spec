# D-5 · Licence

Status: **Accepted** for this repository (18 November 2026) · Still open for the `bondpost` implementation repository

## Context

The goal is for others to adopt the format and copy its text into a draft NIP or a future BIP. Ecosystem practice:

- The BOLTs use CC-BY-4.0.
- rust-bitcoin, rust-secp256k1 and rust-miniscript use CC0-1.0.
- BIP2 lists both as acceptable for BIPs.

## Options

1. CC0-1.0: public domain dedication, no conditions.
2. CC-BY-4.0: free reuse with attribution.
3. MIT or Apache-2.0: the same licence as the code. These are software licences, so they fit documents poorly.

## Decision

**The spec text and the test vectors are CC0-1.0.** See [`LICENSE`](../LICENSE).

CC0 removes every barrier to re-publishing the text in a NIP or BIP and to bundling the vectors with any implementation, whatever that implementation's own licence is. Attribution is still given in the README as a courtesy, but it is not required.

## Still open

The licence for the implementation (`bondpost` repository). Proposed: MIT or Apache-2.0 dual licence, matching BDK and LDK.
