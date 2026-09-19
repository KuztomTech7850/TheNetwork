# The Network

> A flexible, decentralized platform for empowering communities to safely
> organize, share, and decide together.

---

## What This Is

The Network is open-source civic infrastructure. It provides a core set of
modules — identity, voting, messaging, community boards, and permanent
record-keeping — that any community can adopt, adapt, and extend without
forking the underlying philosophy.

A town. A family support group. A neighborhood bulletin board. A vulnerable
citizens' hub. Each one runs its own "flavor" of The Network, built on the
same stack, sharing the same trust model.

---

## The One-Line Promise

*The community, not the software, decides what features, rules, and structures
it needs.*

---

## Core Modules

> v0.2 reframes these as modules of a single **modular monolith**, with
> AT Protocol, Matrix, and blockchain anchoring introduced later as
> adapters. See [`docs/ENGINEERING_DEEP_DIVE.md`](docs/ENGINEERING_DEEP_DIVE.md).

| Module | Purpose | Status |
|---|---|---|
| **Identity** | Actor model (Account, Person, Organization, Membership, Role) — portable DID added later as a federation adapter | 🔵 Spec in progress |
| **Governance** | Egalitarian governance — proposals, ballots, tamper-evident results | 🔵 Spec in progress |
| **Content** | Typed community content — announcements, listings, events, discussions | 🔵 Spec in progress |
| **Records** | Encrypted document vault, versioning, succession — the Family Trust MVP core | 🔵 Spec in progress |
| **Messaging** | In-app async discussion first; Matrix as a later adapter | 🔵 Spec in progress |
| **Storage** | PostgreSQL + encrypted object storage; Arweave permanence is an opt-in, T0-only adapter | 🔵 Spec in progress |
| **Economy** | Internal non-transferable ledger; transferable tokens deferred pending legal review | 🔵 Spec in progress |

---

## Design Principles

1. **Modular by default** — communities plug in what they need and leave out
   what they don't.
2. **Privacy by architecture** — personal data never leaves the user's control
   without explicit, granular consent.
3. **Open source, always** — every component is MIT or Apache 2.0 licensed.
   No commercial lock-in.
4. **Portable identity** — a profile created in one Network instance works in
   any other. No re-registration.
5. **Tamper-evident records** — votes, proposals, and critical community
   records are anchored immutably.
6. **Server-optional** — users can keep data fully local or sync to a hosted
   server. Both are first-class options.

---

## What's Being Worked On

**Active phase: Engineering Foundation v0.2 — modular monolith, identity-first
sequencing, Family Trust MVP scope.**

- `README.md` / `ARCHITECTURE.md` — updated for v0.2 ✅
- `docs/ROADMAP.md`, `docs/ENGINEERING_DEEP_DIVE.md` — drafted ✅
- `docs/adr/0001`–`0007` — seven founding decision records drafted ✅
- `docs/DATA_CLASSIFICATION.md`, `docs/RISK_REGISTER.md`, `docs/security/` — drafted ✅
- `spec/core/`, `spec/records/`, `spec/content/`, `spec/governance/`,
  `spec/economy/`, `spec/federation/` — new module contracts drafted ✅
- Legacy `spec/identity`, `spec/messaging`, `spec/board`, `spec/storage`,
  `spec/voting` — annotated with v0.2 notes, retained for research history

Next milestone: **Milestone 0 (Foundation)** — operator sign-off on the
modular-monolith decision, then begin Milestone 1 (Secure family shell).
See [`docs/ROADMAP.md`](docs/ROADMAP.md).

---

## Known Integrations

| Project | Role | Repo |
|---|---|---|
| **Chuck's List** | Community bulletin board use case | [ChucksList_Builder](https://github.com/KuztomTech7850/ChucksList_Builder) |
| **HolistiveHive** | Personal dashboard / Network client app | [HolistiveHive_App](https://github.com/KuztomTech7850/HolistiveHive_App) |
| **TechSpecific Website** | Business site / public hub linking Network properties | TechSpecific_Website |

---

## Repository Layout
TheNetwork/
├── docs/ Vision, use cases, open questions, research threads, ADRs, roadmap
├── spec/ Module-level technical specifications (core, identity, records, content, governance, economy, messaging, storage, federation)
├── integrations/ Integration notes for connected projects
├── .github/ Issue/PR templates, CODEOWNERS, workflows
└── ARCHITECTURE.md Full stack overview

text

---

## Who Should Read What

| You are... | Start here |
|---|---|
| **Understanding the project** | This file — then [`ARCHITECTURE.md`](ARCHITECTURE.md) |
| **Planning or prioritizing work** | [`docs/ROADMAP.md`](docs/ROADMAP.md) and [`docs/PROJECT_GOVERNANCE.md`](docs/PROJECT_GOVERNANCE.md) |
| **Reviewing open decisions** | [`docs/open-questions.md`](docs/open-questions.md) and [`docs/adr/`](docs/adr/) |
| **An AI engineering agent** | [`docs/AGENT_SEED.md`](docs/AGENT_SEED.md) — read this before anything else |
| **A contributing developer** | [`CONTRIBUTING.md`](CONTRIBUTING.md) |

---

## Status

This repository is in **architectural definition** phase, now sequenced
under **Engineering Foundation v0.2**: a modular monolith for a Family
Trust MVP first, with AT Protocol, Matrix, and blockchain/Arweave
introduced later as adapters. No production code exists yet. All files
are living documents — dated, versioned, and open for revision as
decisions are made.

See [`ARCHITECTURE.md`](ARCHITECTURE.md) for the current stack overview,
[`docs/ROADMAP.md`](docs/ROADMAP.md) for the phased feature sequence, and
[`docs/open-questions.md`](docs/open-questions.md) for the open decisions
that must be resolved before any module moves to implementation.

---

*HarterHill Network — Montezuma County, Colorado.*
*Built by TechSpecific / KuztomTech7850.*