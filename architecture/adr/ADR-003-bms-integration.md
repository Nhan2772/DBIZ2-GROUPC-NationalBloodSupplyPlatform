# ADR-003: Integrate with the Blood Management Software through one adapter

> DBIZ 2 – Group C. NBSP architecture decision.

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-11-19 (architecture v1) |

**Context.** NBC inventory lives in its existing Blood Management Software (BMS); NBSP must not make
NBC enter stock twice (StR-06). Reading stock is frequent and can tolerate a short delay, but
reserving units for an order must be exact at that moment. The Context Diagram says "real-time
inventory sync", while the Use Case Diagram says UC-09 reads periodically synced data (open point 5
in [architecture](../architecture.md)).

**Decision.** All traffic with the BMS goes through one BMS adapter. Reading stock uses a periodic
sync into an NBSP inventory snapshot (UC-17). Reserving units when an order is dispatched uses a
direct call to the BMS (UC-08).

**Consequences.** The sync never writes back to the BMS; if the BMS returns an error or invalid data,
the previous snapshot is kept and the failure is logged (FR-02-05). Screens show "Đồng bộ gần nhất"
with the snapshot time and never call the data real-time (FR-02-02). If the BMS is unavailable when
an order is approved, approval is blocked (FR-01-03). W1 carries INT work items for the scheduled
connector (FR-02-05-INT) and the reservation call with timeout handling (FR-01-03-INT). The Context
Diagram wording "real-time" is to be corrected. Open point 5 is closed.
