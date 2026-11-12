# Sequence Diagram: SD-UC-17 Synchronise NBC Inventory

| Field | Value |
|---|---|
| Use case | UC-17 Synchronise NBC Inventory |
| Version | v1 (new use case, approved 2026-11-01; started by time, not by a person) |
| FRs | FR-02-05 (threshold check after a valid sync: FR-02-04, see SD-UC-11) |

```mermaid
sequenceDiagram
    autonumber
    participant SCH as Scheduler
    participant NBSP as NBSP System
    participant DB as NBSP Database
    participant BMS as Blood Management Software

    Note over SCH,NBSP: Trigger - scheduled synchronisation runs every N min (interval configurable, to be confirmed)

    SCH->>+NBSP: Start inventory synchronisation
    NBSP->>+BMS: Request latest NBC inventory (API, read-only)
    BMS-->>-NBSP: Latest inventory with source timestamp, or error (no reply in time limit = error)

    alt Valid inventory data received (FR-02-05)
        NBSP->>DB: Update inventory snapshot with source timestamp
        DB-->>NBSP: Snapshot updated
        NBSP->>NBSP: Check thresholds and create alerts (SD-UC-11, FR-02-04)
    else BMS error or invalid data (FR-02-05)
        Note right of NBSP: Invalid = negative or non-integer units, unknown blood type
        NBSP->>DB: Keep previous snapshot, log the failure
        Note right of NBSP: No threshold check. Next interval retries
    end
    deactivate NBSP

    Note over DB: Dashboards (UC-09, UC-16) show the snapshot with its sync timestamp
```
