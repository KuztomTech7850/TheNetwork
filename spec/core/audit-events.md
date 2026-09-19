# Audit Events

> Status: Draft. See `docs/adr/0004-append-only-audit-events.md`.

## Contract

Every module that performs a sensitive action must emit an `AuditEvent`.
"Sensitive" includes, at minimum: authentication changes, role/
membership changes, document access/export, recovery/administrative
actions, proposal/ballot lifecycle transitions, moderation actions, and
any publication of content from a private/community audience to Public.

```
AuditEvent {
  id
  organizationId / communityId     // scope, per domain-model invariant 1
  actorId                          // who performed the action
  onBehalfOf?                      // set if acting via Delegation
  action                           // e.g. "document.access", "role.assign"
  resourceType
  resourceId
  timestamp
  priorEventHash                   // hash-chaining, see below
  metadata                          // non-sensitive context only — no T2/T3 payloads
}
```

## Rules

1. **Append-only.** No update or delete operation is permitted against
   the AuditEvent store at the application layer; enforce at the
   database layer where possible (e.g., revoked UPDATE/DELETE
   privileges on the audit table/role).
2. **Hash-chained.** Each event's `priorEventHash` links to the previous
   event in its scope, so any deletion or tampering breaks the chain and
   is detectable without requiring a public ledger.
3. **Classified T2.** AuditEvents are restricted to administrators and
   security reviewers by default (`docs/DATA_CLASSIFICATION.md`) — they
   are not public, and `metadata` must never contain T2/T3 payloads
   (e.g., document contents, message bodies). Reference resource IDs,
   not content.
4. **Viewer required.** An audit-event viewer (filterable by actor,
   resource, action, time range) is an explicit Milestone 1 exit
   condition in `docs/ROADMAP.md`.
5. **Integrity check.** The hash chain must be verifiable on demand
   (and as part of backup-restore drills, `docs/security/RECOVERY_RUNBOOK.md`).
6. **External anchoring is optional and later.** Anchoring the audit
   chain hash to a public ledger is a Phase 8+ adapter, not a Phase 1–3
   requirement (ADR-0003, ADR-0004).
