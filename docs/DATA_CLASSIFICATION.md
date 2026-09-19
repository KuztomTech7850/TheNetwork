# Data Classification

> Status: Draft — required before any Phase 2+ storage work begins.
> Companion to: [`ENGINEERING_DEEP_DIVE.md`](ENGINEERING_DEEP_DIVE.md),
> [`spec/storage/README.md`](../spec/storage/README.md)

Every record in The Network must be assigned one of the tiers below
before it is persisted anywhere. The tier determines which storage
targets are permitted. This classification is the gate that the storage
spec's "permanent mode" currently lacks.

## Tiers

| Tier | Examples | Permitted storage | Forbidden storage |
|---|---|---|---|
| **T0 — Public** | Public announcements, published decision records the community has voted to publish, public board listings | PostgreSQL, encrypted object storage, Arweave/permanence adapter (opt-in, per-record) | — |
| **T1 — Community-internal** | Non-public posts, comments, membership rosters visible to a community, moderation cases | PostgreSQL, encrypted object storage | Public ledger, Arweave, any adapter without per-record consent |
| **T2 — Sensitive personal** | Trust instruments, succession documents, health information, private messages, identity evidence, financial ledger entries | PostgreSQL (encrypted at rest), encrypted S3-compatible object storage with envelope encryption | Public ledger, Arweave, any external adapter, application logs |
| **T3 — Credential/secret** | Passwords/authenticator secrets, recovery codes, private keys, session tokens | Managed secrets service / KMS-backed encryption only | Application database in plaintext, logs, backups without equivalent encryption |

## Rules

1. **Default tier is T2** for any new record type until explicitly
   reclassified in a data-classification review. Nothing is assumed
   public.
2. **Publication is an explicit action**, not a storage default. Moving a
   T1 record to T0 (e.g., publishing a decision record) requires a
   recorded authorization event (see `spec/core/audit-events.md`).
3. **Arweave/blockchain anchoring only applies to T0 data**, and only
   after the record's author/organization has opted in per-record. See
   ADR-0003 (private storage boundary) and ADR-0007 (defer transferable
   token).
4. **Content hashes** of T1/T2 records may be anchored externally only
   when an external timestamp or integrity proof is justified by a
   specific governance or legal need — never as a default behavior.
5. **No sensitive data (T2/T3) in application logs.** Structured logs may
   reference record IDs, not content.
6. **Backups inherit the classification of their contents** and must be
   encrypted and access-controlled to the same standard as the source.
7. **Retention rules** are defined per record type in
   `spec/records/README.md` and must be enforced by the Records module,
   not left to ad hoc deletion.

## Review Process

Every new aggregate/entity introduced in `spec/core/domain-model.md` must
list its tier in this document before implementation begins (Definition
of Done requirement — see `ROADMAP.md`).

| Aggregate | Tier | Notes |
|---|---|---|
| Account | T3 (credentials) / T2 (profile) | Split by field — see `spec/identity/README.md` |
| Person | T2 | |
| Organization | T1 | Name/description may be T0 if the org opts into public listing |
| Membership | T1 | |
| RoleAssignment | T1 | |
| Post / Comment | T0–T1 per `AudienceRule` | Author sets audience at creation |
| Document / DocumentVersion | T2 | Trust/estate documents default T2 |
| Proposal / Ballot / Tally | T1, published `DecisionRecord` may become T0 | |
| AuditEvent | T2 | Restricted to admins/security reviewers |
| LedgerAccount / LedgerEntry | T2 | Financial data — see `spec/economy/README.md` |
