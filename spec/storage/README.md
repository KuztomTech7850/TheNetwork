# Spec — Storage Module

> Status: Draft | Version: 0.1 | Date: 2026-06-05

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