# ADR-0007: Defer Transferable Token

> Status: Accepted
> Date: 2026-09-18

## Context

A transferable cryptocurrency or token carries the highest legal, abuse,
and operational uncertainty of any capability in the original
architecture. Colorado's current guidance indicates certain stored-value
activity may require money-transmitter licensing, and the treatment of
virtual-currency models depends on the actual flow and custody
arrangement — "cryptocurrency" cannot be safely implemented from the
name alone.

## Decision

- Begin with an **internal double-entry ledger** supporting
  **non-transferable or narrowly transferable community credits**:
  volunteer hours, contribution acknowledgements, mutual-aid credits,
  community grants, participatory-budget allocations
  (`spec/economy/README.md`).
- The ledger **never controls identity, voting eligibility, moderation
  authority, or access to essential community services**.
- **Token-weighted governance is prohibited as a default** — purchasing
  power must not determine family or civic standing (reinforces
  ADR-0002's separation of identity from wallet ownership).
- Before enabling redemption, exchange, custody, or fiat conversion,
  require: securities analysis, money-transmission analysis, tax review,
  consumer-protection review, AML and sanctions analysis, treasury and
  key-management policy, lost-key and disputed-transfer procedures, and
  economic abuse/concentration analysis. This is a Phase 9 gate in
  `ROADMAP.md`, requiring explicit legal review and economic threat-model
  approval before implementation begins.

## Consequences

- Phase 9 work cannot start on transferable-value features until the
  above reviews are complete and approved by the operator.
- The ledger's initial implementation (Phase 9 internal-credit stage) is
  isolated from identity/voting/access-control modules by construction,
  not just by policy — enforced in `spec/core/authorization.md`.
- An external wallet or token adapter (Technology Baseline, "Economy"
  row) remains a future, explicitly-reviewed integration, not a default
  build target.

## Alternatives Considered

- **Build a transferable token early (original design momentum):**
  rejected — legal and abuse risk is too high before governance and
  identity semantics are stable, and before legal review occurs.
- **Never support any economic ledger:** rejected — non-transferable
  community credit tracking (volunteer hours, mutual aid) is low-risk and
  valuable; the risk is specifically in transferability/redemption, which
  this ADR defers rather than the ledger concept itself.
