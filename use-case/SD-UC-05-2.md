# Sequence Diagram: SD-UC-05 Log surgical schedules with blood demand

| Field | Value |
|---|---|
| Use case | UC-05 Log surgical schedules with blood demand |
| Version | v2 |
| Date | 21/11/2026 |
| FRs | FR-03-01 (including its IF clause for invalid plan data) |
| Screen | S-F03-01 Blood demand log |
| Precondition | Hospital Staff is signed in (UC-06) |

```mermaid
sequenceDiagram
  autonumber
  actor S as Hospital Staff
  participant P as NBSP Portal (S-F03-01)
  participant D as NBSP Database
  S->>P: open Blood demand log
  S->>P: select plan type (scheduled surgery or periodic transfusion)
  S->>P: enter details (date, blood type, units, and for periodic transfusion the number of patients and the cycle)
  S->>P: submit plan
  P->>P: validate required demand fields (FR-03-01)
  alt valid data
    P->>D: save plan as forecasting input (FR-03-01)
    alt saved
      D-->>P: success
      P-->>S: show confirmation and the updated plan list
    else database error
      D-->>P: error
      P-->>S: show that the plan was not saved, keep entered values
    end
  else invalid data
    P-->>S: show the reason for each invalid field, keep entered values (FR-03-01 IF clause)
    Note over S,P: nothing is saved, Hospital Staff correct and resubmit (back to step 4)
  end
  Note over D: the saved plan is read later by the forecast run (FR-03-05), outside UC-05
```

## Changes from v1

| # | v1 | v2 | Why |
|---|---|---|---|
| 1 | Message 2 contained a `;` | Replaced with "and" | Mermaid reads `;` as the end of a statement, so v1 did not render (parse error on line 8) |
| 2 | Confirmation shown before saving | Confirmation shown only after the database returns success | The user must not see "saved" when the save fails |
| 3 | No branch for a database error | `else database error` branch | A valid plan can still fail to save |
| 4 | "Show updated plan list (optional)" after both branches | Inside the success branch | After invalid data there is nothing new to list |
| 5 | Participant "Forecasting Engine" declared but never used | Removed; a note says the forecast reads the plan later (FR-03-05) | Forecasting is not part of UC-05 |
| 6 | "Hospital Blood Mgmt Portal", "Hospital Database" | "NBSP Portal (S-F03-01)", "NBSP Database" | The plan is stored in NBSP, and the system name is NBSP (as in W1) |
| 7 | No explicit submit step; invalid branch did not cite an FR | "submit plan" step; invalid branch cites the FR-03-01 IF clause | Matches W1 FR-03-01 and the correct-and-resubmit loop in the usage flow |
