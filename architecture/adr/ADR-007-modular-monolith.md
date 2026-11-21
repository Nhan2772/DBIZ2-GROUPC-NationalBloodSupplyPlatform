# ADR-007: Build the business layer as one deployable with separate modules

> DBIZ 2 – Group C. NBSP architecture decision.

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-11-19 (architecture v1) |

**Context.** The business layer has four services (Order, Clinical data, Forecast, Inventory), a
Scheduler and two adapters (BMS, Notification). Separate deployables would each need their own
deployment, pipeline, monitoring and data store, and calls between them would cross the network.
The delivery pipeline must run unit, integration and smoke tests before each release (FR-04-05).

**Decision.** Build the business layer as one deployable with separate modules and one NBSP
database. Modules talk to each other only through their module interfaces.

**Consequences.** One build, one test gate and one deployment for the whole business layer
(FR-04-05). One database keeps organisations, users and roles, requests and shipments, usage
records, schedules, verification requests, forecasts, inventory snapshots, thresholds and alerts.
Role and data-scope checks (FR-04-03) run in one place for every module. Clear module boundaries
keep the option to split a module out later; that split would need a new ADR.
