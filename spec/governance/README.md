# Governance Module

> Status: Draft — supersedes/extends `spec/voting/README.md` as the
> full governance domain, per `docs/ENGINEERING_DEEP_DIVE.md` §7.

## Purpose

Voting is a policy engine, not merely a ballot table. This module owns
the full proposal-to-decision lifecycle for a Community/Organization.

## Aggregates

- **Proposal** — a sponsored item under discussion. Every Proposal
  carries: sponsor rules, eligibility rule, eligibility snapshot time,
  discussion period, voting period, ballot method, quorum, approval
  threshold, abstention treatment, tie procedure, amendment procedure,
  challenge period, and publication policy.
- **EligibilitySnapshot** — a fixed-in-time record of who is eligible to
  vote on a specific Proposal, taken at a defined point so later
  membership changes don't retroactively change eligibility.
- **BallotDefinition** — the method and options for a specific Proposal's
  vote (see Methods below).
- **Ballot** — an individual cast vote. Starts **visible/signed** by
  default (`docs/ENGINEERING_DEEP_DIVE.md` §7) — secret ballots are added
  only where governance policy requires them (Phase 8+).
- **Tally** — the deterministic, reproducible result computed from
  Ballots under a BallotDefinition.
- **DecisionRecord** — the published outcome; may be classified T0
  (Public) once the community has voted to publish it
  (`docs/DATA_CLASSIFICATION.md`).

## Ballot Methods

| Method | Suitable use |
|---|---|
| Consent or objection | Routine family and operational decisions |
| Simple majority | Binary decisions |
| Supermajority | Structural or high-impact changes |
| Approval voting | Choosing acceptable options |
| Ranked-choice | Selecting among competing alternatives |
| Score voting | Comparing preferences across options |
| Delegated voting | Specialized domains, only with revocable delegation (`spec/core/authorization.md` Delegation) |

Token-weighted voting is **not** a supported default method
(ADR-0007) — purchasing power must not determine standing.

## Phase Gating

- Phase 5: visible, signed ballots; deterministic tally; eligibility
  snapshot; decision register.
- Phase 8: cryptographic/verifiable voting (e.g., ElectionGuard-style
  end-to-end verifiability with voter verification codes and ballot
  challenges) is a `VerifiableTallyProvider` adapter
  (`spec/core/integration-contracts.md`), added only after governance
  semantics are stable and an independent verifier can reproduce a
  published result.
- Municipal/statutory elections remain explicitly out of scope until
  election counsel, accessibility specialists, and qualified security
  reviewers are involved (Phase 10, `docs/PROJECT_GOVERNANCE.md`). A
  town pilot initially supports deliberation, surveys, participatory
  budgeting, and nonbinding votes only.

## Dependencies

- `spec/core/authorization.md` — sponsor rules, eligibility
- `spec/core/audit-events.md` — every proposal/ballot lifecycle
  transition is audited
- `spec/content/README.md` — Proposals and Decisions are also surfaced
  as typed content items

## Legacy

`spec/voting/README.md` is retained for the original voting-mechanism
research notes; this module is the authoritative spec going forward.
