# Integration — Chuck's List Builder

> Repo: [ChucksList_Builder](https://github.com/KuztomTech7850/ChucksList_Builder)

Chuck's List is a long-running community email list for Montezuma County,
Colorado. The ChucksList_Builder pipeline currently produces HTML email
editions from CSV exports.

## Integration Path

The Board module is the primary integration point.

| ChucksList Phase | Network Integration |
|---|---|
| Phase 2 (cPanel migration) | No changes — pipeline runs independently |
| Phase 3 (Web GUI) | Submission form writes signed ATProto records → Board AppView renders public listings alongside email delivery |
| Phase 4+ | Arweave archival of compiled editions; optional trust/reputation layer for submitters |

## What Does Not Change
- Chuck McAfee's editorial control
- Zoho Campaigns email delivery (runs alongside the board)
- The two-pipeline architecture (Bulletin + Events)

## Status
> Not started. Integration begins when the Board module spec is complete
> and the Phase 3 GUI design begins.