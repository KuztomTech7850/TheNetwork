# Security Requirements

> Status: Active — the OWASP ASVS 5.0 baseline is the application-security
> acceptance checklist for every phase in `ROADMAP.md`.

## Minimum Requirements (all phases)

- Deny-by-default authorization
- Tenant and community isolation tests
- Passkeys or multifactor authentication for privileged roles
- Encrypted transport (TLS) and storage (at-rest encryption)
- Key rotation and recovery runbooks
- Structured, append-only security audit events
- Rate limits and abuse controls
- Dependency and secret scanning
- Signed releases and dependency lockfiles
- Software bill of materials (SBOM)
- Backup restoration exercises
- Vulnerability disclosure policy
- Documented incident-response process
- No sensitive data in application logs
- Separate administrative and normal-user actions

## Standards Referenced

- **OWASP ASVS 5.0** — application-security acceptance checklist. Every
  Definition-of-Done review should map to relevant ASVS controls.
- **OWASP Threat Modeling** — repeatable process of decomposing the
  system, identifying/ranking threats, selecting mitigations, validating
  the result. See [`THREAT_MODEL.md`](THREAT_MODEL.md).
- **NIST SP 800-63 (Digital Identity Guidelines)** — separates identity
  proofing, authentication, authenticator management, and federation.
  The Network's actor model (ADR-0002) follows this separation rather
  than treating a DID as the entire identity system.
- **CISA Secure-by-Design principles** — secure defaults, built-in MFA,
  logging, and vendor ownership of customer security outcomes.

## Phase-Gated Additions

| Phase | Additional security requirement |
|---|---|
| 2 | Encrypted backups + successful restore drill before Trust data is stored |
| 5 | Deterministic, reproducible vote tallies; eligibility snapshot integrity |
| 6 | Matrix pilot must be independently disable-able without affecting core records or audit history |
| 7 | Export/import package integrity verified before any federation is enabled |
| 8 | Independent cryptographic verifier reproduces a published result before secret ballots ship |
| 9 | Legal review (securities, money-transmission, tax, AML/sanctions) completed before any ledger redemption/exchange feature ships |
| 10 | External accessibility and security review completed before any municipal pilot |

## Enforcement

Security requirements are part of the Definition of Done in
[`ROADMAP.md`](../ROADMAP.md). A change that touches authentication,
authorization, storage, or an adapter boundary is not complete until its
threat-model impact is recorded and the relevant items above are
satisfied.
