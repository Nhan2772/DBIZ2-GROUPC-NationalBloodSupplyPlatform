# Activity Diagram: AD-UC-08 — Approve & Dispatch Blood Order

> **v0.2 — Condensed workshop draft (14 nodes).** Based on the UC-08 inventory screenshot, the course's Mermaid activity profile and the historic FR-01-03 reference. The exact EARS wording, approval/shortage rules and official object states need cross-checking against the team's latest W0/W1.

| Field | Value |
|---|---|
| AD ID | AD-UC-08 |
| Use case | UC-08 — Approve & Dispatch Blood Order |
| Feature | F-01 |
| Primary actor | NBC Blood storage & Distribution |
| Lanes | NBC Blood storage & Distribution · NBSP System |
| Inputs | UC inventory screenshot; DBIZ2 historical FR-01-03 worked example; Lesson 7 workshop guide; Mermaid activity patterns; LabDesk worked example |
| Trigger | NBC begins reviewing a pending blood order (to confirm in W0) |
| Preconditions | A pending blood order exists (to confirm in W0) |
| Success end state | Dispatch is recorded for the approved order; `Blood Order [Dispatched]` is a provisional state name |
| Alternative end states | Stock shortage or non-approval; no dispatch recorded |
| FRs covered | FR-01-03 — **provisional mapping; check exact EARS text in W1** |
| Business rules applied | Stock adequacy and approval rules — require confirmation from W1 |
| Version/status | v0.2 · Draft, 14 nodes |

## 1. Diagram

```mermaid
swimlane-beta TB
  subgraph NBC["NBC Blood storage & Distribution"]
    start@{ shape: sm-circ }
    a1("Review pending blood order")
    d2{"Approve blood order?"}
    a2("Select blood units")
    a3("Confirm dispatch")
    stop1@{ shape: fr-circ }
    stop2@{ shape: fr-circ }
  end
  subgraph NBSP["NBSP System"]
    s1("Check order and available stock (FR-01-03)")
    d1{"Sufficient eligible stock?"}
    s2("Report stock shortage (FR-01-03)")
    s3("Record non-approval (FR-01-03)")
    s4("Record approval and allocate units (FR-01-03)")
    s5("Record dispatch and order status (FR-01-03)")
    o1["Blood Order [Dispatched]"]
  end
  start --> a1 --> s1 --> d1
  d1 -->|"[no]"| s2 --> stop2
  d1 -->|"[yes]"| d2
  d2 -->|"[no]"| s3 --> stop2
  d2 -->|"[yes]"| a2 --> s4 --> a3 --> s5 --> o1 --> stop1
```

**Description (English).** The UC-08 Activity Diagram presents a streamlined process for reviewing and dispatching a blood order. NBC Blood Storage & Distribution checks the order, decides whether to approve it, selects blood units, and confirms dispatch. NBSP checks availability and records approval and dispatch, while insufficient stock or non-approval ends without dispatch.

## 2. Node trace

| Node | Kind | Meaning | Trace |
|---|---|---|---|
| s1 | Action | Check order and available stock | FR-01-03 — provisional |
| d1 | Decision | Stock sufficient and eligible? | Business rule not supplied; needs confirmation |
| s2 | Action | Report shortage | FR-01-03 — provisional |
| d2 | Decision | Approve blood order? | UC-08 approval goal; acceptance criteria not supplied |
| s3 | Action | Record non-approval | FR-01-03 — provisional |
| s4 | Action | Record approval and allocation | FR-01-03 — provisional |
| s5 | Action | Record dispatch and order status | FR-01-03 — historical scope of function |
| o1 | Object | Blood Order [Dispatched] | Confirm exact status in state vocabulary |

## 3. Branches and acceptance tests

| Branch | Given | When | Then |
|---|---|---|---|
| d1 [no] | An order requires more eligible units than are available | NBC reviews the order | NBSP reports insufficient stock; dispatch does not proceed (policy needs confirmation) |
| d1 [yes] | Eligible available stock meets the requested quantity | NBC reviews the order | NBC can decide whether to approve the order |
| d2 [no] | Eligible stock is sufficient, but NBC does not approve | NBC makes its decision | Non-approval is recorded and no dispatch is made (policy needs confirmation) |
| d2 [yes] | Eligible stock is sufficient and NBC approves | NBC selects units and confirms dispatch | Approval, allocation and dispatch are recorded |

## 4. Object states used

| Object | State used | Note |
|---|---|---|
| Blood Order | Dispatched | Provisional — must match the exact official status spelling in the team's diagrams/W1 |

## 5. Screens touched

| Actor actions | Screen ID |
|---|---|
| Review order; approve; select units; confirm dispatch | Fill in during Lesson 8 from the Screen List |

## 6. Open questions

| # | Question | Blocking? | Draft treatment |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: Does W1 prohibit dispatch if stock is insufficient, or allow partial fulfillment?] | Yes | Treat insufficient stock as an alternative end without dispatch |
| 2 | [NEEDS CLARIFICATION: Is non-approval supported and recorded? If yes, what is its official state?] | Yes | Record non-approval but do not invent a rejection status |
| 3 | [NEEDS CLARIFICATION: Is `Dispatched` the official Blood Order state?] | Yes | Use provisional state label |
| 4 | [NEEDS CLARIFICATION: Which specific W1 FR/BR rows govern stock checking, allocation, and dispatch?] | Yes | Temporarily map system actions to FR-01-03 |

## 7. Verification log

| Check | Result |
|---|---|
| 10–15 nodes | Pass — 14 nodes including start/end |
| One UC, two known lanes | Pass |
| Both decisions guarded; guards cover yes/no | Pass |
| Two incoming flows to activity final | Allowed by the course profile |
| Merge required? | No return loop or reconverging action |
| Fork/join required? | No parallel paths |
| Time event required? | No time trigger provided |
| System actions have FR IDs | Present, but exact W1 linkage not independently verified |
| Official object-state spelling | Pending verification |
| Rendered in mermaid.live | Not independently verified in this environment |
| Peer review | Pending |
