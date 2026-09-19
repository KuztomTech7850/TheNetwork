# Recovery Runbook

> Status: Draft — must be exercised (restore drill) before Milestone 1
> ("Secure family shell") is considered complete, per `ROADMAP.md`.

## Account Recovery (Person locked out)

1. Preferred path: passkey re-registration via an existing trusted device
   or recovery code.
2. If recovery codes are exhausted: administrator-assisted recovery.
   - Requires identity re-verification appropriate to the account's role
     (higher bar for Trustee/Administrator than for Member).
   - Requires a second approver (dual control) for any Trustee or
     Administrator account — no single admin unilaterally recovers a
     high-privilege account.
   - Every step is written to the append-only audit log
     (`spec/core/audit-events.md`).
3. Social recovery (trusted contacts) is a candidate future mechanism —
   not implemented in the MVP. It must not become the *only* recovery
   path for Trust records if adopted later.

## Organization Continuity (Trust/Family succession)

1. Succession responsibility directory (Family Trust MVP feature) names
   who assumes Trustee/Administrator duties if the current holder is
   unavailable.
2. Emergency contact and continuity information is stored as T2 data
   (`DATA_CLASSIFICATION.md`) and is accessible to designated successors
   via an access grant, not a standing role.
3. Activating succession is itself a sensitive action: audited, and
   ideally requiring acknowledgement from more than one remaining member
   where the organization's policy defines a quorum for that action.

## Data Restore (backup recovery)

1. Backups are encrypted and tested on a defined schedule (see
   `SECURITY_REQUIREMENTS.md`).
2. Restore drills must succeed end-to-end (backup → restore → integrity
   check → audit-log continuity check) before Trust data is stored in a
   production instance — this is Milestone 1's exit condition.
3. A restore that would overwrite live data requires operator approval
   and is itself an audited action.

## Key Rotation

1. Envelope-encryption keys and any signing keys are rotated on a
   defined schedule and immediately upon suspected compromise.
2. Rotation must not require decrypting and re-encrypting all historical
   data in a single blocking operation where avoidable — prefer
   key-versioning so old data remains readable under its original key
   version while new data uses the rotated key.
3. Rotation events are audited.

## Post-Recovery

Every recovery or restore action closes with an audit-log entry and, for
anything above routine self-service password/passkey recovery, an entry
in `security/INCIDENT_RESPONSE.md`'s log if it was triggered by a
security event.
