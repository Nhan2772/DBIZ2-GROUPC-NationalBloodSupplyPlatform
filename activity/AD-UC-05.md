# Activity Diagram: AD-UC-05 Log Surgical Schedules with Blood Demand

| Field | Value |
|---|---|
| AD ID | `AD-UC-05` |
| Use case | `UC-05 Log Surgical Schedules with Blood Demand` |
| Feature | `<F-ID> <feature name>` [NEEDS CLARIFICATION: copy from W0 Table 2] |
| Primary actor | Hospital Staff |
| Lanes | Hospital Staff · Hospital Blood Mgmt Portal |
| Inputs (L3-L5) | Context Diagram v<n> · Use Case Diagram v<n> · Sequence: `SD-UC-05` · Architecture/ADR used · W1 rows |
| Trigger | Hospital Staff decides to log a surgical schedule or a periodic transfusion plan with its blood demand |
| Preconditions | Hospital Staff is logged in with a hospital account (UC-06) |
| Success end state | The surgical schedule / transfusion plan is saved as an input for the blood forecast, and Hospital Staff has seen a confirmation |
| Failure end state(s) | None in the current flow: invalid data loops back to re-entry until the data is valid (see Open question 2) |
| FRs covered | `FR-05-xx` [NEEDS CLARIFICATION: list from W1] |
| Business rules applied | `BR-xx` [NEEDS CLARIFICATION: validation rules for date, blood type, units, patients, cycle] |
| Version / status | v0.1 · Draft |
| Author | <member> |

## 1. Diagram

```mermaid
swimlane-beta TB
  subgraph STF["Hospital Staff"]
    start@{ shape: sm-circ }
    a1("Select plan type")
    a2("Enter plan details")
    a3("Correct plan details")
    a4("See confirmation")
    stop1@{ shape: fr-circ }
  end
  subgraph SYS["Hospital Blood Mgmt Portal"]
    m1{" "}
    s1("Validate input data (FR-05-xx)")
    q1@{ shape: text, label: "Data valid?" }
    d1{" "}
    s2("Show error and request re-entry (FR-05-xx)")
    s3("Save plan as forecasting input (FR-05-xx)")
    o1["Plan [Saved]"]
    s4("Show confirmation (FR-05-xx)")
  end
  start --> a1 --> a2 --> m1 --> s1 --> d1
  q1 ~~~ d1
  d1 -->|"[no]"| s2 --> a3 --> m1
  d1 -->|"[yes]"| s3 --> o1 --> s4 --> a4 --> stop1
```

Reading notes:

- The decision question "Data valid?" is a separate text node (`q1`) placed next to the empty diamond `d1`, as in the course worked example. If `q1` is drawn far from the diamond, move the `q1 ~~~ d1` line or put the question text on the flow into `d1`.
- Plan type is either **scheduled surgery** (date, blood type, units) or **periodic transfusion** (date, blood type, units, number of patients, cycle). Both follow the same flow, so they are one action "Enter plan details" and not two branches.
- In `SD-UC-05` the confirmation message (step 4) is drawn before the save (step 5). Here the confirmation comes after the save, so Hospital Staff is never told "saved" before it is true. Align the sequence diagram or log it as an open question.
- Step 8 of the sequence diagram, "Show updated plan list (optional)", is left out of the diagram because it is optional (Open question 3).
- The Hospital Database and the Forecasting Engine are not lanes: the database is inside the system, and the Forecasting Engine has no message in `SD-UC-05`.

## 2. Node trace

| Node | Kind | Text | Traces to |
|---|---|---|---|
| s1 | Action | Validate input data | FR-05-xx |
| d1 | Decision | Data valid? (question drawn outside the diamond as text node q1) | FR-05-xx / BR-xx |
| s2 | Action | Show error and request re-entry | FR-05-xx |
| s3 | Action | Save plan as forecasting input | FR-05-xx |
| s4 | Action | Show confirmation | FR-05-xx |

## 3. Branches and acceptance tests

| Branch | Given | When | Then |
|---|---|---|---|
| d1 [yes] | Hospital Staff chose "scheduled surgery" and entered date 2026-11-03, blood type O+, 4 units | The portal validates the input | The plan is saved as a forecasting input, the plan is in state Saved, and the confirmation is shown |
| d1 [yes] | Hospital Staff chose "periodic transfusion" and entered 12 patients, cycle 14 days, blood type A+, 2 units | The portal validates the input | The plan is saved as a forecasting input, the plan is in state Saved, and the confirmation is shown |
| d1 [no] | Hospital Staff chose "scheduled surgery" and left the date empty and entered units = 0 | The portal validates the input | Nothing is saved, an error is shown, and Hospital Staff is asked to re-enter the details |

## 4. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
|---|---|---|
| Surgical schedule / transfusion plan | Saved | [NEEDS CLARIFICATION: e.g. Saved, Updated, Cancelled, from the data model] |

## 5. Screens touched

| Actor action | Screen ID |
|---|---|
| | |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: FR IDs, BR IDs, Feature ID and diagram versions are not in the file; the FR-05-xx placeholders must be replaced from W1] | Yes | Placeholders kept |
| 2 | [NEEDS CLARIFICATION: can Hospital Staff cancel instead of re-entering after an error? The sequence diagram has no cancel] | No | No cancel; loop until valid |
| 3 | [NEEDS CLARIFICATION: is "Show updated plan list" (step 8) part of the use case or a screen detail for Lesson 8?] | No | Left out of the diagram |
| 4 | [NEEDS CLARIFICATION: lane name. The sequence diagram uses "Hospital Blood Mgmt Portal"; confirm it matches the system name on the Context Diagram] | No | Name from `SD-UC-05` |
| 5 | [NEEDS CLARIFICATION: validation rules (past dates, maximum units, allowed cycle values)] | No | Not stated in the diagram |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders on mermaid.live | Not yet run | | |
| Every decision has guards on all outgoing flows, no overlap, no gap | d1: [yes] / [no] | | |
| No action has two incoming flows (merge used instead) | m1 merges a2 and s2/a3 | | |
| Every fork has a matching join | No fork used | | |
| Every system action cites an FR | FR IDs pending (placeholders) | | |
| Every FR of this use case appears in the diagram | n of n pending | | |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pending Context Diagram check | | |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | alt [Valid data] / [Invalid data] = d1 | | |
| AI critic prompt run, findings resolved or logged as open questions | Not yet run | | |

---

Template source: DBIZ2 Lesson 7, VJCBI College - FTU. Notation: OMG UML 2.5.1, Clause 15.
