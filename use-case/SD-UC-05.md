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
  participant P as Hospital Blood Mgmt Portal
  participant D as Hospital Database
  participant F as Forecasting Engine
  S->>P: select plan type (scheduled surgery or periodic transfusion)
  S->>P: enter details (date, blood type, units; for periodic: number of patients, cycle)
  P->>P: validate input data
  alt valid data
    P-->>S: show confirmation message
    P->>D: save surgical schedule / transfusion plan as forecasting input (FR-03-01)
    D-->>P: return success response
  else invalid data
    P-->>S: show error message and request re-entry
  end
  P-->>S: show updated plan list (optional)
```
