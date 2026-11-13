# W0 Scope: NBSP

Lesson 5 (Function List workshop, block W0). Inputs: [CVCA](../context/cvca.md), [Context Diagram](../context/context-diagram.md), [Use Case Diagram](../use-case/use-case-diagram.md).

## Table 1. Use case inventory

| Use case ID | Use case name | Primary actor | Assigned feature | Login required |
|---|---|---|---|---|
| UC-01 | Request Blood Provision | Hospital Staff | F-01 | Yes |
| UC-02 | View Shipment Details | Hospital Staff | F-01 | Yes |
| UC-03 | Confirm Blood Received | Hospital Staff | F-01 | Yes |
| UC-04 | Record Blood Usage | Hospital Staff | F-02 | Yes |
| UC-05 | Log Surgical Schedules with Blood Demand | Hospital Staff | F-03 | Yes |
| UC-06 | Sign in with Hospital account | Hospital Staff | F-04 | No (sign-in) |
| UC-07 | View Request Queue | NBC Blood storage & Distribution dept | F-01 | Yes |
| UC-08 | Approve & Dispatch Blood Order | NBC Blood storage & Distribution dept | F-01 | Yes |
| UC-09 | View Inventory Dashboard | NBC Blood storage & Distribution dept, Hospital Staff | F-02 | Yes |
| UC-10 | View Near-Expiry Blood Units | NBC Blood storage & Distribution dept | F-02 | Yes |
| UC-11 | Receive Below-Threshold Alert | NBC Blood storage & Distribution dept | F-02 | Yes |
| UC-12 | Sign in with NBC account | NBC Blood storage & Distribution dept | F-04 | No (sign-in) |
| UC-13 | Validate Clinical Demand Data | NBC Forecasting Analyst | F-03 | Yes |
| UC-14 | Respond to Data Verification Request | Hospital Staff | F-03 | Yes |
| UC-15 | Review and Approve Blood Forecast | NBC Forecasting Analyst | F-03 | Yes |
| UC-16 | View Validated Blood Forecast | NBC Operations | F-03 | Yes |
| UC-17 | Synchronise NBC Inventory | Blood Management Software | F-02 | No (system-initiated) |

## Table 2. Feature list (flows into W1)

| Feature ID | Feature name (business noun) | Scope in one sentence | Primary actors served | Owner | Use cases covered |
|---|---|---|---|---|---|
| F-01 | Blood Ordering & Fulfillment | Manage blood requests from submission and queueing through approval, dispatch, shipment visibility and receipt confirmation. | Hospital Staff, NBC Blood storage & Distribution dept | Owner A | UC-01, UC-02, UC-03, UC-07, UC-08 |
| F-02 | Inventory Monitoring & Usage | Maintain hospital blood usage and inventory visibility, while surfacing near-expiry units and below-threshold alerts. | Hospital Staff, NBC Staff, Blood Management Software | Owner B | UC-04, UC-09, UC-10, UC-11, UC-17 |
| F-03 | Clinical Demand & Forecasting | Capture forward demand, validate hospital data, generate and approve forecasts, and expose validated forecasts for planning. | Hospital Staff, NBC Forecasting Analyst, NBC Operations | Owner C | UC-05, UC-13, UC-14, UC-15, UC-16 |
| F-04 | Access & Authorization | Authenticate hospital and NBC users and enforce role- and department-scoped access across NBSP. | All NBSP users | Owner D | UC-06, UC-12 |

Feature count: 4 (allowed 3 to 6): PASS. Coverage against the CVCA: every primary actor (Hospital Staff, NBC Blood storage & Distribution dept, NBC Forecasting Analyst, NBC Operations) is served by at least one feature.

## Table 3. Won't-have (recorded, never dropped silently)

| Excluded from scope | How it surfaced | Reason for exclusion |
|---|---|---|
| Blood stock intake and blood donation management | Use Case List - External System Integration assumption | The existing Blood Management Software remains the source system for NBC stock intake and donation operations. |
| Model development, training, evaluation and retraining | Use Case List - Processes Outside the System | Handled by the NBSP Data/ML Team outside NBSP; NBSP only runs the active deployed model. |
| Blood allocation planning / optimisation | UC-16 and Processes Outside the System | NBC Operations performs allocation planning outside NBSP; actual requests and dispatches remain UC-01 and UC-08. |
| Replacement of the existing Blood Management Software | Context Diagram / CVCA | The existing software remains an external system connected to NBSP through API synchronisation. |
| Configuration screens for alert thresholds and anomaly-rule parameters | Context Diagram; UC-11 and UC-13 assumptions | Configured by an NBSP administrator at deployment, as for accounts; no dedicated use case in this release. Values stay configurable, not hard-coded. |

## Table 4. Data functions

| Data group | ILF / EIF | Why | Function points |
|---|---|---|---|
| Blood request & shipment records | ILF | NBSP creates and maintains request status, selected units, ETA and shipment confirmation. | 10 |
| Hospital inventory & usage records | ILF | NBSP maintains hospital inventory through receipt and usage/discard transactions. | 10 |
| Clinical demand & verification records | ILF | NBSP stores scheduled/periodic demand inputs, validation status, verification requests and responses. | 10 |
| Forecast, review & version history | ILF | NBSP stores draft/approved forecasts, adjustments, reviewer decisions and immutable publication versions. | 10 |
| User accounts & role assignments | ILF | NBSP authenticates hospital/NBC accounts and applies role- and department-scoped permissions. | 10 |
| NBC inventory snapshot | EIF | NBSP reads periodically synchronised NBC inventory from the existing Blood Management Software and does not maintain the source records. | 7 |
| **Total data-function points** |  |  | **57** |

The data group names are the names Screen Specs use as data sources (Lesson 8).

## Business rules

| BR | Rule |
|---|---|
| BR-01 | A blood request needs a blood type chosen from the 8 valid types (A+, A-, B+, B-, AB+, AB-, O+, O-), an urgency chosen from the agreed list, and a quantity that is a whole number of 1 or more. A missing or invalid value means no request is created. |
| BR-02 | A request moves in order Pending, Dispatched, Received. Only a Pending request can be approved and dispatched. |
| BR-03 | Hospital Staff can confirm receipt only for a shipment in Dispatched status, and only once per shipment, so units are never added to hospital inventory twice. |
| BR-04 | Only eligible blood units (not expired, available) can be assigned to an order. If eligible stock is insufficient, dispatch is blocked. |
| BR-05 | The request queue is ordered by urgency first, then by submission time. |
| BR-06 | Hospital Staff see only their own hospital's requests and inventory. NBC users see only the data allowed for their role and department. |
| BR-07 | A forecast is published only after the NBC Forecasting Analyst approves it. A rejected forecast is not published. |
| BR-08 | A demand record whose verification request is still unanswered at the forecast cut-off is excluded from the forecast. |
