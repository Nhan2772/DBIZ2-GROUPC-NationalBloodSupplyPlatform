# ADR-005: Build and retrain the forecast model outside NBSP

> DBIZ 2 – Group C. NBSP architecture decision.

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-11-19 (architecture v1) |

**Context.** NBC needs a demand forecast that its analysts check before it is used (StR-04).
Building and retraining a model is data-science work done by the NBSP Data/ML Team, not by NBSP
users, and it needs its own tools and release cycle (Use Case Diagram note).

**Decision.** The forecast model is built and retrained outside NBSP. NBSP only runs the current
versioned model package to produce draft forecasts, and keeps the analysts' rejection reasons for
the Data/ML Team to review.

**Consequences.** No use case for training or retraining. The Forecast service invokes the active
model package and records its output and version with each draft (FR-03-05, FR-03-05-INT). The
analyst approves or rejects each item with a reason, and only approved items are published as a new
immutable version (FR-03-06, UC-15). Rejection reasons and decisions are kept in the NBSP database
with an audit record (FR-04-04). Handing over a new model package is a process agreed with the
Data/ML Team, outside the app.
