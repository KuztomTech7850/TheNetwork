# Project Governance

> Status: Active
> Companion to: [`ROADMAP.md`](ROADMAP.md), [`docs/adr/`](adr/)

This document describes how decisions, issues, and reviews are run for
The Network, distinct from `docs/AGENT_SEED.md` (which governs AI-agent
session behavior). Human contributors and reviewers should follow this
file; `AGENT_SEED.md` should stay consistent with it.

## Decision Records

Architecture-level decisions are recorded as ADRs in [`docs/adr/`](adr/).
An ADR is required before:
- Adding a new runtime dependency that becomes load-bearing (a database,
  queue, protocol, or external service).
- Changing the actor/identity model.
- Changing the storage/permanence boundary (what may leave the private
  database/object-store boundary).
- Introducing or removing an integration adapter (Matrix, ATProto,
  blockchain anchoring, token/ledger).

ADRs are numbered sequentially, never renumbered, and never deleted.
Superseded ADRs are marked `Status: Superseded by ADR-000N` rather than
removed.

## Epics and Issues

Work is organized under the epics listed in [`ROADMAP.md`](ROADMAP.md).
Every issue must carry:

- Epic
- Milestone
- Priority
- Security impact
- Data classification (see [`DATA_CLASSIFICATION.md`](DATA_CLASSIFICATION.md))
- Decision dependency (which ADR or open question it depends on, if any)
- Responsible engineer
- Reviewer
- Acceptance criteria
- Test evidence
- Documentation impact

Templates for issues and pull requests live in
[`.github/ISSUE_TEMPLATE/`](../.github/ISSUE_TEMPLATE/) and
[`.github/pull_request_template.md`](../.github/pull_request_template.md).

## Definition of Done

See [`ROADMAP.md`](ROADMAP.md#definition-of-done). No change merges
without satisfying all listed conditions, including a reviewer other
than the author.

## Phase Gating

Phases in [`ROADMAP.md`](ROADMAP.md) are sequential gates, not
suggestions. A phase-N feature (e.g. Matrix federation, cryptographic
voting, transferable tokens) must not begin implementation until the
exit condition of the prior phase is met and recorded (issue closed,
ADR approved, or milestone marked complete). The operator (repository
owner) approves phase transitions.

## Municipal Readiness

Municipal/civic use (Phase 10) requires a separate, explicit review
gate: election counsel, accessibility specialists, and qualified
security reviewers must sign off before any nonbinding civic pilot, and
statutory elections are out of scope entirely absent that review.
