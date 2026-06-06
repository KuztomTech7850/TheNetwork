# Spec — Board Module

> Status: Draft | Version: 0.1 | Date: 2026-06-05

## Objective
A community bulletin board for listings, events, and announcements —
with built-in trust, reputation, and tamper-evident records.

## Primary Technology
ATProto AppView (indexes signed records from community PDSes)

## Scope
- ATProto Lexicon schemas for listings, events, announcements
- AppView indexing and filtering
- Relay topology (plural relays, no single chokepoint)
- Arweave archival of compiled board editions
- Moderation policy intersection with governance records (Q6)

## Known Integration
Chuck's List Builder — the Phase 3 GUI for ChucksList_Builder maps
directly to this module. See [`integrations/chuckslist/`](../../integrations/chuckslist/).

## Open Items
- [ ] Lexicon schema draft
- [ ] Moderation policy model (Q6)
- [ ] Relay topology design