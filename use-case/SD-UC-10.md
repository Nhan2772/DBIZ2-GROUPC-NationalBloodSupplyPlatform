# Sequence Diagram: SD-UC-10 View near-expiry blood units

| Field | Value |
|---|---|
| Use case | UC-10 View near-expiry blood units |
| Version | v2 |
| FRs | FR-02-03 |
| Decisions | D-01, D-24 |

```mermaid
sequenceDiagram
  autonumber
  actor D as NBC Blood Storage and Distribution Department
  participant W as NBSP Web App
  participant BE as NBSP Backend
  participant DB as NBSP Database

  D->>W: Open near-expiry list
  activate W
  W->>BE: Request near-expiry blood units
  activate BE
  BE->>DB: Query blood units within NBC scope
  activate DB
  DB-->>BE: Blood unit list
  deactivate DB
  BE->>BE: Filter units within warning threshold
  BE->>BE: Calculate remaining days
  alt Near-expiry units found
    BE-->>W: Unit ID, blood type, expiry date, storage location, days left
    W-->>D: Display near-expiry list
    D->>D: Review list and decide on allocation
  else No units found
    BE-->>W: Empty list
    W-->>D: Show no near-expiry units message
  end
  deactivate BE
  deactivate W
```
