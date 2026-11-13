# Sequence Diagram: SD-UC-06 Sign in with hospital account

Lesson 4, analysis level: how the parts of NBSP talk to each other when Hospital Staff sign in with a hospital account. The lifelines are the elements of the
[physical design](../architecture/architecture.md). The user's own actions (what they choose or
type) are shown in the activity diagram [AD-UC-06](../activity/AD-UC-06.md), not here.

| Field | Value |
|---|---|
| Use case | UC-06 Sign in with hospital account |
| Version | v1 |
| FRs | FR-04-01 |

```mermaid
sequenceDiagram
  actor H as Hospital Staff
  participant P as NBSP Portal
  participant B as NBSP Central Backend
  participant D as NBSP Database
  H->>P: enter account name and password, click "Sign In"
  P->>B: send credentials payload (username, password)
  B->>D: query hospital account and verify credentials
  D-->>B: user data and password hash
  B->>B: verify password and generate access token
  alt valid credentials
    B-->>P: return auth success and access token / session
    P-->>H: grant access and open hospital dashboard functions
  else invalid credentials
    B-->>P: return authentication error
    P-->>H: show error message ("Invalid username or password")
  end
```

The `alt` fragment is an outcome decided inside the system, so it belongs in a sequence diagram.
