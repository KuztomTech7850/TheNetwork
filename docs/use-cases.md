# Use Cases — The Network

See [`docs/vision.md`](vision.md) for the full narrative.
This file is the canonical list of defined use cases and their module requirements.

| Use Case | Required Modules | Notes |
|---|---|---|
| Special-needs family community | Identity, Storage, Board, Messaging | Profile-driven resource matching; high privacy requirements |
| Community bulletin board | Identity, Board | Low friction; trust/reputation layer replaces anonymous classifieds |
| Vulnerable citizens hub | Identity, Storage, Voting, Messaging | Highest privacy bar; vote anonymity critical |
| Town governance | Identity, Voting, Board | Requires Sybil resistance; results must be publicly auditable |
| Personal dashboard (HolistiveHive) | Identity, Storage, Board (read) | Network client app; pillars pull community data as feeds |