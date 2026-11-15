# Sequence Diagram: SD-UC-09 View inventory

| Field | Value |
|---|---|
| Use case | UC-XX View inventory |
| Version | v1 |
| FRs | FR-02-02 |

```mermaid
sequenceDiagram
  autonumber
  actor H as Hospital Staff
  actor N as NBC Staff
  participant D as NBSP Dashboard
  participant S as NBSP Inventory Service
  participant DB as NBSP Inventory Data
  Note over H,DB: Precondition - Hospital signed in via UC-06 and NBC signed in via UC-12
  alt Hospital Staff views inventory
    H->>D: Open Inventory Dashboard
    activate D
    D->>S: Request inventory with user session
    activate S
    S->>S: Identify hospital role and access scope
    S->>DB: Query own hospital inventory and NBC available stock
    activate DB
    DB-->>S: Return inventory data and latest sync time
    deactivate DB
    S-->>D: Return authorised inventory data
    deactivate S
    D-->>H: Display own hospital inventory and NBC available stock
    deactivate D
    Note over H,D: Other hospitals inventory is not displayed
  else NBC Staff views inventory
    N->>D: Open Inventory Dashboard
    activate D
    D->>S: Request inventory with user session
    activate S
    S->>S: Identify NBC role and access scope
    S->>DB: Query NBC inventory and permitted hospital inventories
    activate DB
    DB-->>S: Return inventory data and latest sync time
    deactivate DB
    S-->>D: Return authorised inventory data
    deactivate S
    D-->>N: Display NBC and permitted hospital inventories
    deactivate D
  end
```
