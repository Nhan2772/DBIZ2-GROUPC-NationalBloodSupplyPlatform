# Activity Diagram: AD-UC-08 Approve & Dispatch Blood Order
 
| Field | Value |
|---|---|
| AD ID | `AD-UC-08` |
| Use case | `UC-08 Approve & Dispatch Blood Order` |
| Feature | `F-01 Blood Ordering & Fulfillment` (W0 Table 2) |
| Primary actor | NBC Blood Storage and Distribution Department |
| Lanes | NBC Blood Storage and Distribution Department · NBSP · Blood Management Software (external system, Context Diagram) |
| Inputs (L3-L5) | Context Diagram (Fig. 2, API sync with the Blood Management Software) · Use Case Diagram (Fig. 3) · Sequence: `SD-UC-08` (v3) · Architecture/ADR: not used · Assumption "BMS API" · W1 row FR-01-03 |
| Trigger | The Dept staff opens a pending blood request from the request queue (UC-07). |
| Preconditions | The Dept staff is signed in (UC-12). The request has status Pending. |
| Success end state | The request is Dispatched, the selected units are reserved in the Blood Management Software, and the shipment details (units, expiry dates, ETA) exist for the hospital. |
| Failure end state(s) | (a) Available units are fewer than the requested quantity: dispatch is blocked, nothing changes, the request stays Pending. (b) The Blood Management Software is unavailable or returns an error: approval is blocked, nothing is reserved, the request stays Pending. (c) NBSP cannot save the shipment after the units were reserved: the reservation is cancelled, no shipment exists, the request stays Pending. |
| FRs covered | `FR-01-03` · `FR-01-08` · `FR-01-09` |
| Business rules applied | BR-02, BR-04, BR-09. No partial dispatch (D-05); Not approved with a reason is allowed (D-04, supersedes the 2026-11-06 decision); one attempt, timeout 10 s, idempotency key, release on save failure (D-09) |
| Version / status | v0.6 · Draft (21/11/2026: added the "shipment not saved, reservation cancelled" branch, W1 FR-01-03 clause; screens section aligned with S-F01-04 v2.1) |
 
## 1. Diagram
 
```mermaid
swimlane-beta TB
  subgraph DEP["NBC Blood Storage and Distribution Department"]
    start@{ shape: sm-circ }
    a1("Open a pending blood request")
    a2("Select available units, enter the ETA and approve and dispatch")
    a3("See dispatch confirmed")
    d0{"Dispatch or not approve?"}
    a4("Choose the reason and confirm Not approved")
  end
  subgraph SYS["NBSP"]
    s1("Show the request and the available units (FR-01-03)")
    d1{"Enough available units for the requested quantity?"}
    s2("Report stock shortage and block dispatch (FR-01-08)")
    m1{" "}
    s3("Revalidate request status and unit availability (FR-01-03)")
    d2{"Request still Pending and units still available?"}
    s4("Reject the submission and show the reason (FR-01-03)")
    s5("Ask the Blood Management Software to reserve the units (FR-01-03)")
    d3{"Reservation succeeded?"}
    s6("Block approval and show that the stock service is unavailable (FR-01-03)")
    s7("Save the shipment details (units, expiry dates, ETA) and set the request to Dispatched (FR-01-03)")
    d4{"Shipment saved?"}
    o1["Blood request [Dispatched]"]
    s9("Ask the Blood Management Software to cancel the reservation (FR-01-03)")
    s10("Keep the request Pending and show that the shipment was not saved (FR-01-03)")
    s8("Set the request to Not approved and store the reason (FR-01-09)")
    o2["Blood request [Not approved]"]
    stop1@{ shape: fr-circ }
    stop2@{ shape: fr-circ }
  end
  subgraph BMS["Blood Management Software"]
    b1("Reserve the units or return an error (FR-01-03)")
    b2("Release the reserved units (FR-01-03)")
  end
  start --> a1 --> s1 --> d0
  d0 -->|"[not approve]"| a4 --> s8 --> o2 --> stop1
  d0 -->|"[dispatch]"| d1
  d1 -->|"[not enough units]"| s2 --> stop2
  d1 -->|"[enough units]"| m1 --> a2 --> s3 --> d2
  d2 -->|"[no longer valid]"| s4 --> m1
  d2 -->|"[valid]"| s5 --> b1 --> d3
  d3 -->|"[units refused: no longer available]"| s4
  d3 -->|"[service unavailable or error]"| s6 --> stop2
  d3 -->|"[reserved]"| s7 --> d4
  d4 -->|"[saved]"| o1 --> a3 --> stop1
  d4 -->|"[save failed]"| s9 --> b2 --> s10 --> stop2
```
 
Reading notes:
 
- Approve, unit selection, ETA and dispatch are **one action** (a2), as in FR-01-03. There is no separate "Approved" state: the request goes from Pending to Dispatched.
- **New in v0.6 (d4, s9, b2, s10):** W1 FR-01-03 says that if NBSP fails to save the shipment after the units were reserved, NBSP cancels the reservation. SD-UC-08 already had this ("Save failed"); the AD now draws it. The request stays Pending and no unit stays held in the Blood Management Software.
- The IF clauses of FR-01-03 for the shortage (s2), the request no longer Pending or unit no longer available (s4), the BMS unavailable (s6) and the save failure (s9) are all in W1 (checked 21/11/2026).
- Two different BMS outcomes: a BMS that does not answer within the timeout or returns an error (d3 `[service unavailable or error]`, s6) and a BMS that refuses because a unit is no longer available. The refusal goes back to s4 (the Dept reselects, same message as "unit taken" on S-F01-04); the timeout or error goes to s6. SD-UC-08 draws both as separate branches.
- No partial dispatch (D-05): the selected units must cover the requested quantity.
- Not approved (D-04): the Dept can close a Pending request with a reason (No eligible stock, Request data incorrect, Cancelled by hospital, Other). The hospital sees the reason on S-F01-02. After a shortage (s2) the request stays Pending; the Dept can reopen it and choose Not approved.
- `m1` is a merge: the first selection and every reselection enter through it. Two flows enter `stop2` (and several end in it): allowed for a final node.
- After s6 or s10 the Dept can try again from the same screen (the screen keeps the selection and the ETA). This is a new submission and is not drawn as a loop.
- The confirmation dialog on the screen is a screen detail and is not drawn here.
- Cost: no new L3 item. FR-01-03-BE ("revalidate request; persist shipment") and FR-01-03-INT ("reserve units through the BMS API and handle timeout") already cover the release of the reservation.
## 2. Node trace
 
| Node | Kind | Text | Traces to |
|---|---|---|---|
| s1 | Action | Show the request and the available units | FR-01-03 |
| d0 | Decision (Dept lane) | Dispatch or not approve? | FR-01-09 |
| d1 | Decision | Enough available units for the requested quantity? | FR-01-08 |
| s2 | Action | Report stock shortage and block dispatch | FR-01-08 |
| s3 | Action | Revalidate request status and unit availability | FR-01-03 (L3 FR-01-03-BE) |
| d2 | Decision | Request still Pending and units still available? | FR-01-03 |
| s4 | Action | Reject the submission and show the reason | FR-01-03 |
| s5 | Action | Ask the Blood Management Software to reserve the units | FR-01-03 (L3 FR-01-03-INT) |
| b1 | Action (BMS lane) | Reserve the units or return an error | FR-01-03 |
| d3 | Decision | Reservation succeeded? | FR-01-03 (IF external stock service unavailable; IF a selected unit is no longer available) |
| s6 | Action | Block approval and show that the stock service is unavailable | FR-01-03 |
| s7 | Action | Save the shipment details and set the request to Dispatched | FR-01-03 |
| d4 | Decision | Shipment saved? | FR-01-03 (IF NBSP fails to save the shipment) |
| o1 | Object node | Blood request [Dispatched] | FR-01-03 |
| s9 | Action | Ask the Blood Management Software to cancel the reservation | FR-01-03 |
| b2 | Action (BMS lane) | Release the reserved units | FR-01-03 |
| s10 | Action | Keep the request Pending and show that the shipment was not saved | FR-01-03 |
| s8 | Action | Set the request to Not approved and store the reason | FR-01-09 |
| o2 | Object node | Blood request [Not approved] | FR-01-09 |
 
## 3. Branches and acceptance tests
 
Values are examples.
 
| Branch | Given | When | Then |
|---|---|---|---|
| d0 [not approve] | Request O- x 4, Pending; the hospital has phoned to cancel | The Dept chooses Not approved with reason "Cancelled by hospital" | The request is Not approved, the reason is stored and shown to the hospital on S-F01-02 |
| d1 [enough units] | Request O- x 4, Urgent, Pending; NBC has 10 available O- units | The Dept staff opens the request | The 10 available units are shown and the staff can select |
| d1 [not enough units] | Request O- x 4, Urgent, Pending; NBC has 3 available O- units | The Dept staff opens the request | The shortage is reported, dispatch is blocked, the request stays Pending |
| d2 [valid] | Request is Pending; the 4 selected units are available | The staff submits with 4 units and ETA 08/11/2026 15:00 | The submission goes on to the reservation |
| d2 [no longer valid] | One of the 4 selected units was dispatched to another request a minute ago | The staff submits | The submission is rejected with the reason, nothing is reserved, the staff reselects |
| d3 [reserved] | The Blood Management Software accepts the reservation | The reservation is requested | NBSP goes on to save the shipment (d4) |
| d3 [units refused] | The Blood Management Software answers that one selected unit is no longer available | The reservation is requested | Nothing is reserved, the request stays Pending, the staff sees the reason and reselects |
| d3 [service unavailable or error] | The Blood Management Software does not answer within 10 s | The reservation is requested | Approval is blocked, nothing is reserved, the request stays Pending, the staff sees that the stock service is unavailable |
| d4 [saved] | The 4 units are reserved and the database is available | NBSP saves the shipment | Shipment details (4 units, expiry dates, ETA) exist, the request is Dispatched, the staff sees the confirmation |
| d4 [save failed] | The 4 units are reserved but the database returns an error on saving | NBSP saves the shipment | The reservation is cancelled in the Blood Management Software, no shipment exists, the request stays Pending, the staff sees that the shipment was not saved and can try again |
 
## 4. Object states used
 
| Object | States in this diagram | Full state list (all diagrams) |
|---|---|---|
| Blood request | Pending (precondition), Dispatched, Not approved | Pending, Dispatched, Received, Not approved (decision log D-23). No Approved state: approval and dispatch are one action. Object name "Blood request" as in AD-UC-01 and AD-UC-03 |
 
## 5. Screens touched
 
| Actor action | Screen ID |
|---|---|
| Open a pending blood request | S-F01-03 → S-F01-04 |
| Select available units, enter the ETA and approve and dispatch | S-F01-04 (confirmation dialog "Confirm approve & dispatch"; error states for no longer valid, stock shortage, BMS not answering, shipment not saved) |
| See dispatch confirmed | S-F01-04, success state (the user stays on the screen and leaves with "Quay lại hàng đợi" to S-F01-03) |
| Choose the reason and confirm Not approved | S-F01-04 (modal "Not approve request"), then the success state "Not approved" |
 
## 6. Open questions
 
| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | RESOLVED 21/11/2026: the IF clauses of FR-01-03 (shortage, no longer valid, BMS unavailable, save failure with cancellation of the reservation) are in W1 | No | Drawn as in W1 |
| 2 | RESOLVED 18/11 (D-09): one attempt, timeout 10 s (configurable), idempotency key = request ID, release the reservation if NBSP cannot save | No | As decided |
| 3 | [NEEDS CLARIFICATION: is the hospital notified when the request is Dispatched, or does it only see the status (UC-02)?] To: team | No | RESOLVED D-11: the hospital sees the status and shipment details in UC-02; no notification drawn |
| 4 | [NEEDS CLARIFICATION: ETA rules (not in the past?) and whether the staff must select exactly the requested quantity] To: client | No | ETA must be in the future; selected units = requested quantity |
| 5 | [NEEDS CLARIFICATION: FR-01-04 and UC-02 say "approved or dispatched request"; with one action there is no Approved state] To: integrator | No | Treat as Dispatched only; FR-01-04 should add "or Not approved" with the reason |
| 6 | [NEEDS CLARIFICATION: what if the release of the reservation (b2) itself fails after a save failure? Is the failure logged for manual release by the NBSP administrator?] To: team | No | Not drawn; logged and released manually (assumption) |
 
## 7. Verification log
 
| Check | Result | By | Date |
|---|---|---|---|
| Renders on mermaid.live | To be confirmed by the author | | |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass on reading (d0 to d4; d3 has three guards) | | |
| No action has two incoming flows (merge used instead) | Pass on reading (m1 merges d1 and s4; stop1 and stop2 are final nodes) | | |
| Every fork has a matching join | n/a (no forks) | | |
| Every system action cites an FR | Pass (s2 FR-01-08; s4, s6, s9, s10 cite the IF clauses of FR-01-03 now in W1) | | |
| Every FR of this use case appears in the diagram | 3 of 3 (FR-01-03, FR-01-08, FR-01-09) | | |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass (Dept, NBSP, Blood Management Software) | | |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass on reading with SD-UC-08 v3: "Not approve" = d0, "not Pending" = d2, "insufficient units" = d1/d2, "BMS not answering" = d3 [service unavailable or error], "reservation refused" = d3 [units refused], "Save failed" = d4 | | |
| AI critic prompt run, findings resolved or logged as open questions | Not run yet | | |
 
