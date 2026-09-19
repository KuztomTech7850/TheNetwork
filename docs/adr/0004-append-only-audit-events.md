# ADR-0004: Append-Only Audit Events

> Status: Accepted
> Date: 2026-09-18

## Context

Trust records require accountability and recoverability. Without a
tamper-evident record of sensitive actions (document access, role
changes, recovery/administrative actions, vote tallying), administrator
overreach or a compromised account cannot be detected or investigated,
and disputes cannot be resolved.

## Decision

- Sensitive actions across all modules emit an **AuditEvent** (see
  `spec/core/audit-events.md`) to an **append-only** store.
- Audit events are **hash-chained** (each event references the hash of
  the prior event in its scope) so tampering or deletion is detectable
  without requiring a public blockchain.
- AuditEvent records are classified T2 (`DATA_CLASSIFICATION.md`) —
  restricted to administrators/security reviewers, not public by
  default.
- External anchoring of the audit-chain hash (to a public ledger) is an
  optional, later adapter (Phase 8+), not a Phase 1–3 requirement.

## Consequences

- Every module that performs a sensitive action (role change, document
  access/export, recovery action, vote tally, moderation action) depends
  on the Audit module's event-emission contract from Phase 1 onward.
- The audit-event viewer and hash-chain integrity check are explicit
  exit conditions for Milestone 1.
- Threat-model coverage for "tampering with audit log to hide an action"
  (see `security/THREAT_MODEL.md`) is satisfied by hash-chaining plus
  append-only storage constraints at the database layer.

## Alternatives Considered

- **Mutable/updatable action log:** rejected — does not provide
  tamper-evidence and undermines the accountability guarantee the family
  Trust use case requires.
- **Immediate public-ledger anchoring of every audit event:** rejected —
  conflicts with ADR-0003 (private storage boundary); deferred to an
  optional Phase 8 adapter.
