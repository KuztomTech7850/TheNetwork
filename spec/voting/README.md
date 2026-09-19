# Spec — Voting Module

> Status: Draft | Version: 0.1 | Date: 2026-06-05
>
> **v0.2 note:** This module is superseded by
> [`spec/governance/README.md`](../governance/README.md), which defines
> the full Proposal → Ballot → Tally → DecisionRecord lifecycle,
> starting with visible/signed ballots and a deterministic application
> tally (Phase 5). AO compute, Arweave vote-proof storage, and on-chain
> anchoring below apply only at Phase 8 (cryptographic voting and
> external anchoring), not to the initial implementation. See
> [`docs/adr/0007-defer-transferable-token.md`](../../docs/adr/0007-defer-transferable-token.md)
> for the related token-weighted-voting prohibition.

## Objective
Define proposal creation, ballot submission, tally computation, and
result anchoring for egalitarian community governance.

## Depends On
- Identity module (voter identity and Sybil resistance)
- Resolution of Q3 (Vote Privacy) and Q4 (Sybil Resistance)
- AO compute layer for tally processing
- Arweave for vote proof storage

## Scope
- ATProto Lexicon schemas for proposals and ballots
- AO actor design for tally processes
- Vote proof upload to Arweave
- Final tally hash anchored on-chain
- AppView indexing of vote records

## Open Items
- [ ] Vote privacy model (Q3)
- [ ] Sybil resistance / proof of personhood (Q4)
- [ ] Governance model (Q5 — one-person-one-vote vs. delegated)
- [ ] Lexicon schema draft