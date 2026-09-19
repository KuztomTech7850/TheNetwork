# Spec — Board Module

> Status: Draft | Version: 0.1 | Date: 2026-06-05
>
> **v0.2 note:** The Board module evolves into the shared typed-content
> model in [`spec/content/README.md`](../content/README.md), where a
> Board listing is one `ContentItem` type (`Listing`) among several.
> ATProto Lexicons/AppView indexing described below now apply at the
> Federation phase (Phase 7), not to the initial PostgreSQL-backed
> implementation. See
> [`docs/adr/0006-atproto-as-portability-layer.md`](../../docs/adr/0006-atproto-as-portability-layer.md).

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