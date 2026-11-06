# Activity Diagram: AD-UC-13 Validate Clinical Demand Data

| Field | Value |
| :---- | :---- |
| AD ID | `AD-UC-13` |
| Use case | `UC-13 Validate Clinical Demand Data` |
| Feature | `F-03 Clinical Demand & Forecasting` |
| Primary actor | NBC Forecasting Analyst |
| Lanes | NBC Forecasting Analyst · NBSP · Hospital Staff |
| Inputs (L3-L5) | Context Diagram (Fig. 2\) · Use Case Diagram (Fig. 3\) · Sequence: `SD-UC-13` (Fig. 16\) · Initial anomaly detection rules (UC list assumption) · W1 rows FR-03-02, FR-03-03 (EARS v3), FR-03-08, FR-04-04 (cross-reference: FR-03-04, FR-04-03) |
| Trigger | The analyst selects a hospital, a date range and a blood group. A second, time-based trigger is the forecast cut-off |
| Preconditions | The analyst is signed in (UC-12) with analyst permissions (FR-04-03). Demand data (usage, transfusion-patient counts, scheduled demand) exists for the selected range, already aggregated by hospital, day and blood group |
| Success end state | Every flagged record has a final status (Accepted or Excluded), so only reviewed data goes on to forecasting. Model-specific preprocessing is not part of this use case (it belongs to the forecast generation, FR-03-05) |
| Failure end state(s) | (a) No record is flagged: nothing to decide. (b) A record is still unresolved at the forecast cut-off: it is excluded from forecasting and flagged |
| FRs covered | `FR-03-02` · `FR-03-03` · `FR-03-08` · `FR-04-04` · cross-reference `FR-03-04` (UC-14) · `FR-04-03` not drawn (precondition, non-countable) |
| Business rules applied | Initial anomaly detection rules from the UC list: Missing Data, Invalid Value, Spike, Inconsistency, Duplicate, Late Entry (no BR IDs yet; final rules are open question 2\) |
| Version / status | v0.7 · Draft (FR-03-08, EARS v3 of FR-03-03 and the FR-03-04 change approved 2026-11-01) |
| Author | Owner of F-03 in W0 (name to confirm) |

## 1\. Diagram

```mermaid
swimlane-beta TB
  subgraph ANA["NBC Forecasting Analyst"]
    start@{ shape: sm-circ }
    a1("Select hospital, date range and blood group")
    a2("Review flagged record and reason")
    d2{"Analyst decision?"}
    a3("Review overdue record")
    d4{"Decision on overdue record?"}
    stop1@{ shape: fr-circ }
  end
  subgraph SYS["NBSP"]
    s1("Display demand data and flag records that violate configured rules, with reason (FR-03-02)")
    d1{"Any flagged record?"}
    m1{" "}
    m2{" "}
    m3{" "}
    m4{" "}
    s3("Store Accepted status and append audit record (FR-03-03, FR-04-04)")
    s4("Store Excluded status and append audit record (FR-03-03, FR-04-04)")
    s5("Store verification request, notify hospital, mark record Pending Verification and append audit record (FR-03-03, FR-04-04)")
    o1["Record [Pending Verification]"]
    d3{"Response recorded by deadline?"}
    s6("Mark request Overdue and notify analyst (FR-03-04)")
    d5{"More flagged records to resolve?"}
    t2@{ shape: hourglass }
    s7("Exclude unresolved records from forecasting, flag them and append audit record (FR-03-08, FR-04-04)")
    x1@{ shape: cross-circ }
  end
  subgraph HOS["Hospital Staff"]
    h1("Handle verification request (AD-UC-14)")
  end
  start --> a1 --> s1 --> d1
  d1 -->|"[flagged records exist]"| m1
  d1 -->|"[no flagged record]"| stop1
  m1 --> a2 --> d2
  d2 -->|"[accept]"| m2
  d2 -->|"[exclude]"| m3
  d2 -->|"[request verification]"| s5
  m2 --> s3 --> m4
  m3 --> s4 --> m4
  m4 --> d5
  d5 -->|"[more flagged records]"| m1
  d5 -->|"[none left]"| stop1
  s5 --> o1 --> h1 --> d3
  d3 -->|"[response recorded]"| m1
  d3 -->|"[deadline missed]"| s6 --> a3 --> d4
  d4 -->|"[accept with note]"| m2
  d4 -->|"[exclude]"| m3
  t2 -->|"forecast cut-off (schedule to be confirmed)"| s7 --> x1
```

&nbsp;

Reading notes:

- The cut-off branch (t2, s7, x1, rule FR-03-08) runs in parallel with the analyst's review and stops only its own branch, hence the flow final.  
- h1 is a call step: the hospital's handling of the request is drawn in AD-UC-14. d3 only tests whether a response was recorded before the deadline.  
- s6 cites FR-03-04, which belongs to UC-14. This cross-reference is accepted: it is the step that tells the analyst a request is overdue.  
- Flagged records are handled one at a time. While a verification request is open, the analyst does not move on to the next record (simplification).  
- "Record" means one row of demand data per hospital, day and blood group.

## 2\. Node trace

| Node | Kind | Text | Traces to |
| :---- | :---- | :---- | :---- |
| s1 | Action | Display demand data and flag records that violate configured rules, with reason | FR-03-02 |
| d1 | Decision | Any flagged record? | FR-03-02 |
| d2 | Decision | Analyst decision? (accept, exclude, request verification) | FR-03-03 |
| s3 | Action | Store Accepted status and append audit record | FR-03-03, FR-04-04 |
| s4 | Action | Store Excluded status and append audit record | FR-03-03, FR-04-04 |
| s5 | Action | Store verification request, notify hospital, mark record Pending Verification, append audit record | FR-03-03, FR-04-04 |
| h1 | Action (hospital lane) | Handle verification request | AD-UC-14 (call step) |
| d3 | Decision | Response recorded by deadline? | FR-03-04 (cross-reference, UC-14; deadline is open question 3\) |
| s6 | Action | Mark request Overdue and notify analyst | FR-03-04 (cross-reference, UC-14; EARS v2) |
| d4 | Decision | Decision on overdue record? (accept with note, exclude) | FR-03-03 (EARS v3) |
| d5 | Decision | More flagged records to resolve? | FR-03-03 (one decision per flagged record; the loop itself is a UC-13 main flow step) |
| t2 | Time event | Forecast cut-off | FR-03-08 |
| s7 | Action | Exclude unresolved records from forecasting, flag them and append audit record | FR-03-08, FR-04-04 |

## 3\. Branches and acceptance tests

| Branch | Given | When | Then |
| :---- | :---- | :---- | :---- |
| d1 \[flagged records exist\] | Hospital A, 01-07 Oct 2026, blood group O+; the record of 03 Oct shows usage \= \-2 | The analyst selects the hospital, range and group | The record is flagged with reason "Invalid Value" and the analyst reviews it |
| d1 \[no flagged record\] | Hospital A, 01-07 Oct 2026, O+; all values are valid whole numbers | The analyst selects the hospital, range and group | No record is flagged and the activity ends |
| d2 \[accept\] | Record of 04 Oct flagged "Spike": usage 40 against a recent level of about 12 | The analyst accepts the record | Status Accepted is stored; an audit record holds actor, timestamp, prior value, final status and note |
| d2 \[exclude\] | Record of 05 Oct flagged "Duplicate" | The analyst excludes the record | Status Excluded is stored and the record is left out of forecasting data; an audit record is appended |
| d2 \[request verification\] | Record of 04 Oct flagged "Spike" | The analyst requests verification from the hospital | The request is stored, the hospital is notified, the record is marked Pending Verification and an audit record is appended |
| d3 \[response recorded\] | A request sent on 10 Oct; deadline 14 Oct; the hospital confirms the value on 12 Oct | The response is recorded before 14 Oct | The analyst reviews the record again (back to merge m1) |
| d3 \[deadline missed\] | A request sent on 10 Oct; deadline 14 Oct; no response | 14 Oct passes | The request is marked Overdue and the analyst is notified, then reviews the overdue record |
| d4 \[accept with note\] | An overdue request for the record of 04 Oct | The analyst accepts it with the note "no hospital response" | The record gets status Accepted with the note; audit record appended |
| d4 \[exclude\] | An overdue request for the record of 04 Oct | The analyst excludes the record | The record gets status Excluded; audit record appended |
| d5 \[more flagged records\] | 3 flagged records, 1 resolved | The decision on the first record is stored | The analyst returns to review the next record |
| d5 \[none left\] | 3 flagged records, all resolved | The last decision is stored | The activity ends |
| t2 → s7 (cut-off) | A record is still Pending Verification | The forecast cut-off time is reached | The record is excluded from forecasting, flagged, and an audit record is appended |

## 4\. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
| :---- | :---- | :---- |
| Record | Pending Verification (drawn). Accepted and Excluded appear in the text of s3, s4, s7 and in SD-UC-13 | Pending Verification, Accepted, Excluded (add to the state table in activity-index.md, spelled identically in UC-13, UC-14 and UC-15) |
| Demand period | Finalised (set by FR-03-08; not drawn) | Finalised (used as trigger of FR-03-05 and UC-15) |
| Request | Overdue (in the text of s6) | Overdue (plus the states of UC-14) |

## 5\. Screens touched

| Actor action | Screen ID |
| :---- | :---- |
|  |  |

## 6\. Open questions

| \# | Question | Blocking? | Default applied |
| :---- | :---- | :---- | :---- |
| 1 | \[NEEDS CLARIFICATION: what schedule defines the forecast cut-off for unresolved demand records?\] To: client | Yes (W5 open question 3\) | Configurable cycle cut-off; unresolved records are excluded as written in FR-03-08 |
| 2 | \[NEEDS CLARIFICATION: which anomaly rules and thresholds are approved for clinical-demand validation?\] To: client | Yes (W5 open question 4\) | Configurable rule engine; thresholds pending analyst and NBC confirmation |
| 3 | \[NEEDS CLARIFICATION: what response deadline does a hospital receive for a verification request?\] To: client | No (W5 open question 2\) | Configurable deadline; overdue logic does not fix the duration in code |
| 4 | \[NEEDS CLARIFICATION: FR-03-03 (Should in W4) lets the analyst request verification, but the hospital can only respond through FR-03-04 (Could). Raising FR-03-04 to Should lowers the Could share to about 17.6%, below the 20% threshold, unless another Should moves down. Decide, or accept that a request is resolved only by the cut-off\] To: integrator and team | No | Diagram keeps the full flow; scope decision pending |
| 5 | \[NEEDS CLARIFICATION: the rule "zero patients transfused but high usage" needs actual transfusion-patient counts, but FR-02-01 and UC-05 capture patients only for planned periodic transfusion. Where do actual counts come from, or is the rule dropped? (The usage date is now in FR-02-01)\] To: integrator and client | No | Assumed available |
| 6 | RESOLVED 2026-11-01: names are those of the UC list (Hospital Staff, NBC Forecasting Analyst); the Context Diagram labels follow them | No | UC list names |
| 7 | RESOLVED 2026-11-01: FR-03-08 (approved) marks the demand data of a period Finalised when no unresolved record remains or the cut-off is reached, and FR-03-05 uses that state as its trigger | No | FR-03-08 |
| 8 | RESOLVED 2026-11-01: EARS v3 of FR-03-03 (analyst decision, overdue decision) and the Overdue notification in FR-03-04 approved | No | FR-03-03 EARS v3, FR-03-04 EARS v2 |

## 7\. Verification log

| Check | Result | By | Date |
| :---- | :---- | :---- | :---- |
| Renders on mermaid.live | To be confirmed by the author |  |  |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass on reading (d1 to d5); confirm after render |  |  |
| No action has two incoming flows (merge used instead) | Pass on reading (m1 to m4; stop1 is a final node, allowed) |  |  |
| Every fork has a matching join | Pass (no fork; the cut-off branch ends in a flow final) |  |  |
| Every system action cites an FR | Pass on reading (s6 cites FR-03-04 as an accepted cross-reference; h1 is a call step to AD-UC-14) |  |  |
| Every FR of this use case appears in the diagram | 4 of 4 (FR-03-02, FR-03-03, FR-03-08, FR-04-04); FR-03-04 is a cross-reference; FR-04-03 is not drawn (precondition) |  |  |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass (UC list names approved) |  |  |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass on reading after SD-UC-13 is updated (loop over records, Overdue, cut-off) |  |  |
| AI critic prompt run, findings resolved or logged as open questions | To be run by the author |  |  |

---

