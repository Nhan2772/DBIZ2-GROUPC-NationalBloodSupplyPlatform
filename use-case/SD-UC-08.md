# Sequence Diagram: SD-UC-08 Review and process blood request

| Field | Value |
|---|---|
| Use case | UC-08 Review and process blood request |
| Version | v2 |
| FRs | FR-01-03, FR-01-08, FR-01-09 |
| Decisions | D-01, D-04, D-07, D-09, D-24 |

```mermaid
sequenceDiagram
  autonumber
  actor D as NBC Blood Storage and Distribution Department
  participant W as NBSP Web App
  participant BE as NBSP Backend
  participant DB as NBSP Database
  participant BMS as Blood Management Software
  D->>W: Open pending blood request
  activate W
  W->>BE: Get request details and available blood units
  activate BE
  BE->>DB: Query request status and stock snapshot
  activate DB
  DB-->>BE: Request details and eligible units with expiry dates
  deactivate DB
  BE->>BE: Pre-select units by FEFO (earliest expiry first)
  BE-->>W: Request details, available units and FEFO pre-selection
  deactivate BE
  W-->>D: Display request with FEFO pre-selected units
  alt Dept does not approve request (FR-01-09)
    D->>W: Choose "Not approve request" and select reason from list
    W->>BE: Submit Not approved decision and reason
    activate BE
    BE->>DB: Save status Not approved with reason
    activate DB
    DB-->>BE: Decision saved
    deactivate DB
    BE-->>W: Return Not approved confirmation
    deactivate BE
    W-->>D: Display request as Not approved
    Note over BE,DB: Final state Not approved; status and reason are shown to the hospital
  else Dept dispatches request (FR-01-03)
    D->>W: Keep or adjust pre-selected units, enter ETA and confirm dispatch
    W->>BE: Submit dispatch, selected units and ETA
    activate BE
    BE->>DB: Revalidate request status and stock availability
    activate DB
    DB-->>BE: Validation result
    deactivate DB
    alt Request no longer valid (not Pending)
      BE-->>W: Reject dispatch with "no longer valid" reason
      W-->>D: Display request no longer valid
    else Insufficient eligible units (FR-01-08)
      BE-->>W: Block dispatch and report stock shortage
      W-->>D: Display shortage reason
    else Request valid and stock sufficient
      BE->>BMS: Reserve selected eligible blood units
      activate BMS
      alt Reservation failed (FR-01-08)
        BMS-->>BE: Units unavailable for reservation
        BE-->>W: Block dispatch and report reservation failure
        W-->>D: Display shortage or reservation conflict
      else Reservation successful (FR-01-03)
        BMS-->>BE: Confirm reserved units and expiry dates
        BE->>DB: Save shipment, ETA and status Dispatched
        activate DB
        DB-->>BE: Shipment and status saved
        deactivate DB
        Note over BE,BMS: If the save fails, Backend releases the reservation in BMS and returns an error
        BE-->>W: Return dispatch confirmation
        W-->>D: Display dispatch confirmation
      end
      deactivate BMS
    end
    deactivate BE
  end
  deactivate W
```
