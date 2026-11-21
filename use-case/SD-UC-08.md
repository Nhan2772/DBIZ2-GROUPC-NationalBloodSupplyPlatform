# Sequence Diagram: SD-UC-08 Review and process blood request
 
| Field | Value |
|---|---|
| Use case | UC-08 Review and process blood request |
| Version | v3 (21/11/2026: separate branch for a Blood Management Software that does not answer or errors; refusal and save failure kept) |
| FRs | FR-01-03, FR-01-08, FR-01-09 |
| Decisions | D-04, D-07, D-09, D-24 |
| Screen | S-F01-04 |
 
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
  W->>BE: Get request details and eligible units
  activate BE
  BE->>DB: Query request status and stock snapshot
  activate DB
  DB-->>BE: Request details and eligible units with expiry dates
  deactivate DB
  BE->>BE: Pre-select units by FEFO (earliest expiry first)
  BE-->>W: Request details, eligible units and FEFO pre-selection
  deactivate BE
  W-->>D: Display request with pre-selected units
 
  alt Not approve request (FR-01-09)
    D->>W: Choose Not approve request and select reason from list
    W->>BE: Submit Not approved decision and reason
    activate BE
    BE->>DB: Save status Not approved and reason
    activate DB
    DB-->>BE: Decision saved
    deactivate DB
    BE-->>W: Not approved confirmation
    deactivate BE
    W-->>D: Display request as Not approved
    Note over W,DB: Final state Not approved. Hospital sees the status and the reason.
  else Dispatch request (FR-01-03)
    D->>W: Keep or adjust selected units, enter ETA, confirm dispatch
    W->>BE: Submit dispatch with selected units, ETA and idempotency key
    activate BE
    Note over W,BE: Same idempotency key returns the first result, no second dispatch
    BE->>DB: Revalidate request status and selected units
    activate DB
    DB-->>BE: Validation result
    deactivate DB
    alt Request no longer valid (not Pending)
      BE-->>W: Reject dispatch, request no longer valid
      W-->>D: Display request no longer valid message
    else Insufficient eligible units (FR-01-08)
      BE-->>W: Reject dispatch and report stock shortage
      W-->>D: Display shortage reason
    else Request valid and stock sufficient
      BE->>BMS: Reserve selected units (one attempt, timeout 10 s, D-09)
      alt BMS does not answer in time or returns an error (FR-01-03)
        Note over BE,BMS: No reply within the time limit counts as an error
        BE-->>W: Block dispatch, stock service unavailable, nothing reserved
        W-->>D: Display that the stock service is unavailable, request stays Pending, try again later
      else Reservation refused, units no longer available (FR-01-08)
        BMS-->>BE: Units unavailable for reservation
        BE-->>W: Reject dispatch and report reservation conflict
        W-->>D: Display shortage or reservation conflict, reselect units
      else Reservation successful
        BMS-->>BE: Reserved units and expiry dates
        BE->>DB: Save shipment, ETA and status Dispatched
        activate DB
        alt Save failed (FR-01-03)
          DB-->>BE: Save error
          BE->>BMS: Release reserved units
          BMS-->>BE: Reservation released
          BE-->>W: Dispatch failed, no units held
          W-->>D: Display error, request stays Pending, ask to retry
        else Save successful
          DB-->>BE: Shipment and status saved
          BE-->>W: Dispatch confirmation
          W-->>D: Display dispatch confirmation
        end
        deactivate DB
      end
    end
    deactivate BE
  end
  deactivate W
```
 
Notes:
 
- The alt branches match the decisions of [AD-UC-08](../activity/AD-UC-08.md) v0.6: "Not approve" = d0, "request no longer valid" = d2, "insufficient units" = d1, "BMS not answering" = d3 [service unavailable or error], "reservation refused" = d3 [units refused], "Save failed" = d4 [save failed].
- In every error branch the request stays Pending and no unit stays reserved. The only branch that changes the request to a final state without dispatch is Not approved (FR-01-09).
- The screen states are described in S-F01-04 v2.1 (section 4): one error state for each failure branch.
 
