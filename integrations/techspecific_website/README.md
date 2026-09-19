# Integration — TechSpecific Website

> Role: Business / marketing site for TechSpecific — also serves as the
> public-facing hub linking to the other HarterHill Network properties
> (Chuck's List, HolistiveHive).

## Integration Path

The Identity module is the primary integration point. TechSpecific_Website
is the first place most visitors encounter the Network, so it is the
natural anchor for portable identity across all connected projects.

| TechSpecific_Website Phase | Network Integration |
|---|---|
| Current (business site) | No changes — site runs independently |
| Identity rollout | Site adopts Network DID for a single sign-on-style login; visitors carry the same identity into Chuck's List and HolistiveHive |
| Later | Site surfaces Board module content (e.g. Chuck's List highlights) as a lightweight embed |

## What Does Not Change
- Site ownership, branding, and business content
- Hosting/deployment targets (MochaHost / Cortez Web Services / cPanel)

## Status
> Not started. Integration begins once the Identity module spec (DID method,
> see `docs/open-questions.md` Q1) is resolved.
