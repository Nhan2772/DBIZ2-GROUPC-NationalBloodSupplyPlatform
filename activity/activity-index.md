# Activity Diagram Index: National Blood Supply Platform (NBSP)

One row per use case in the Use Case inventory. Every use case appears, even when it needs no
diagram, so a reader can see that the decision was made on purpose.

## Inputs check (Block 1: what Lesson 3 and Lesson 4 gave us)

| # | Finding | Source to fix | Action | Owner | Status |
|---|---|---|---|---|---|
| 1 | UC-17 "Synchronize NBC Inventory" appears in the Use Case Diagram but is missing from the UC list, the Function List and the W5 handover | UC List, Function List | Add UC-17 with a function ID and priority, or remove it from the diagram | Nhàn, Nga | Open |
| 2 | Four use cases run on a timer or a background job: UC-11 (threshold check after sync), UC-14 (overdue after the response deadline), UC-15 (scheduled draft forecast) and UC-17 (periodic sync). No scheduler appears in the architecture | Architecture | ADR "Add a scheduler for timed jobs" | TBD | Open |
| 3 | The Function List numbers one function per use case (F-01 to F-16), but the W5 handover uses FR-01 to FR-04 groups and names owning features F-02 and F-03 that mean something else in the Function List (for example F-02 is "Request blood provision", not near-expiry) | Function List, W5 handover | Choose one numbering and update both files | Minh Anh, Nhàn | Open |
| 4 | UC-08 main flow only describes approval, but the sequence also covers decline (FR-01-09), stock shortage and reservation failure (FR-01-08) | UC List | Add an Alt Flow for decline and the failure cases | Hằng | Open |
| 5 | Actor names differ between documents: the Context Diagram says "NBC Analysts" and "Receiving Hospitals", the Use Case Diagram says "NBC Forecasting Analyst" and "Hospital Staff", the UC list says "Hospitals" and "NBC Staff" | Context Diagram, UC List | Use one actor name per role in all documents | Hằng, Nhàn | Open |
| 6 | Use case headings are inconsistent in the mid-term report (`UC-8`, `UC14`, `UC 12`), and a leftover note "The UC number must match the group's final version" is still in the UC-12 text | Mid-term report | Rewrite all headings as `UC-xx` and delete the note | Nhàn | Open |

## Use cases

Rule used in this course: draw an activity diagram for every **Must** use case that has at least
one decision, more than one lane, or a time trigger. A use case that is a single display step gets
"not needed" and a one-line reason. A use case has at most one diagram, and one diagram never
covers two use cases.

| Use case | Name | Feature | Priority | Activity diagram | Status | Reason if not needed |
|---|---|---|---|---|---|---|
| UC-01 | Request Blood Provision | F-02 | Must | `AD-UC-01.md` |  | |
| UC-02 | View Shipment Details | F-03 | Must | not needed | | Read-only display; no decision, one lane |
| UC-03 | Confirm Blood Received | F-04 | Must | `AD-UC-03.md` |  | |
| UC-04 | Record Blood Usage | F-05 | Must | `AD-UC-04.md` |  | |
| UC-05 | Log Surgical Schedules with Blood Demand | F-06 | Must | `AD-UC-05.md` |  | |
| UC-06 | Sign in with Hospital account | F-01 | Must | not needed |  | same sign-in for every flow, no business decision. |
| UC-07 | View Request Queue | F-08 | Must | not needed | | Single display step |
| UC-08 | Approve & Dispatch Blood Order | F-09 | Must | `AD-UC-08.md` |  | |
| UC-09 | View Inventory Dashboard | F-10 | Must | not needed |  | Single display step |
| UC-10 | View Near-Expiry Blood Units | F-11 | Should | not needed | | Should priority; one lane; only a list or empty-list display |
| UC-11 | Receive Below-Threshold Alert | F-12 | Must | `AD-UC-11.md` |  | |
| UC-12 | Sign in with NBC account | F-07 | Must | not needed |  | same sign-in for every flow, no business decision |
| UC-13 | Validate Clinical Demand Data | F-13 | Must | `AD-UC-13.md` |  | |
| UC-14 | Respond to Data Verification Request | F-14 | Must | `AD-UC-14.md` |  | |
| UC-15 | Review and Approve Blood Forecast | F-15 | Must | `AD-UC-15.md` |  | |
| UC-16 | View Validated Blood Forecast | F-16 | Must | not needed |  | Single display step |
| UC-17 | Synchronize NBC Inventory | TBD | Must | `AD-UC-17.md` |  | |

UC-17 is time-triggered, so it needs a diagram if it is confirmed as Must (see finding 1).

Object state lists collected from all diagrams (copy into the data model in DBIZ3):

| Object | States |
|---|---|
| Blood request | Pending, Dispatched, Not approved, Received |
| Blood unit | Available, Reserved, Used, Discarded |
| Demand record | Pending Verification, Accepted, Excluded |
| Verification request | Open, Responded, Overdue |
| Forecast | Draft, Approved, Rejected, Published |
| User account | Active, Locked |
