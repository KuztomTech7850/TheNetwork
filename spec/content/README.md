# Content Module

> Status: Draft — supersedes the listings-only Board spec (`spec/board/README.md`)
> as the shared content model, per `docs/ENGINEERING_DEEP_DIVE.md` §5.

## Purpose

Do not design a generic social network first. Begin with **typed
community content**, sharing one envelope, so audience rules and
moderation are consistent across content types.

## Typed Content

- Announcement
- Question
- Resource
- Event
- Request for help
- Listing (the original Board use case — see `spec/board/README.md` for
  Chuck's List-specific migration notes)
- Proposal (lifecycle owned by `spec/governance/README.md`; represented
  here only as a content-envelope entry)
- Decision (published `DecisionRecord` from governance, surfaced as
  content)
- Family-history entry

## Shared Content Envelope

```
ContentItem {
  id
  type              // one of the typed content kinds above
  authorId
  organizationId / communityId
  audience          // AudienceRule
  lifecycleState    // draft, published, archived, retracted
  attachments[]
  moderationState
  createdAt / updatedAt
}
```

Module-specific fields extend this envelope rather than replacing it.

## Aggregates

- **Post / Comment** — the core content and reply objects.
- **Attachment** — files/media associated with a Post or Comment;
  classified per `docs/DATA_CLASSIFICATION.md` like any Document.
- **Reaction** — lightweight acknowledgement (not a full Comment).
- **AudienceRule** — determines visibility: Only me, Named people, Role,
  Group/Space, Entire community, Federated communities, Public.
  Moving to Public is an explicit, audited authorization event
  (`spec/core/authorization.md` requirement 4).

## Moderation

Designed alongside posting, not bolted on later:
- Report, hide, lock, archive, appeal
- Retention (see `RetentionRule` in `spec/records/README.md`)
- Moderator-action audit records (`spec/core/audit-events.md`)

## Migration Note (Chuck's List / Board)

`spec/board/README.md` remains the record of the original listings-only
schema and the ChucksList integration plan
(`integrations/chuckslist/README.md`). When the Content module is
implemented, Board listings become one typed content kind (`Listing`)
within this shared envelope rather than a separate schema.
