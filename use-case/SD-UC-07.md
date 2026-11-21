# Sequence Diagram: SD-UC-07 View request queue

| Field | Value |
|---|---|
| Use case | UC-07 View request queue |
| Version | v1 |
| FRs | FR-01-02 |

```mermaid
sequenceDiagram
  autonumber
  actor S as NBC Blood Storage and Distribution Department
  participant P as NBSP Web App
  participant B as NBSP Backend
  participant D as NBSP Database
  S->>P: open request queue
  P->>B: get pending requests (session)
  B->>B: check role and scope (FR-04-03)
  B->>D: query Pending requests in scope
  D-->>B: pending requests
  alt query succeeded
    B-->>P: requests ordered by urgency, then submission time (BR-05)
    P-->>S: display request queue (FR-01-02)
  else error
    B-->>P: error
    P-->>S: show error message
  end
```
