# Authorization

> Status: Draft — required before Phase 1 (accounts, membership, roles)
> is considered complete.

## Model

Authorization is **deny-by-default** and evaluated against the
organization/community scope carried by every record
(`domain-model.md` invariant 1) — never against front-end visibility.

```
Can(Person, Action, Resource) =
    exists Membership(Person, Resource.organizationId)
    AND exists RoleAssignment granting Action for Resource.type
    AND (no active Policy/AccessGrant restriction blocks it)
```

## Core Primitives

- **Membership** — links a Person to an Organization. No action is
  authorized without an active Membership in the resource's scope.
- **RoleAssignment** — grants a Role (Trustee, Beneficiary,
  Administrator, Member, Advisor, or a community-defined role) within an
  Organization or Community. Roles carry a defined set of permitted
  Actions per resource type.
- **Policy** — a Community- or Organization-level rule that can further
  restrict (never silently expand) what a Role permits (e.g., a
  moderation policy limiting who may publish to Public audience).
- **AccessGrant** — a scoped, time-bounded exception granting a specific
  Person access to a specific Resource outside their standing Role (e.g.,
  a successor granted access to succession documents).
- **Delegation** — a Person grants another Person a limited, revocable
  subset of their own authority (e.g., an Advisor acting for a Trustee
  during an absence). Delegations are always narrower than the
  delegator's own authority and are independently auditable.

## Requirements

1. **Authorization test matrix** — every Action/Resource/Role
   combination must have both a permitted and a forbidden test case
   before merge (Definition of Done, `docs/ROADMAP.md`).
2. **Tenant/community isolation** — a query must never be able to return
   or mutate a Resource outside the caller's authorized
   organization/community scope, even via an indirect join. Isolation
   tests are required per `docs/security/THREAT_MODEL.md` (T4).
3. **Administrator actions are not unchecked.** High-impact
   administrative actions (recovery, data export, role changes affecting
   Trustees) require dual approval where the Organization's Policy
   defines it, and are always audited (see `audit-events.md`).
4. **Publication is an authorization event.** Moving content from a
   private/community audience to Public (`docs/DATA_CLASSIFICATION.md`)
   requires an explicit authorized action, not a side effect of another
   operation.
5. **Delegations and AccessGrants expire.** Both must carry an
   expiration or explicit revocation path; neither becomes a silent
   permanent grant.

## Non-Goals (current phase)

- Attribute-based policy engines (listed as a future adapter in the
  Technology Baseline, `docs/ENGINEERING_DEEP_DIVE.md` §9) — the initial
  implementation uses scoped roles and grants only.
- Cross-instance/federated authorization — deferred to Phase 7
  (`spec/federation/README.md`).
