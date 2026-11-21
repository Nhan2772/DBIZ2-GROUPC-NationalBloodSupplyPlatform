# ADR-002: NBSP signs users in itself in release 1

> DBIZ 2 – Group C. NBSP architecture decision.

| Field | Value |
|---|---|
| Status | Amended (release 1) |
| Date | 2026-11-19 (architecture v1) |

**Context.** Users come from two kinds of organisation: 180+ hospitals and NBC. Each session must
carry the user's organisation and role, because data is scoped per hospital and per NBC
department (FR-04-01, FR-04-02, FR-04-03). No shared identity provider covering every hospital and
NBC is confirmed for release 1.

**Decision.** In release 1 NBSP signs users in itself. An NBSP administrator creates the accounts
and assigns organisation and role; passwords are stored only as salted hashes. An external
identity provider stays a later option.

**Consequences.** No self-registration use case: accounts come from the administrator. The NBSP
database keeps organisations, users and roles, and the password hashes. Hospital and NBC accounts
share the sign-in screen S-F04-01 (UC-06, UC-12). Password reset and account administration are not
yet use cases: they must be scoped in W1 or recorded as out of scope. Moving to an external identity
provider later needs a new ADR that supersedes this one.
