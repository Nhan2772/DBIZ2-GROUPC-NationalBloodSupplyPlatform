# Project brief: NBSP

Lessons 1 and 2 (kick-off, project management, handover from the client). Every later document
refers back to the names fixed here.

| Field | Value |
|---|---|
| Product | NBSP (National Blood Supply Chain Platform), a platform that connects hospitals and the National Blood Center to order, track and use blood, and to forecast blood demand |
| Client | National Blood Center (Trung tâm Máu Quốc gia), under the National Institute of Hematology and Blood Transfusion (NIHBT) |
| Client contact | Director of the National Blood Center |
| Team | Five members (see Team and roles) |
| Course | DBIZ2 Digital Business Project II (Group C), followed by DBIZ3 (build) |

## Problem

The National Blood Center has digitised mainly the donor side; distribution is almost not
digitised. Once blood is dispatched, the Center loses sight of it: 180+ hospitals do not
report stock in real time, and they call only when they have already run out. Rare types (O-,
AB-) have no early warning. With no demand forecast, blood is over-ordered and expires unused.

## Goal of the project

Hospitals request and receive blood in the system instead of by phone, and confirm and record
what they receive and use. The Center sees hospital stock and near-expiry units and is warned
when its own stock of a blood group falls below threshold; warnings on hospital stock follow once
hospital inventory data is reliable (decision log D-18). Hospitals' demand data feeds a validated blood forecast that the
Center uses to plan its supply, so less blood expires unused. Measures agreed with the client are
in [the CVCA](../context/cvca.md) (stakeholder requirements) and, in money terms, in
[W6 cash flow](../function-list/w6-cashflow.md).

Business decisions are recorded in [the decision log](decision-log.md).

## Team and roles

| Member | Role | Owns |
|---|---|---|
| Vu Thanh Nhan | Project manager |  SD for UC-01, UC-02, UC-03, UC-06; Check Function List & Costs: FR-01-01 to FR-01-03-FE, FR-04-01; Project Brief; Function List Scope (w0-scope.md) & Priority Assessment (w4-priority.md); Screens: s-f01-01 (Blood Request), s-f01-02 (Details), s-f04-01 (Sign-in); GitHub documentation preparation and coordination. |
| Nguyen Minh Anh | Quality manager | Check UC Lists: UC-01 to UC-12, Check Function List & Costs: FR-01-03-BE to FR-01-05, FR-04-03, Sequence Diagrams for UC-04, UC-05, UC-07, Screen s-f02-01(blood and usage record) s-f03-03(confirm), W1-function list, screenlist, screen transformation, usage-flow, AD-UC-04 AD-UC-05, Rule Check |
| Dao Bich Hang | Schedule manager | CVCA/ Context diagram,Sequence UC08, UC-09, UC-10, Check Function List & Cost: FR-02-01 to FR-02-05, FR-04-04, Check capital calculations, s-f02-02 (Inventory Dashboard),s-f02-03 (Near Expiry Units), architecture, activity index, w5-handover |
| Phung Duc Minh | Cost Manager | Sequence Diagrams for UC-11, UC-13, UC-14, Function List FR-03-01 to FR-03-03, FR-04-05, Cost & Budgeting, Check UC-List: UC-13 to UC-16, s-f03-02 (Demand Data Review), s-f03-04 (Forecast Review and Approval), ui-conventions, w6-cash-flow, README |
| Le Thanh Nga | TBD | TBD |
