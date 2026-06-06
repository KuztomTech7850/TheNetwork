# Spec — Messaging Module

> Status: Draft | Version: 0.1 | Date: 2026-06-05

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