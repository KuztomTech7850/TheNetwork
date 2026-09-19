# Threat Model — v0.1

> Status: Draft — initial pass for Phase 0/1 (identity, membership, audit,
> encrypted storage). Revise as each phase adds capability.
> Method: OWASP threat-modeling process — decompose the system, identify
> and rank threats, select mitigations, validate the result.

## 1. System Decomposition (Phase 0–3 scope)

**Actors:** Person (account holder), Organization (Family/Trust),
Administrator, Trustee, Beneficiary, Advisor, unauthenticated visitor.

**Trust boundaries:**
- Public internet ↔ application edge (TLS termination, rate limiting)
- Authenticated session ↔ application services (authorization policy)
- Application services ↔ PostgreSQL (row/tenant scoping)
- Application services ↔ encrypted object storage (access grants,
  envelope encryption)
- Application services ↔ secrets/KMS (credential and key material)
- Application services ↔ integration adapters (Matrix, ATProto,
  anchoring) — all optional, all off by default until their phase gate

**Assets:** account credentials, session tokens, Trust/estate documents,
succession and emergency-contact data, private messages, audit logs,
backups, signing/encryption keys.

## 2. Threats (STRIDE-style, initial pass)

| # | Threat | Category | Mitigation |
|---|---|---|---|
| T1 | Credential stuffing / password reuse against accounts | Spoofing | Passkeys/WebAuthn preferred, MFA for privileged roles, rate limiting, breach-password checks |
| T2 | Session hijacking via token theft | Spoofing/Tampering | Short-lived sessions, secure/http-only cookies, device binding where possible |
| T3 | Privilege escalation via missing scope checks | Elevation of Privilege | Deny-by-default authorization; scoped-role policy engine; authorization test matrix (permitted + forbidden cases) per Definition of Done |
| T4 | Cross-organization data leakage (tenant isolation failure) | Information Disclosure | Every query scoped by organization/community ID; isolation tests required before merge |
| T5 | Administrator reads/exports sensitive documents without cause | Information Disclosure / Repudiation | Append-only audit events on all sensitive actions; dual-approval for high-impact admin actions |
| T6 | Lost or compromised recovery path locks out or hijacks a Trust account | Denial of Service / Spoofing | Passkeys + recovery codes + documented assisted-recovery runbook (see `RECOVERY_RUNBOOK.md`); no single-admin bypass |
| T7 | Tampering with audit log to hide an action | Tampering/Repudiation | Append-only, hash-chained audit events (ADR-0004) |
| T8 | Backup exposure or corruption | Information Disclosure / DoS | Encrypted backups, tested restore drills (Milestone 1 exit condition) |
| T9 | Sensitive data logged in plaintext (T2/T3 per `DATA_CLASSIFICATION.md`) | Information Disclosure | Structured logging references IDs only; log-scrubbing lint/check |
| T10 | Malicious/compromised dependency in the monolith | Tampering | Dependency and secret scanning, lockfiles, SBOM, signed releases |
| T11 | Abuse via unmoderated posting once Phase 4 content model lands | Repudiation / DoS | Audience rules, report/hide/lock/appeal workflow, moderator audit trail |
| T12 | Premature exposure of family data via an integration adapter (Matrix/ATProto/anchoring) enabled before its phase gate | Information Disclosure | Adapters ship disabled by default; enabling one requires the phase-gate exit condition in `ROADMAP.md` and a data-classification check |

## 3. Non-Goals (this phase)

- Cryptographic ballot secrecy (deferred to Phase 8, ADR-driven)
- Federation trust models beyond a single-instance deployment (Phase 7)
- Custody of transferable value (Phase 9, requires separate legal/economic
  threat model — see `RISK_REGISTER.md`)
- Statutory election security (explicitly out of scope; see
  `ENGINEERING_DEEP_DIVE.md` §7)

## 4. Validation

- Threat-model changes are required documentation for any Definition-of-
  Done checklist item that touches authentication, authorization,
  storage, or an integration adapter.
- Revisit this document at the start of each new phase in `ROADMAP.md`,
  and whenever a new integration adapter is proposed.

See also: [`SECURITY_REQUIREMENTS.md`](SECURITY_REQUIREMENTS.md),
[`INCIDENT_RESPONSE.md`](INCIDENT_RESPONSE.md),
[`RECOVERY_RUNBOOK.md`](RECOVERY_RUNBOOK.md).
