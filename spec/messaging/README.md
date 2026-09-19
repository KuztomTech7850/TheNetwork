# Spec — Messaging Module

> Status: Draft | Version: 0.1 | Date: 2026-06-05
>
> **v0.2 note:** Matrix is implemented as an adapter behind an abstract
> `ConversationProvider` interface, introduced at Phase 6 — not a core
> dependency. In-app asynchronous comments, mentions, and notifications
> ship first (Phase 4/6). See
> [`docs/adr/0005-matrix-as-adapter.md`](../../docs/adr/0005-matrix-as-adapter.md)
> and [`spec/core/integration-contracts.md`](../core/integration-contracts.md).

## Objective
Provide federated, encrypted communication for community members
without routing messages through a central operator.

## Primary Technology
Matrix protocol / Element (open source, MIT licensed)

## Scope
- Matrix homeserver deployment model (community-run vs. delegated)
- Identity bridge: ATProto DID ↔ Matrix user ID
- Encryption: E2E by default (Matrix's Megolm)
- Federation between Network instances
- Moderation tooling scoped to community operators

## Open Items
- [ ] DID–Matrix identity bridge design
- [ ] Homeserver hosting expectations for community operators