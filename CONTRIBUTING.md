# Contributing

## Licence of contributions

This repository is CC0-1.0. By opening a pull request, you dedicate your contribution to the public domain under the same terms.

## Proposing a change

1. Open an issue that describes the problem before you write spec text. This applies to anything that changes behaviour.
2. In the same pull request, change the spec document, the affected test vectors and the `CHANGELOG.md` entry.
3. A change that alters addresses, scripts, tags or verification results needs a new protocol version. Say so in the pull request title.
4. A design question with real alternatives gets a decision file in `decisions/`.

## Document status

| Status | Meaning |
| --- | --- |
| Draft | Under active change. Do not lock mainnet coins under it |
| Proposed | Feature-complete, with test vectors and at least one implementation passing them. Open for outside review |
| Final | Reviewed. Changes only through a new protocol version |

A document moves from Draft to Proposed when its vectors exist and the reference implementation passes them. It moves to Final after independent review and a second implementation or integration.

## Style

- Use RFC 2119 keywords for rules, and keep each rule in one sentence.
- Put the reasoning in decision files, or in sections marked non-normative.
- Name each check, and give each failure exactly one code.
