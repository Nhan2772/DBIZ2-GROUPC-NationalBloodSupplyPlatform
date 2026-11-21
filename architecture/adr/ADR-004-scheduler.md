# ADR-004: Add a scheduler for timed jobs

> DBIZ 2 – Group C. NBSP architecture decision.

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-11-19 (architecture v1) |

**Context.** Several jobs in NBSP start with nobody clicking anything: the periodic inventory sync
(UC-17), the threshold check after each sync (UC-11, FR-02-04) and the draft forecast once demand
data is finalised (FR-03-05). The Use Case Diagram note on UC-15 says draft forecasts come from a
background job that is not a use case (open point 7 in [architecture](../architecture.md)).

**Decision.** Add a Scheduler in the business layer. It starts the inventory sync through the BMS
adapter, the threshold check in the Inventory service after each sync, and the forecast generation
in the Forecast service.

**Consequences.** UC-17, UC-11 and FR-03-05 have no screen of their own; their results appear on
S-F02-02 and S-F03-04 or by notification (Screen List, "What is deliberately not a screen"). The
sync interval and the forecast trigger time are configuration values to be set. Other timed rules,
such as the forecast cut-off (FR-03-08) and the verification deadline (FR-03-04), are candidates
for the same Scheduler. Open point 7 is closed.
