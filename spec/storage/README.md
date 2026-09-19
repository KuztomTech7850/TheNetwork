# Spec — Storage Module

> Status: Draft | Version: 0.1 | Date: 2026-06-05
>
> **v0.2 note:** The permanent (Arweave) mode requires the data-
> classification gate defined in
> [`docs/DATA_CLASSIFICATION.md`](../../docs/DATA_CLASSIFICATION.md) —
> only T0 (Public), per-record opt-in data may reach Arweave. Local and
> server-assisted modes below map to PostgreSQL + encrypted
> S3-compatible object storage in the initial modular monolith. See
> [`docs/adr/0003-private-storage-boundary.md`](../../docs/adr/0003-private-storage-boundary.md).

## Objective
Define how profile capsules, community records, and large blobs are
stored — locally, server-assisted, and permanently on Arweave.

## Storage Modes

| Mode | Where data lives | Privacy | Risk |
|---|---|---|---|
| **Local-only** | Device only | Maximum | Data loss if device lost |
| **Server-assisted** | Encrypted blob on hosted PDS | High (server cannot read) | Depends on host uptime |
| **Arweave permanent** | Immutable on Arweave | Public (hashed references only) | None — permanent |

## Primary Technologies
- **RxDB** — local-first database with field-level encryption
- **ATProto PDS** — optional hosted encrypted sync
- **Arweave / Irys** — permanent artifact storage

## Open Items
- [ ] Server-side storage scope decision (Q7)
- [ ] Arweave cost model at scale (Q8)
- [ ] RxDB schema draft
- [ ] PDS adapter / plugin design