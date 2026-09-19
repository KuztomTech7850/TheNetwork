# Records Module

> Status: Draft — supports the Family Trust MVP (Phase 3, `docs/ROADMAP.md`).

## Purpose

Manage documents and record-keeping for an Organization (initially a
Family/Trust): the encrypted document vault, versioning, access grants,
succession responsibility directory, and retention.

## Aggregates

- **Document** — a logical document within an Organization's vault
  (e.g., a Trust instrument, a succession plan, an emergency-contact
  sheet). Classified per `docs/DATA_CLASSIFICATION.md` (default T2).
- **DocumentVersion** — an immutable version of a Document's content,
  stored in encrypted S3-compatible object storage with envelope
  encryption (ADR-0003). Creating a new version never overwrites a prior
  one.
- **AccessGrant** — a scoped, time-bounded grant of access to a Document
  or category of Documents to a specific Person, independent of their
  standing Role (`spec/core/authorization.md`).
- **RetentionRule** — defines how long a Document/DocumentVersion is
  kept and what happens at expiration (archive vs. delete), enforced by
  the Records module rather than ad hoc deletion.

## Family Trust MVP Scope

- Encrypted document vault
- Document version history
- Access grants and expiration
- Succession responsibility directory (who assumes which duties)
- Emergency contact and continuity information
- Export package (full vault export for backup/portability)

## Explicit Non-Claim

The application coordinates estate and Trust activity but does not
create legally valid Trust instruments or replace counsel. Legal
documents retain their own signature, notarization, custody, and
amendment requirements outside this system.

## Dependencies

- `spec/core/authorization.md` — AccessGrant evaluation
- `spec/core/audit-events.md` — every access/export is audited
- `docs/DATA_CLASSIFICATION.md` — default tiering and storage rules
- `docs/security/RECOVERY_RUNBOOK.md` — backup/restore and succession
  activation procedures
