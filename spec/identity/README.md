# Spec — Identity Module

> Status: Draft | Version: 0.1 | Date: 2026-06-05

## Objective
Define how a user's identity is created, stored, updated, and verified
across Network instances without dependence on any single operator.

## Depends On
- Resolution of Q1 (DID Method) in [`docs/open-questions.md`](../../docs/open-questions.md)
- Resolution of Q2 (Blockchain selection) in [`docs/open-questions.md`](../../docs/open-questions.md)

## Scope
- DID creation and key management
- PDS setup (self-hosted vs. delegated host)
- Profile capsule schema (local-first, encrypted)
- DID–blockchain address binding
- Key rotation and recovery

## Open Items
- [ ] DID method decision (Q1)
- [ ] Key recovery UX (social vouching vs. backup seed)
- [ ] Mapping between ATProto DID and EVM address