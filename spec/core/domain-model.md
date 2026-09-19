# Domain Model

> Status: Draft — Phase 0 foundation. See `docs/adr/0002-identity-and-actor-model.md`
> and `docs/ENGINEERING_DEEP_DIVE.md` §2.

## Aggregates

```
Actor
├── Person
└── Organization
    ├── Family
    ├── Trust
    ├── Business
    ├── Association
    └── Municipality

Community
├── Membership
├── RoleAssignment
├── Policy
├── Space
└── ModerationCase

Content
├── Post
├── Comment
├── Attachment
├── Reaction
└── AudienceRule

Governance
├── Proposal
├── EligibilitySnapshot
├── BallotDefinition
├── Ballot
├── Tally
└── DecisionRecord

Records
├── Document
├── DocumentVersion
├── AccessGrant
├── RetentionRule
└── AuditEvent

Economy
├── LedgerAccount
├── LedgerEntry
├── CreditDefinition
└── TransferPolicy
```

## Invariants

1. **Every record carries an organization or community scope.**
   Authorization is evaluated against that scope — see
   `authorization.md` — never inferred from front-end visibility alone.
2. **Actors are not conflated.** A Person may hold multiple Memberships,
   Roles, and Profiles across Organizations without creating duplicate
   identities (ADR-0002).
3. **Sensitive aggregates are classified** in
   `docs/DATA_CLASSIFICATION.md` before implementation. New aggregates
   must be added to that table as part of their design review.
4. **Sensitive state changes emit AuditEvents** (`audit-events.md`) —
   this is not optional instrumentation, it is part of the aggregate's
   contract.
5. **Module boundaries are explicit.** Cross-aggregate references go
   through application services / domain events, not direct foreign-key
   joins across module lines that would prevent future extraction
   (ADR-0001).

## Per-Module Detail

Each module's full field-level schema lives in its own spec folder:

- [`spec/identity/`](../identity/README.md) — Account, Person, Claim,
  Credential evidence, Delegation
- [`spec/records/`](../records/README.md) — Document, DocumentVersion,
  AccessGrant, RetentionRule
- [`spec/content/`](../content/README.md) — Post, Comment, Attachment,
  Reaction, AudienceRule
- [`spec/governance/`](../governance/README.md) — Proposal,
  EligibilitySnapshot, BallotDefinition, Ballot, Tally, DecisionRecord
- [`spec/economy/`](../economy/README.md) — LedgerAccount, LedgerEntry,
  CreditDefinition, TransferPolicy
- [`spec/messaging/`](../messaging/README.md) — ConversationProvider
  adapter contract
- [`spec/federation/`](../federation/README.md) — export/import package,
  ATProto mapping

Organization, Membership, RoleAssignment, Policy, Space, and
ModerationCase (the Community group) are defined in
`spec/identity/README.md` alongside Account/Person, since membership and
role assignment are the first authorization primitives every other
module depends on.
