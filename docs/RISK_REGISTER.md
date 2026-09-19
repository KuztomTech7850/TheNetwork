# Risk Register

> Status: Active — reviewed each milestone.
> Companion to: [`security/THREAT_MODEL.md`](security/THREAT_MODEL.md)

| Risk | Response |
|---|---|
| Excessive distributed-system complexity | Start with one deployable application and adapter boundaries (ADR-0001) |
| Immutable disclosure of family data | Prohibit sensitive payloads on public ledgers; enforce `DATA_CLASSIFICATION.md` tiers (ADR-0003) |
| Administrator overreach | Scoped roles, dual approval, audit logs, exportability |
| Lost credentials | Passkeys, recovery codes, documented assisted recovery |
| Identity conflated with wallet ownership | Keep accounts, people, organizations, and wallets separate (ADR-0002) |
| Social abuse | Audience rules, reporting, moderation, appeals |
| Vote coercion or leakage | Visible ballots first; reviewed cryptography for secrecy before Phase 8 |
| Token speculation | Defer transferable token; prohibit token-weighted governance (ADR-0007) |
| Municipal claims too early | Label early voting as community or nonbinding participation; gate Phase 10 on legal/accessibility review |
| Vendor lock-in | Open contracts, exports, adapters, reproducible deployment |

## Process

- Risks are added when identified during threat modeling, design review,
  or incident response — never silently absorbed into "known issues."
- Each risk must have an owner and a response strategy (avoid, mitigate,
  transfer, or accept) before a phase that depends on it can begin.
- Review this register at the start of each milestone
  (see [`ROADMAP.md`](ROADMAP.md)) and update responses as mitigations
  land.
- Risks tied to a specific ADR are cross-referenced above; do not
  duplicate the reasoning — link to the ADR instead.
