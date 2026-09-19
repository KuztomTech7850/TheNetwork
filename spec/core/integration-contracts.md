# Integration Contracts

> Status: Draft. See `docs/adr/0001-modular-monolith-first.md`,
> `docs/adr/0005-matrix-as-adapter.md`, `docs/adr/0006-atproto-as-portability-layer.md`.

## Principle

Distributed-systems integrations are **adapters behind explicit
interfaces**, never direct dependencies reached into from core-module
business logic. This is what allows the modular monolith (ADR-0001) to
extract modules or swap adapters later without a data-model rewrite.

## Required Adapter Interfaces

| Adapter | Interface | Phase gate | Notes |
|---|---|---|---|
| Messaging (Matrix) | `ConversationProvider` | Phase 6 | Matrix is not authoritative for roles/membership (ADR-0005) |
| Portability (ATProto) | `PortabilityExporter` / `IdentityResolver` | Phase 7 | Export/import package defined first; ATProto is one realization (ADR-0006) |
| Permanence (Arweave/Irys or equivalent) | `PublicationAdapter` | Phase 8 | Only accepts T0-classified records with explicit per-record opt-in (ADR-0003) |
| Finality (blockchain anchoring) | `AnchoringAdapter` | Phase 8 | Anchors hashes only, never raw sensitive content |
| Verifiable voting (e.g. ElectionGuard-style) | `VerifiableTallyProvider` | Phase 8 | Replaces/augments the deterministic application tally only after independent verification is demonstrated |
| Economy (external wallet/token) | `LedgerSettlementAdapter` | Phase 9 | Only after legal review per ADR-0007 |

## Contract Rules

1. **Off by default.** No adapter is enabled in a deployment until its
   phase gate's exit condition (`docs/ROADMAP.md`) is met and the
   operator explicitly enables it.
2. **Disable-able independently.** Disabling any adapter must not break
   core record integrity, audit history, or authorization — verified as
   part of that adapter's phase-gate testing (see
   `docs/security/SECURITY_REQUIREMENTS.md`).
3. **No adapter becomes a source of truth** for identity, roles,
   membership, or authorization. Core Postgres-backed aggregates remain
   authoritative (ADR-0002, ADR-0006).
4. **Data classification enforced at the boundary.** Every adapter
   implementation must check `docs/DATA_CLASSIFICATION.md` tier before
   accepting a record — this is enforced in the adapter interface, not
   left to caller discipline.
5. **API contracts documented.** Internal module interfaces and any
   external adapter contract are described using OpenAPI/JSON Schema
   (Technology Baseline, `docs/ENGINEERING_DEEP_DIVE.md` §9) so they can
   be versioned and tested independently of the module's internals.
