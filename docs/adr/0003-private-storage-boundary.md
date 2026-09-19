# ADR-0003: Private Storage Boundary

> Status: Accepted
> Date: 2026-09-18

## Context

The original storage spec treated Arweave/blockchain permanence as a
first-class default mode alongside local and hosted storage, without a
strong data-classification gate. Trust instruments, succession
documents, health information, private messages, identity evidence, and
personal records must never be placed on a public immutable ledger —
once written, such data cannot be retracted.

## Decision

- Structured application data lives in **PostgreSQL**.
- Documents live in **encrypted S3-compatible object storage**, with
  **envelope encryption** using a managed key or secrets service.
- Documents are **versioned** with retention rules; backups are
  **encrypted and tested**.
- Sensitive actions are recorded as **append-only audit events**
  (ADR-0004), not stored on a public ledger.
- **Arweave and blockchain anchoring are optional publication adapters**
  for deliberately public artifacts only, gated by the tiering in
  `docs/DATA_CLASSIFICATION.md`. A record may only reach a permanence
  adapter if it is classified T0 (Public) and the relevant
  author/organization has opted in per-record.
- Content hashes of non-public records may be anchored externally only
  when an external timestamp or integrity proof is specifically
  justified — never as a default.

## Consequences

- `spec/storage/README.md` must be rewritten to make the classification
  gate explicit before "permanent mode" can be implemented.
- Every new record type must be assigned a tier in
  `DATA_CLASSIFICATION.md` before implementation (Definition of Done
  requirement).
- Publishing a record (T1 → T0) is an explicit, audited action, never an
  implicit consequence of creating the record.

## Alternatives Considered

- **Default-permanent storage (original design):** rejected — creates
  irreversible disclosure risk for family/Trust data by default.
- **No permanence layer at all:** rejected — deliberately public
  artifacts (e.g., a community's published decision record) do benefit
  from tamper-evident, durable storage; the fix is gating, not removal.
