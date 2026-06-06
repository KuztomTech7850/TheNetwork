# Open Questions — The Network

> These must be resolved before any module moves to implementation.
> Each item is tagged with which modules it blocks.

---

## Priority 1 — Blocks Everything

**Q1 — DID Method**
Which DID method roots user identity?
- `did:plc` (Bluesky-compatible, but depends on plc.directory for key rotation)
- `did:web` (self-hosted, no external dependency, but requires a domain)
- Custom method binding ATProto DID to an EVM address
- **Blocks:** Identity, Voting, Storage, all downstream modules

**Q2 — Blockchain / L2 Selection**
Which chain anchors identity bindings and vote hashes?
Constraint: open-source-first, EVM-compatible preferred for tooling.
First deployment should use whatever most closely enables the full vision.
- **Blocks:** Finality layer, DID–address binding, governance contracts

---

## Priority 2 — Blocks Voting

**Q3 — Vote Privacy Model**
How are individual votes kept private while tallies remain publicly verifiable?
- AO TEEs (trusted execution environments built into AO)
- External ZK (zero-knowledge proofs — e.g., Groth16, PLONK)
- Commit-reveal scheme (simpler, lower trust guarantees)
- **Blocks:** Voting spec, AO compute design

**Q4 — Sybil Resistance / Proof of Personhood**
How does The Network enforce one-person-one-vote?
- BrightID (open-source, social graph-based)
- Proof-of-humanity (biometric, higher friction)
- Community vouching (simpler, less Sybil-resistant)
- **Blocks:** Voting spec, governance contracts

---

## Priority 3 — Blocks Governance

**Q5 — Governance Model**
One-person-one-vote vs. stake-weighted vs. delegated?
First iteration should support at minimum egalitarian one-person-one-vote.
- **Blocks:** Smart contract spec, voting UX

**Q6 — Moderation Policy Intersection**
How do AppView moderation policies interact with "official" governance records?
Who decides what a community's AppView surfaces vs. suppresses?
- **Blocks:** AppView design, board spec

---

## Priority 4 — Blocks Storage Design

**Q7 — Server-Side Storage Scope**
For the first iteration, server-assisted storage (encrypted blob on a hosted PDS)
is offered alongside local-only mode. What exactly is stored server-side?
Full profile capsule? Identity hash only? Selective fields?
- **Blocks:** Storage spec, RxDB schema, PDS adapter design

**Q8 — Arweave Cost Model at Scale**
Arweave's pay-once model is cost-effective at small scale (Irys free tier for
sub-100KB uploads). At Network scale (many communities, many votes), what is
the projected write volume and cost? When does batching become necessary?
- **Blocks:** Storage spec, economic model thread