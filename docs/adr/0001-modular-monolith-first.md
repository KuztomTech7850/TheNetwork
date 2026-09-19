# ADR-0001: Modular Monolith First

> Status: Accepted
> Date: 2026-09-18

## Context

The repository's original architecture called for AT Protocol, AO
compute, Arweave/Irys permanence, an EVM-compatible L2, and Matrix
federation to operate together from the start. Integrating five
distributed systems before validating any family-hub workflow would
substantially increase operational and security risk and delay proof
that the core domain (membership, records, decisions) even works.

## Decision

Build a single deployable application (a modular monolith) divided into
modules with explicit interfaces:

- Identity and access
- Communities and membership
- Documents and records
- Posts and discussions
- Proposals and voting
- Notifications
- Audit events
- Integration adapters

Modules communicate through application services and domain events, not
direct cross-module database access, so they can be extracted into
separate services later without redesigning the data model.

Do not begin with AO compute, an EVM L2, Arweave, independent ATProto
infrastructure, and Matrix federation all operating simultaneously.

## Consequences

- Transactional consistency across modules (single database transaction
  boundary) during the MVP phase.
- Simpler debugging, backups, and operations — one deployable, one
  datastore.
- Distributed-systems integrations become adapters, gated by the phase
  sequence in `ROADMAP.md`, not prerequisites.
- Risk: module boundaries must be kept honest (no leaking internals
  across module lines) or extraction later becomes costly. Enforced via
  code review and the module-interface convention documented in
  `spec/core/integration-contracts.md`.

## Alternatives Considered

- **Microservices/distributed-first (original design):** rejected for
  the MVP — too much operational surface before the domain is proven.
- **Fully generic CMS/social platform:** rejected — would not naturally
  produce the typed governance/records model the family Trust use case
  needs.
