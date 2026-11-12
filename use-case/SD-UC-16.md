# Sequence Diagram: SD-UC-16 View Validated Blood Forecast

| Field | Value |
|---|---|
| Use case | UC-16 View Validated Blood Forecast |
| Version | v1 |
| FRs | FR-03-07; data source: FR-02-05 (periodic sync) |

```mermaid
sequenceDiagram
    autonumber
    actor OPS as NBC Operations
    participant Portal as NBSP Portal
    participant FS as Forecast Service
    participant DB as NBSP Database

    OPS->>+Portal: Select scope (NBC-wide or one hospital), blood group, time horizon
    Portal->>+FS: Get approved forecast for the selection
    FS->>+DB: Get latest approved forecast, approval time, recent accuracy, and current inventory (read-only)
    DB-->>-FS: Forecast data and inventory (with last sync time)
    FS-->>Portal: Result

    alt Approved forecast available
        Portal-->>OPS: Show approved forecast with approval time and recent accuracy, next to current inventory and last sync time
    else No approved forecast
        Portal-->>-OPS: Show "No approved forecast is available for the selected period" and current inventory with last sync time
    end
    deactivate FS

    Note over OPS: Allocation planning is carried out outside the system
    Note over DB: NBC inventory is synced periodically from Blood Management Software
```
