# The Network — Architecture Overview

> Last updated: 2026-06-05
> Status: Draft v0.1

---

## Stack Summary

The Network is a six-layer architecture. Each layer has a defined role and
a clear boundary with the layers above and below it.

| Layer | Role | Primary Technology |
|---|---|---|
| **1. Identity** | Portable user identity and key management | AT Protocol (DIDs, PDS) |
| **2. Data** | Signed user records — profiles, votes, posts | AT Protocol (Lexicons, Repos) |
| **3. Compute** | Heavy or parallelizable logic — vote tallying, matching | AO (on Arweave) |
| **4. Permanence** | Immutable storage of vote proofs, repo snapshots, blobs | Arweave / Irys |
| **5. Finality** | Tamper-evident anchoring of critical state | EVM-compatible L2 (TBD) |
| **6. Application** | Community-facing UX — board, voting booth, messenger | SvelteKit + Matrix |

---

## Layer Detail

### Layer 1 — Identity (ATProto)

- Every user has a **DID** (Decentralized Identifier) — a portable,
  self-sovereign identity not tied to any single server.
- Users control their own **Personal Data Server (PDS)** — or delegate
  to a trusted host.
- Identity is the key that unlocks every other module. A profile created
  here works across all Network instances.
- **Open question:** DID method — `did:plc` (Bluesky-compatible) vs.
  `did:web` (self-hosted) vs. a custom method bound to an EVM address.
  See [`docs/open-questions.md`](docs/open-questions.md).

### Layer 2 — Data (ATProto Records)

- User profiles, votes, proposals, board listings, and community posts
  are **signed ATProto records** in personal data repositories.
- **Lexicons** (ATProto schemas) define the shape of each record type.
  See [`spec/voting/`](spec/voting/) and [`spec/board/`](spec/board/).
- **AppViews** index these records to power community dashboards and
  vote tallies without centralizing the underlying data.
- **Relays** are performance optimizations, not trusted authorities.
  The Network is designed to work with alternative relays or direct
  PDS–AppView connections.

### Layer 3 — Compute (AO on Arweave)

- **AO** is the actor-model compute layer for workloads too heavy for
  client-side processing: vote tallying, resource matching, governance
  simulations.
- AO processes receive ATProto-derived data, execute deterministically,
  and commit results back to Arweave.
- **Open question:** Whether AO's TEEs or external ZK/MPC systems handle
  vote privacy. See [`docs/open-questions.md`](docs/open-questions.md).

### Layer 4 — Permanence (Arweave / Irys)

- Critical artifacts — vote proofs, compiled tallies, repo snapshots,
  large blobs — are stored on **Arweave** via **Irys** for batching.
- ATProto records reference Arweave transaction IDs for tamper-evident
  history without overloading PDS storage.
- Uploads under 100 KiB via Irys are free — covers the vast majority of
  Network-scale governance records.

### Layer 5 — Finality (Blockchain)

- A lightweight validation/finality layer anchors critical state:
  final vote tallies, protocol upgrade hashes, DID–address bindings.
- Day-to-day data and computation live in ATProto and AO respectively.
  The blockchain is not a general-purpose app platform here.
- **Open question:** Which chain or L2. Preference is open-source-first
  and EVM-compatible for tooling breadth. See [`docs/open-questions.md`](docs/open-questions.md).

### Layer 6 — Application

- **SvelteKit** front end — community website, voting booth, profile
  creation, board listings.
- **Matrix / Element** for federated, encrypted messaging.
- **RxDB** for local-first profile capsule storage with optional server sync.
- The application layer is the most "swappable" — communities can replace
  the front end entirely while keeping the identity and data layers intact.

---

## Data Flow (Simple Path)
## Data Flow (Simple Path)
User creates profile (RxDB local)
→ Profile capsule encrypted, stored locally
→ Optional: sync to hosted PDS (ATProto)
→ Identity hash anchored on-chain

User submits a vote
→ Signed ATProto record written to user's PDS
→ AppView indexes the record → updates tally dashboard
→ Vote proof uploaded to Arweave via Irys
→ Final tally committed to AO → result hash anchored on-chain

text

---

## Module Flexibility Table

| Module | Replaceable? | Swap Constraint |
|---|---|---|
| ATProto identity | Yes — `did:web` or custom DID | Must produce a portable DID |
| ATProto data layer | Partial — Lexicons are extendable | Changing record schema breaks AppView indexing |
| AO compute | Yes — any verifiable compute layer | Must produce a deterministic, auditable result |
| Arweave permanence | Yes — IPFS + Filecoin is viable | Must provide content-addressed permanent URLs |
| Blockchain finality | Yes — any EVM L2 or Solana | Must support on-chain hash anchoring |
| SvelteKit front end | Yes — any web framework | Must implement the ATProto client SDK |
| Matrix messaging | Yes — any federated IM protocol | Must support federation and E2E encryption |

---

*See individual spec folders for module-level detail.*