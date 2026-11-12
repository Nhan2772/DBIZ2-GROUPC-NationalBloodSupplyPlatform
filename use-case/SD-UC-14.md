# Sequence Diagram: SD-UC-14 Respond to Data Verification Request

| Field | Value |
|---|---|
| Use case | UC-14 Respond to Data Verification Request |
| Version | v1 |
| FRs | FR-03-04; supporting: FR-04-04 (audit of the response and change history) |

```mermaid
sequenceDiagram
    autonumber
    actor HS as Hospital Staff
    participant NBSP as NBSP System
    actor FA as NBC Forecasting Analyst

    alt Hospital responds before deadline
        HS->>+NBSP: Open verification request
        NBSP-->>-HS: Show flagged record, reason, and analyst note

        alt Confirm existing data
            HS->>NBSP: Confirm data is correct
        else Correct data
            HS->>NBSP: Submit corrected data
        else Supplement data
            HS->>NBSP: Submit additional information
        end

        activate NBSP
        NBSP->>NBSP: Save response and change history
        NBSP-->>-FA: Notify verification response received

    else Verification deadline is missed
        activate NBSP
        NBSP->>NBSP: Mark request as Overdue
        NBSP-->>-FA: Notify overdue verification request
    end
```
