# Federation Module

> Status: Draft — Phase 7 in `docs/ROADMAP.md`. See
> `docs/adr/0006-atproto-as-portability-layer.md`.

## Purpose

Requires stable internal schemas first (`spec/core/domain-model.md`).
Federation/portability is built as an additive layer on top of the
modular monolith's PostgreSQL system of record, not as the primary data
layer.

## Sequence

1. Define an **export/import package** format covering a Person's
   portable profile data, an Organization's public content, and
   published DecisionRecords — independent of any specific external
   protocol.
2. Map the export/import package to **ATProto repositories and
   Lexicons** as one realization of portability.
3. Implement an **IdentityResolver** so a DID can resolve to/from an
   internal Account/Person without that DID being the internal identity
   model itself (ADR-0002 stays intact).
4. Demonstrate export/import and remote identity resolution — this is
   the Phase 7 exit condition.

## Constraints

- Only **T0 (Public)**-classified data is eligible for export to a
  federation adapter by default; anything else requires the same
  per-record opt-in as the permanence adapter
  (`docs/DATA_CLASSIFICATION.md`, ADR-0003).
- Federation does not become authoritative for identity, roles, or
  membership — the internal Account/Membership/RoleAssignment records
  remain the source of truth (ADR-0002, ADR-0006).
- Open question carried over from the original spec: DID method
  (`did:plc` vs. `did:web` vs. custom EVM-bound method) — see
  `docs/open-questions.md` Q1. This must be resolved before Phase 7
  begins, not before Phase 0–6 work.

## Dependencies

- `spec/core/integration-contracts.md` — `PortabilityExporter` /
  `IdentityResolver` adapter interfaces
- `docs/adr/0006-atproto-as-portability-layer.md`
- `docs/open-questions.md` — Q1 (DID method) must be resolved first
