# 06 · Network parameters

Status: Draft · Protocol version: 1

All networks share one set of rules and differ only in the constants below.

| Parameter | `regtest` | `signet` | `mainnet` | Normative? |
| --- | --- | --- | --- | --- |
| Address human-readable part | `bcrt` | `tb` | `bc` | Yes |
| BIP44 `coin_type` for base keys | 1 | 1 | 0 | Recommendation |
| Default `min_conf` | 1 | 1 | 6 | Default, verifier MAY raise it |
| Maximum term and weight cap | 52,560 blocks | 52,560 blocks | 52,560 blocks | Weight cap: yes |
| Minimum bond amount for weight | 10,000 sats | 10,000 sats | 10,000 sats | Proposal |

Notes:

- `bcrt` is a Bitcoin Core convention, not part of BIP173. Signet shares `tb` with testnet.
- A proof for one network MUST NOT verify on another (step 3 of [03](03-verification.md)). The outpoint also would not exist on the other chain.
- 52,560 is 144 blocks a day × 365 days.

## Implementation limits (non-normative)

These are not protocol rules, but implementations SHOULD apply them:

- Refuse to create a bond with a term above 52,560 blocks unless the user overrides explicitly.
- During the mainnet beta, refuse bonds above the beta cap (Proposal: 1,000,000 sats, see [D-4](../decisions/D-4-mainnet-cap.md)).
- Keep wallet data for each network in separate storage.
