# ADR-0002: Identity and Actor Model

> Status: Accepted
> Date: 2026-09-18

## Context

The original design treated an ATProto DID as effectively the entire
identity system. NIST SP 800-63 separates identity proofing,
authentication, authenticator management, and federation — collapsing
these into "a DID" would conflate distinct concerns and make it hard to
represent a person who participates personally, represents a business,
serves as a Trustee, and belongs to several communities without creating
unrelated identities.

## Decision

Adopt an explicit actor model with these concepts, kept distinct in the
domain model (`spec/core/domain-model.md`):

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

Authentication uses passkeys/WebAuthn where practical, with recovery
codes and carefully controlled administrator-assisted recovery. A
portable DID (ATProto or otherwise) may later be layered on as one
authenticator/federation option under this model — it is not the model
itself. Social recovery may be investigated later but must not become
the only way to recover authority over Trust records.

## Consequences

- Every other module (records, content, governance, economy) authorizes
  against Membership + RoleAssignment, not against a raw account or DID.
- Wallet/token ownership is explicitly kept separate from identity and
  voting eligibility (see ADR-0007) — owning a key does not equal being a
  Person or holding a Role.
- Federation (Phase 7) becomes an additive capability: a Person's
  internal Account can later resolve to/from a portable DID without
  restructuring Membership or Role data.

## Alternatives Considered

- **DID-as-identity (original design):** rejected — too early to bind
  the entire identity system to one external protocol's assumptions.
- **Single flat "User" table:** rejected — cannot represent a person
  acting in multiple organizational/role contexts cleanly, and conflates
  authentication with organizational membership.
