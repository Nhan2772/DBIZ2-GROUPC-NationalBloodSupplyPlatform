# Sequence Diagram: SD-UC-08 Review and process blood request

| Field | Value |
|---|---|
| Use case | UC-08 Review and process blood request |
| Version | v1 |
| FRs | FR-01-03, FR-01-08, FR-01-09 |

```mermaid
sequenceDiagram
  autonumber
  actor N as NBC Staff
  participant P as NBC Portal
  participant S as NBSP System
  participant DB as NBSP Database
  participant B as Blood Management Software
  N->>P: Review pending blood request
  activate P
  P->>S: Get request details and available blood units
  activate S
  S->>DB: Query request status and stock snapshot
  activate DB
  DB-->>S: Request details and stock availability
  deactivate DB
  S-->>P: Return request details and available units
  deactivate S
  P-->>N: Display request and available blood units
  alt NBC Staff declines request (FR-01-09)
    N->>P: Decline request and provide reason
    P->>S: Submit non-approval decision and reason
    activate S
    S->>DB: Record non-approval and block dispatch
    activate DB
    DB-->>S: Decision recorded
    deactivate DB
    S-->>P: Return non-approval confirmation
    deactivate S
    P-->>N: Display non-approval result
  else NBC Staff approves request (FR-01-03)
    N->>P: Select units, enter ETA and confirm dispatch
    P->>S: Submit approval, selected units and ETA
    activate S
    S->>DB: Revalidate request status and stock availability
    activate DB
    DB-->>S: Validation result
    deactivate DB
    alt Request invalid or no longer Pending
      S-->>P: Block dispatch and return validation reason
      P-->>N: Display invalid-request error
    else Insufficient eligible units (FR-01-08)
      S-->>P: Block dispatch and report stock shortage
      P-->>N: Display shortage reason
    else Request valid and stock sufficient
      S->>B: Reserve selected eligible blood units
      activate B
      alt Reservation failed (FR-01-08)
        B-->>S: Units unavailable for reservation
        S-->>P: Block dispatch and report reservation failure
        P-->>N: Display shortage or reservation conflict
      else Reservation successful (FR-01-03)
        B-->>S: Confirm reserved units and expiry dates
        S->>DB: Save shipment, ETA and status as Dispatched
        activate DB
        DB-->>S: Shipment and status saved
        deactivate DB
        S-->>P: Return dispatch confirmation
        P-->>N: Display dispatch confirmation
      end
      deactivate B
    end
    deactivate S
  end
  deactivate P
```

