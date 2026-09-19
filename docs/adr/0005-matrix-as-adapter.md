# ADR-0005: Matrix as Adapter

> Status: Accepted
> Date: 2026-09-18

## Context

Matrix remains a reasonable candidate for federated, encrypted instant
messaging, and the existing messaging spec correctly identifies the
unresolved DID-to-Matrix binding and hosting model. However, making
Matrix a core dependency from the outset would require running and
federating a homeserver before the identity, membership, and moderation
model it depends on even exists.

## Decision

Matrix is implemented as an **adapter behind an abstract
`ConversationProvider` interface**, not a dependency of the core
application. Sequence:

1. Build asynchronous comments, mentions, inbox notifications, and
   private administrative notes natively in the modular monolith
   (Phase 4/6).
2. Define the `ConversationProvider` interface
   (`spec/messaging/README.md`).
3. Pilot a single Matrix homeserver (Phase 6) behind that interface.
4. Map Network memberships to Matrix rooms **without making Matrix
   authoritative for roles** — role/membership truth stays in the core
   application.
5. Add federation only after moderation, account suspension, backup, and
   recovery behavior are tested (post-Phase 6).

Custom end-to-end cryptography is not implemented in-house; a mature,
externally-reviewed protocol implementation is used, and private
messaging is not represented as "hardened" without external review.

## Consequences

- The Matrix pilot must be independently disable-able without affecting
  core records or audit history (`SECURITY_REQUIREMENTS.md` Phase 6
  gate).
- Notifications and in-app discussion work standalone even if Matrix is
  never enabled for a given deployment.
- Federation trust/abuse questions are deferred until the adapter
  boundary and moderation/suspension behavior are proven internally.

## Alternatives Considered

- **Matrix as the core messaging system from day one (original design):**
  rejected — couples core record-keeping to a federation protocol before
  identity/moderation are proven.
- **No federated messaging at all:** rejected — federation remains a
  stated goal of the Network; the adapter approach preserves the option
  without the up-front cost.
