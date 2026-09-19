# Engineering Foundation — v0.2

> Tags: architecture, governance, research, coordination
> Status: Open
> Date: 2026-09-18

## Objective

Turn The Network's broad vision into a buildable family-hub MVP while
preserving clean paths toward federation, civic governance, verifiable
voting, and community economics.

The recommendation: build a secure **modular monolith** for the family
Trust first. Treat AT Protocol, Matrix, blockchain anchoring, permanent
storage, and cryptocurrency as **replaceable integrations**, introduced
only after the core domain works.

## Background

The repository already defines identity, voting, messaging, board,
storage, permanence, compute, and blockchain layers, but remains in the
architectural-definition phase with no production code. The original
design required integrating several distributed systems before validating
family-hub workflows, substantially increasing operational and security
risk.

The first deployment should prove that a family can securely maintain
membership, documents, succession responsibilities, proposals, decisions,
and shared communications. Municipal adoption remains a future profile
built on the same domain contracts — not an initial compliance target.

## What Changed From v0.1

- Runtime architecture simplified: one modular monolith instead of six
  simultaneously-integrated distributed layers (see ARCHITECTURE.md).
- Identity model expanded from "DID = identity" to a full actor model:
  Account, Person, Organization, Membership, Role, Profile, Claim,
  Credential evidence, Delegation.
- Private/sensitive data explicitly barred from public immutable ledgers.
  Arweave/blockchain become optional publication adapters for deliberately
  public artifacts only.
- Full phased feature sequence (Phase 0–10) added, gating federation,
  cryptographic voting, and community economics behind a working core.
- Family Trust MVP defined as the first usable release target.
- Full documentation, ADR, and spec restructuring proposed (see
  `docs/ROADMAP.md`, `docs/adr/`, and the new `spec/` layout).

## Disposition

This thread is the working record of the v0.2 planning pass. See:
- [`docs/ENGINEERING_DEEP_DIVE.md`](../ENGINEERING_DEEP_DIVE.md) — full domain model, phase sequence, technology baseline
- [`docs/ROADMAP.md`](../ROADMAP.md) — milestones and feature sequence
- [`docs/DATA_CLASSIFICATION.md`](../DATA_CLASSIFICATION.md) — data sensitivity tiers and storage rules
- [`docs/RISK_REGISTER.md`](../RISK_REGISTER.md) — tracked risks and responses
- [`docs/security/THREAT_MODEL.md`](../security/THREAT_MODEL.md) — initial threat model
- [`docs/adr/`](../adr/) — seven architecture decision records
