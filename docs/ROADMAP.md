# Roadmap

> Status: Active
> Companion to: [`ENGINEERING_DEEP_DIVE.md`](ENGINEERING_DEEP_DIVE.md)

This roadmap sequences work so that identity, authorization, and audit
trust are established before social, governance, messaging, federation,
or economic features are added. Do not start a later phase before the
prior phase's exit condition is met.

---

## Feature Sequence

| Phase | Build | Why now | Exit condition |
|---|---|---|---|
| 0 | Scope, data classification, threat model, ADRs | Prevent irreversible architectural mistakes | Risks, actors, trust boundaries, and non-goals approved |
| 1 | Accounts, people, organizations, memberships, roles | All other modules depend on trusted membership | Invite, login, recovery, suspension, and role tests pass |
| 2 | Audit log, encrypted storage, backups | Trust records require accountability and recoverability | Restore drill succeeds; sensitive actions are auditable |
| 3 | Family Trust MVP | Validates the actual initial use case | Family can manage records, duties, announcements, and decisions |
| 4 | Posts, comments, audiences, moderation | Establishes social behavior without real-time complexity | Private/group/public audiences and moderation work |
| 5 | Proposals and transparent voting | Depends on identity, roles, and audit history | Eligibility snapshot and reproducible tally work |
| 6 | Notifications and IM integration | Useful after stable membership and permissions exist | Matrix pilot can be disabled without affecting core records |
| 7 | ATProto portability and federation | Requires stable internal schemas first | Export/import and remote identity resolution are demonstrated |
| 8 | Cryptographic voting and external anchoring | Adds assurance after governance semantics stabilize | Independent verifier reproduces a published result |
| 9 | Community credits or cryptocurrency | Highest legal, abuse, and operational uncertainty | Legal review and economic threat model approved |
| 10 | Municipal profile | Requires hardening, accessibility, records policy, and legal review | Controlled nonbinding civic pilot completes |

Only after Milestones 0–3 (below) should engineering begin Matrix,
ATProto federation, public anchoring, secret ballots, or transferable
value.

---

## First Milestones

### Milestone 0 — Foundation
- Approve the modular-monolith decision.
- Define the actor, organization, membership, and role models.
- Complete data classification.
- Complete the initial threat model.
- Add ADR and issue templates.
- Establish CI, testing, migrations, and development containers.

### Milestone 1 — Secure family shell
- Invitation and onboarding
- Account recovery
- Family and Trust organizations
- Membership and roles
- Authorization test matrix
- Audit-event viewer
- Backup and restore

### Milestone 2 — Trust workspace
- Document vault
- Document versions
- Access grants
- Succession responsibilities
- Tasks and acknowledgements
- Announcements
- Export package

### Milestone 3 — Decisions
- Proposals
- Discussion period
- Eligibility snapshots
- Visible ballots
- Deterministic tallies
- Decision register
- Challenge and correction workflow

---

## Epics

1. Engineering governance and decision records
2. Identity, membership, and authorization
3. Audit, storage, and recovery
4. Family Trust records
5. Community content and moderation
6. Proposals and voting
7. Messaging integration
8. Federation and portability
9. Verifiable records and anchoring
10. Community economics
11. Municipal-readiness program

---

## Required Issue Fields

- Epic
- Milestone
- Priority
- Security impact
- Data classification
- Decision dependency
- Responsible engineer
- Reviewer
- Acceptance criteria
- Test evidence
- Documentation impact

## Definition of Done

A change is complete only when:
- Acceptance criteria pass.
- Authorization tests cover permitted and forbidden cases.
- Data classification is documented.
- Database migrations have rollback or recovery instructions.
- Audit-event requirements are satisfied.
- Unit and integration tests pass.
- User-facing behavior has an end-to-end test.
- Threat-model changes are recorded.
- API and operational documentation are updated.
- Accessibility is checked.
- A reviewer other than the author approves it.

See [`PROJECT_GOVERNANCE.md`](PROJECT_GOVERNANCE.md) for how epics,
issues, and reviews are run.
