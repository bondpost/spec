# References

The rules in this specification depend on these standards. They were checked against published copies on 17 November 2026, and the pages read are listed under the table.

**Read**

- [defiads repository and README](https://github.com/defiads/defiads): the original design, pay-to-contract and timelock.
- [bitcoin-dev thread, September 2019](https://mirror.b10c.me/lists/bitcoindev/f3d9000a-beef-a10c-5aa4-30bbda966c24@gmail.com/T): Tamas Blummer's announcement and ZmnSCPxj's replies.

**Standards used**

Checked this session: BIP65, BIP340, BIP46, Miniscript, NIP-01, NIP-40, NIP-46, NIP-69 and NIP-99. Not re-checked, because they are long-settled: BIP32, BIP39, BIP141, BIP143 and BIP173. Read those in full before coding.

| Standard | Used for |
| --- | --- |
| BIP32, BIP39 | Seed and key derivation |
| BIP65 | OP\_CHECKLOCKTIMEVERIFY and the nLockTime rules |
| BIP141, BIP143, BIP173 | P2WSH outputs, signature digest, bech32 addresses |
| BIP340 | Tagged hashes and x-only public keys |
| BIP46 | Prior art: JoinMarket's timelocked fidelity bond addresses |
| Miniscript | The descriptor form of the bond script |
| NIP-01 | Event format, signatures, addressable events |
| NIP-40 | The `expiration` tag |
| NIP-46 | Remote signing |
| NIP-69 (kind 38383) | Peer-to-peer order events, a first integration target |
| NIP-99 (kind 30402) | Classified listings, a first integration target |

**Pages read for the checks**

- [BIP65](https://bips.dev/65/) and its [bitcoin.org copy](https://bitcoin.org/bip/65/): the five failure conditions, including the sequence rule.
- [BIP340 tagged hash](https://www.github.com/JeremyRubin/bips/blob/master/bip-0340.mediawiki), as restated in BIP341.
- [BIP46](https://bips.dev/46/): path, script, certificate scheme, status Draft.
- [Miniscript, BIP379](https://bips.dev/379/): the translation table for `pk`, `after`, `and_v` and the `v:` wrapper.
- [Bitcoin Core IsFinalTx](https://git.sudo.is/mirrors/bitcoin/src/branch/master/src/consensus/tx_verify.cpp): a locktime below the next block height is final.
- [Bitcoin Core gettxout](https://bitcoincore.org/en/doc/27.0.0/rpc/blockchain/gettxout): null when spent or unknown, value in BTC.
- [Esplora API](https://raw.githubusercontent.com/Blockstream/esplora/master/API.md): transaction, outspend and tip height endpoints.
- [Full replace-by-fee history](https://www.spark.money/glossary/full-rbf): default in Core 28, unconditional in Core 29.
- [NIP-01](https://nips.4rs.nl/nips/1), [NIP-40](https://nips.4rs.nl/nips/40), [NIP-46](https://nips.4rs.nl/nips/46), [NIP-69](https://nips.4rs.nl/nips/69) and [NIP-99](https://nips.4rs.nl/nips/99).
- [Nostr kinds table, June 2026 copy](https://d-central.tech/fr/nostr-event-kinds-reference/).
- [strfry plugin documentation](https://github.com/hoytech/strfry/blob/beta/docs/plugins.md): JSON over stdin and stdout, reload when the script changes.
- [JoinMarket fidelity bond mathematics](https://gist.github.com/chris-belcher/87ebbcbb639686057a389acb9ab3e25b): bond value proportional to the square of the coins.
- [OrangeCheck protocol repository](https://github.com/orangecheck/oc-attest-protocol) and its [relay filter guide](https://oc-docs.vercel.app/attest/guides/nostr-relay).
- Crate pages for [nostr-sdk](https://docs.rs/crate/nostr-sdk/latest) and [bdk\_wallet](https://docs.rs/bdk_wallet). The other crates named were seen only as dependencies of these.

Kind 38421 does not appear in a June 2026 copy of the NIPs kind table. That is not a reservation: Mostro already uses the unlisted kind 38384, so check the live registry and search relays before signet.
