# Sequence Diagram: SD-UC-15 Review and Approve Blood Forecast

| Field | Value |
|---|---|
| Use case | UC-15 Review and Approve Blood Forecast |
| Version | v1 |
| FRs | FR-03-05, FR-03-06; supporting: FR-04-04 (audit of review decisions); trigger comes from FR-03-08 (period Finalised) |

```mermaid
sequenceDiagram
    autonumber
    actor FA as NBC Forecasting Analyst
    participant NBSP as NBSP System
    participant AM as Active Model

    Note over FA,AM: Trigger - demand data of the period is validated and finalized (UC-13)

    NBSP->>+AM: Run active model with accepted demand data and NBC allocation history
    AM-->>-NBSP: Draft forecast items (NBC-wide and per hospital, by blood group and time horizon)
    NBSP->>FA: Notify - draft batch ready (hospitals with insufficient data are listed as skipped)

    FA->>+NBSP: Open draft batch
    NBSP-->>FA: Show Normal and Needs Review groups, flag reasons, previous forecast accuracy

    loop Each item in Needs Review
        alt Approve
            FA->>NBSP: Approve (adjust value with reason if needed)
        else Reject
            FA->>NBSP: Reject with reason and optional feedback category
            Note right of NBSP: Not published. Reason is kept for the NBSP Data/ML Team to review outside NBSP
        end
    end

    FA->>NBSP: Approve Normal group in one action
    NBSP->>NBSP: Record decisions (system, adjusted and approved values, reviewer, timestamp, notes)
    NBSP->>NBSP: Publish approved items as a new version (previous versions kept as history, rejected or undecided items excluded)
    NBSP-->>-FA: Confirm publication (approved, rejected, and still Draft counts)
```
