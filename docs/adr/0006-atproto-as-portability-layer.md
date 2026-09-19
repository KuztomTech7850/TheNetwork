# ADR-0006: ATProto as Portability Layer

> Status: Accepted
> Date: 2026-09-18

## Context

The original architecture made AT Protocol the primary data layer
(Layer 2, signed ATProto records as the system of record) from the
start. Building the core domain directly on ATProto Lexicons before the
internal schema is stable would couple every schema change to
AppView-indexing compatibility, and would require running PDS/AppView/
relay infrastructure before the family-hub workflows are validated.

## Decision

- The modular monolith's PostgreSQL schema (via `spec/core/domain-model.md`)
  is the system of record for Phases 0–6.
- **ATProto becomes a portability and federation layer** (Phase 7):
  an export/import package format is defined first
  (`spec/federation/README.md`), and ATProto repositories/Lexicons are
  introduced as one way to realize that portability and to resolve
  remote identity — not as the only way to persist data.
- Exit condition for Phase 7: export/import and remote identity
  resolution are demonstrated using the internal schema mapped to
  Lexicons, without requiring a schema rewrite.

## Consequences

- Internal schema changes during Phases 0–6 do not risk breaking
  AppView indexing, because no AppView exists yet to break.
- When Phase 7 begins, `spec/core/domain-model.md` must be stable enough
  that a Lexicon mapping is a translation, not a redesign.
- A profile created in one Network instance working in any other Network
  instance (the README's portability promise) is fulfilled via the
  export/import package plus ATProto identity resolution, not by ATProto
  being the primary datastore from day one.

## Alternatives Considered

- **ATProto as primary data layer from day one (original design):**
  rejected for the MVP — too much infrastructure and schema-stability
  risk before the domain is validated.
- **No ATProto integration at all:** rejected — federation and portable
  identity remain core promises of the Network; ATProto is deferred, not
  dropped.
