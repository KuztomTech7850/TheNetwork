# Engineering Deep Dive — v0.2

> Status: Draft
> Companion to: [`ARCHITECTURE.md`](../ARCHITECTURE.md), [`ROADMAP.md`](ROADMAP.md)

This document is the technical detail behind the v0.2 engineering
foundation decision: build a secure modular monolith for the family Trust
MVP first, and treat distributed-systems integrations (AT Protocol,
Matrix, blockchain, Arweave) as adapters introduced later.

---

## 1. Core Decisions

### 1.1 Modular monolith first

Start with one deployable application divided into modules with explicit
interfaces:

- Identity and access
- Communities and membership
- Documents and records
- Posts and discussions
- Proposals and voting
- Notifications
- Audit events
- Integration adapters

This gives transactional consistency, straightforward debugging, simpler
backups, and fewer production services. Modules communicate through
application services and domain events so they can be extracted later
without redesigning the data model.

**Do not begin with:** AO compute, an EVM L2, Arweave, independent ATProto
infrastructure, and Matrix federation all operating simultaneously.

### 1.2 Identity before social features

Authentication, identity, roles, and recovery must precede messaging,
voting, posting, or currency. See Section 2 for the actor model.

### 1.3 Private data stays mutable

Trust instruments, succession documents, health information, private
messages, identity evidence, and personal records must never be placed on
a public immutable ledger.

Use:
- PostgreSQL for structured application data
- Encrypted S3-compatible object storage for documents
- Envelope encryption with a managed key or secrets service
- Versioned documents with retention rules
- Encrypted, tested backups
- Append-only audit events for sensitive actions
- Content hashes only when an external timestamp or integrity proof is
  justified

Arweave and blockchain anchoring are optional publication adapters for
deliberately public artifacts — not default storage. See
[`spec/storage/README.md`](../../spec/storage/README.md) and
[`DATA_CLASSIFICATION.md`](DATA_CLASSIFICATION.md) for the classification
gate that must pass before anything reaches a permanence layer.

---

## 2. Fundamental Domain Model

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

Every record carries an organization or community scope. Authorization
must be evaluated against that scope rather than relying on front-end
visibility. See [`spec/core/domain-model.md`](../../spec/core/domain-model.md)
and [`spec/core/authorization.md`](../../spec/core/authorization.md).

### 2.1 Identity concepts

| Concept | Meaning |
|---|---|
| Account | Login credentials and authenticators |
| Person | Human represented by an account |
| Organization | Family, Trust, company, association, or town |
| Membership | Relationship between a person and organization |
| Role | Trustee, beneficiary, administrator, member, advisor |
| Profile | Context-specific public or private presentation |
| Claim | Assertion such as residency or trustee authority |
| Credential evidence | Restricted evidence supporting a claim |
| Delegation | Limited authority one actor grants another |

A person may participate personally, represent a business, serve as a
trustee, and belong to several communities without creating unrelated
identities. NIST's current digital-identity guidance (SP 800-63)
separates identity proofing, authentication, authenticator management,
and federation; The Network preserves those distinctions rather than
treating a DID as the entire identity system.

Use passkeys where practical, retain recovery codes, and support
carefully controlled administrator-assisted recovery. Social recovery can
be investigated later, but must not be the only way to recover authority
over Trust records.

---

## 3. Feature Sequence

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

Full milestone breakdown lives in [`ROADMAP.md`](ROADMAP.md).

---

## 4. Family Trust MVP

The first usable release should include:

- Invitation-only family community
- Passkey or strong multifactor authentication
- Trustee, beneficiary, advisor, member, and administrator roles
- Encrypted document vault
- Document version history
- Access grants and expiration
- Succession responsibility directory
- Emergency contact and continuity information
- Announcements and threaded discussion
- Tasks, acknowledgements, and due dates
- Proposals with visible ballots
- Decision register
- Full administrative audit trail
- Backup, restore, export, and account-recovery procedures

The application coordinates estate and Trust activity but does not claim
to create legally valid Trust instruments or replace counsel. Legal
documents retain their own signature, notarization, custody, and
amendment requirements.

---

## 5. Posting and Social Model

Do not design a generic social network first. Begin with typed community
content:

- Announcement
- Question
- Resource
- Event
- Request for help
- Listing
- Proposal
- Decision
- Family-history entry

Each object uses a shared content envelope containing author, community,
audience, lifecycle state, attachments, moderation state, and timestamps.
Module-specific fields extend that envelope.

Audience controls support:
- Only me
- Named people
- Role
- Group or space
- Entire community
- Federated communities
- Public

Moderation is designed alongside posting: report, hide, lock, archive,
appeal, retention, and moderator-action audit records. The board spec
evolves from a listings-only schema into this shared content model — see
[`spec/content/README.md`](../../spec/content/README.md).

---

## 6. Messaging Strategy

Matrix remains a reasonable candidate for federated encrypted IM, but it
is an adapter, not a dependency of the core application.

Recommended sequence:
1. Build asynchronous comments, mentions, inbox notifications, and
   private administrative notes.
2. Define an abstract `ConversationProvider` interface.
3. Pilot a single Matrix homeserver.
4. Map Network memberships to Matrix rooms without making Matrix
   authoritative for roles.
5. Add federation only after moderation, account suspension, backup, and
   recovery behavior are tested.

Do not implement custom end-to-end cryptography. Use a mature protocol
implementation and obtain external review before representing private
messaging as hardened.

---

## 7. Voting Model

Voting is a policy engine, not merely a ballot table. Every proposal
requires: sponsor rules, eligibility rule, eligibility snapshot time,
discussion period, voting period, ballot method, quorum, approval
threshold, abstention treatment, tie procedure, amendment procedure,
challenge period, and publication policy.

Start with visible, signed community votes — easier to explain and audit.
Add secret ballots only where governance policy requires them.

| Method | Suitable use |
|---|---|
| Consent or objection | Routine family and operational decisions |
| Simple majority | Binary decisions |
| Supermajority | Structural or high-impact changes |
| Approval voting | Choosing acceptable options |
| Ranked-choice | Selecting among competing alternatives |
| Score voting | Comparing preferences across options |
| Delegated voting | Specialized domains, only with revocable delegation |

Do not use token-weighted voting as the default; purchasing power should
not determine family or civic standing.

ElectionGuard demonstrates that end-to-end verifiability requires voter
verification codes, ballot challenges, and publication of encrypted
artifacts sufficient for independent verification. It is a future
reference for high-assurance ballots, not a reason to build cryptography
into the family MVP.

Official municipal elections remain out of scope until election counsel,
officials, accessibility specialists, and qualified security reviewers are
involved. A town pilot should initially support deliberation, surveys,
participatory budgeting, and nonbinding votes rather than replacing
statutory elections.

---

## 8. Community Economics

A transferable cryptocurrency comes last. Begin with an internal
double-entry ledger supporting non-transferable or narrowly transferable
community credits: volunteer hours, contribution acknowledgements,
mutual-aid credits, community grants, participatory-budget allocations.

The ledger never controls identity, voting eligibility, moderation
authority, or access to essential community services.

Before enabling redemption, exchange, custody, or fiat conversion,
require: securities analysis, money-transmission analysis, tax review,
consumer-protection review, AML and sanctions analysis, treasury and
key-management policy, lost-key and disputed-transfer procedures, and
economic abuse and concentration analysis.

Colorado's current guidance says certain stored-value activity may
require money-transmitter licensing, while the treatment of
virtual-currency models depends on the actual flow and custody
arrangement. "Cryptocurrency" cannot be safely implemented from the name
alone; its issuance, custody, redemption, and transfer design must be
reviewed first.

---

## 9. Technology Baseline

| Area | Initial choice | Future adapter |
|---|---|---|
| Web application | SvelteKit and TypeScript | Alternative clients |
| Backend | SvelteKit service layer or separate TypeScript API | Independently deployed services |
| Database | PostgreSQL | Read replicas and specialized indexes |
| Documents | S3-compatible encrypted object storage | User-controlled storage |
| Authentication | WebAuthn/passkeys plus recovery codes | OIDC and federated identity |
| Authorization | Policy service using scoped roles and grants | Attribute-based policy engine |
| Search | PostgreSQL full-text search | OpenSearch if scale requires |
| Background work | Database-backed queue | Dedicated message broker |
| API contracts | OpenAPI and JSON Schema | Federation protocol contracts |
| Deployment | Docker Compose for initial instance | Kubernetes only if operationally justified |
| Observability | Structured logs, metrics, traces, audit events | Multi-instance aggregation |
| Messaging | In-app asynchronous discussion | Matrix adapter |
| Portability | Export/import package | ATProto repositories and Lexicons |
| Integrity | Hash-chained audit events | Public ledger anchoring |
| Voting | Deterministic application tally | Independent cryptographic verifier |
| Economy | Internal double-entry ledger | External wallet or token adapter |

---

## 10. Security Foundation

Threat modeling happens during design, not after implementation. OWASP
defines it as a repeatable process of decomposing the system, identifying
and ranking threats, selecting mitigations, and validating the result.
See [`security/THREAT_MODEL.md`](security/THREAT_MODEL.md) and
[`security/SECURITY_REQUIREMENTS.md`](security/SECURITY_REQUIREMENTS.md).

Minimum security requirements:
- Deny-by-default authorization
- Tenant and community isolation tests
- Passkeys or multifactor authentication for privileged roles
- Encrypted transport and storage
- Key rotation and recovery runbooks
- Structured, append-only security audit events
- Rate limits and abuse controls
- Dependency and secret scanning
- Signed releases and dependency lockfiles
- Software bill of materials
- Backup restoration exercises
- Vulnerability disclosure policy
- Documented incident-response process
- No sensitive data in application logs
- Separate administrative and normal-user actions

Use OWASP ASVS 5.0 as the application-security acceptance checklist. CISA
recommends secure defaults, built-in MFA, logging, and vendor ownership of
customer security outcomes rather than shifting configuration risk to
deployers.

---

*See [`ROADMAP.md`](ROADMAP.md) for milestones, [`docs/adr/`](adr/) for
the seven founding decision records, and [`spec/`](../spec/) for
per-module contracts.*
