# Sequence Diagram: SD-UC-05 Log surgical schedules with blood demand

| Field | Value |
|---|---|
| Use case | UC-05 Log surgical schedules with blood demand |
| Version | v1 |
| FRs | FR-03-01 |

```mermaid
sequenceDiagram
  autonumber
  actor S as Hospital Staff
  participant P as NBSP Web App
  participant B as NBSP Backend
  participant D as NBSP Database
  S->>P: select plan type (scheduled surgery or periodic transfusion)
  S->>P: enter details (date, blood type, product, units; for periodic: number of patients, cycle)
  P->>B: submit plan
  B->>B: validate input data
  alt valid data (FR-03-01)
    B->>D: save plan as forecasting input
    D-->>B: saved
    B-->>P: success
    P-->>S: show confirmation
  else invalid data (FR-03-01, IF clause)
    B-->>P: reason, nothing saved
    P-->>S: show the reason and request re-entry
  end
```
