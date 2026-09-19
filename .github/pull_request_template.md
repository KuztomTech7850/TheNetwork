## Summary

<!-- What does this change do, and why? Link the relevant Epic/Milestone from docs/ROADMAP.md. -->

## Required Fields (Definition of Done — docs/ROADMAP.md)

- **Epic:**
- **Milestone:**
- **Priority:**
- **Security impact:** <!-- none / low / medium / high — explain -->
- **Data classification:** <!-- see docs/DATA_CLASSIFICATION.md; note any new/changed record types and their tier -->
- **Decision dependency:** <!-- related ADR or open question, if any -->
- **Responsible engineer:**
- **Reviewer:**

## Checklist

- [ ] Acceptance criteria pass
- [ ] Authorization tests cover permitted and forbidden cases
- [ ] Data classification is documented for any new/changed record type
- [ ] Database migrations include rollback or recovery instructions
- [ ] Audit-event requirements are satisfied (`spec/core/audit-events.md`)
- [ ] Unit and integration tests pass
- [ ] User-facing behavior has an end-to-end test
- [ ] Threat-model changes are recorded (`docs/security/THREAT_MODEL.md`)
- [ ] API and operational documentation are updated
- [ ] Accessibility is checked
- [ ] Reviewed by someone other than the author

## Test Evidence

<!-- Paste test output, screenshots, or links to CI runs. -->

## Documentation Impact

<!-- Which docs/spec files were updated as a result of this change? -->
