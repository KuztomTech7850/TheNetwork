# Incident Response

> Status: Draft — to be exercised before Milestone 1 (Secure family
> shell) ships to any real family Trust.

## Scope

Covers security incidents affecting accounts, Trust/estate records,
audit integrity, backups, or any integration adapter once enabled.

## Process

1. **Detect** — structured logs, audit events, and monitoring surface an
   anomaly (failed-auth spikes, unexpected data export, integrity-check
   failure).
2. **Triage** — on-call/responsible engineer classifies severity using
   the data-classification tiers in `DATA_CLASSIFICATION.md` (a T2/T3
   exposure is always high severity).
3. **Contain** — revoke sessions/credentials, disable the affected
   adapter or feature flag, isolate the affected organization/tenant if
   isolation failure is suspected.
4. **Eradicate & Recover** — patch the root cause, restore from a known-
   good encrypted backup if data integrity is in question (see
   `RECOVERY_RUNBOOK.md`), rotate any exposed keys/secrets.
5. **Notify** — affected account holders and the repository operator are
   notified per the vulnerability-disclosure policy; regulatory
   notification obligations are evaluated per the data classification
   involved.
6. **Record** — file an `AuditEvent`-backed incident record and a
   retrospective entry in this document's log (below). Update
   `THREAT_MODEL.md` and `RISK_REGISTER.md` if the incident reveals a new
   or under-rated threat.

## Roles

- **Responsible engineer** — leads containment and eradication.
- **Repository operator** — approves external notification and any
  emergency administrative action; final sign-off on closure.
- **Reviewer** (Definition of Done) — verifies the retrospective and
  documentation updates before the incident is closed.

## Vulnerability Disclosure

Security issues should be reported privately to the repository operator
before any public disclosure. A formal disclosure email/contact and SLA
should be published once the project has a public deployment (tracked as
a Milestone 1 exit item).

## Incident Log

| Date | Summary | Severity | Resolution | Follow-up |
|---|---|---|---|---|
| — | No incidents recorded yet | — | — | — |
