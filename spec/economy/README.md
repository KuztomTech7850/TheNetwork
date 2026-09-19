# Economy Module

> Status: Draft — Phase 9 in `docs/ROADMAP.md`. See
> `docs/adr/0007-defer-transferable-token.md` before implementing anything
> beyond the internal, non-transferable ledger described here.

## Purpose

Begin with an internal double-entry ledger supporting non-transferable
or narrowly transferable community credits. A transferable
cryptocurrency comes last, and only after legal and economic threat-model
review (ADR-0007).

## Aggregates

- **LedgerAccount** — an account within an Organization/Community's
  ledger (e.g., a Person's volunteer-hours balance, a mutual-aid pool).
- **LedgerEntry** — a double-entry debit/credit record. Immutable once
  posted; corrections are new offsetting entries, never edits.
- **CreditDefinition** — defines a credit type and its rules (volunteer
  hours, contribution acknowledgements, mutual-aid credits, community
  grants, participatory-budget allocations).
- **TransferPolicy** — defines whether and how a credit type may move
  between LedgerAccounts (default: non-transferable; narrow transfer
  requires an explicit policy).

## Hard Constraints

1. The ledger **never controls identity, voting eligibility, moderation
   authority, or access to essential community services**
   (`spec/core/authorization.md` stays independent of LedgerAccount
   balances).
2. **No transferable-value redemption, exchange, custody, or fiat
   conversion** ships until all of the following are complete and
   approved by the operator: securities analysis, money-transmission
   analysis, tax review, consumer-protection review, AML/sanctions
   analysis, treasury and key-management policy, lost-key and
   disputed-transfer procedures, and economic abuse/concentration
   analysis (ADR-0007, `docs/RISK_REGISTER.md`).
3. **Token-weighted governance is prohibited** as a default
   (`spec/governance/README.md`).

## Dependencies

- `spec/core/audit-events.md` — every LedgerEntry is audited
- `docs/DATA_CLASSIFICATION.md` — LedgerAccount/LedgerEntry classified T2
- `docs/adr/0007-defer-transferable-token.md` — the phase gate for
  anything beyond the internal, non-transferable ledger
