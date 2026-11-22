# Activity Diagram: AD-UC-01 Request Blood Provision

| Field | Value |
|---|---|
| AD ID | `AD-UC-01` |
| Use case | `UC-01 Request Blood Provision` |
| Feature | `F-01 Blood Ordering & Fulfillment` (W0 Table 2) |
| Primary actor | Hospital Staff |
| Lanes | Hospital Staff · NBSP (name matches Context Diagram v4.1) · no external systems |
| Inputs (L3-L5) | Context Diagram v4.1 · Use Case Diagram · Sequence: SD-UC-01 (v2) · Architecture/ADR: not used · W1 rows: FR-01-01, FR-01-06 · W0 BR-01, BR-10 |
| Trigger | Hospital Staff submits a blood type, blood product, quantity, urgency, needed-by date and time and a contact person (name and phone), with an optional reason (FR-01-01; BR-01, BR-10). |
| Preconditions | Hospital Staff is signed in (assumed; sign-in is UC-06 / FR-04-01, not stated in FR-01-01). |
| Success end state | A Pending blood request exists and is in the NBC request queue. |
| Failure end state(s) | None in this flow: invalid input is rejected with reasons and the flow returns to validation until the input is valid. Abandoning the request is not modeled (no FR covers it). |
| FRs covered | `FR-01-01` · `FR-01-06` |
| Business rules applied | BR-01 (valid blood type, blood product, urgency, quantity), BR-10 (needed-by date and time, contact name and phone, optional reason) |
| Version / status | v0.4 · Draft (21/11/2026: fields and rules aligned with W1 FR-01-01 / FR-01-06, BR-01, BR-10 and SD-UC-01 v2; to be reviewed) |
| Author | Nhàn |

## 1. Diagram

```mermaid
swimlane-beta TB
  subgraph HS["Hospital Staff"]
    start@{ shape: sm-circ }
    a1("Submit blood request with blood type, product, quantity, urgency, needed-by time and contact")
    a2("Correct the invalid fields and resubmit")
  end
  subgraph SYS["NBSP"]
    m1{" "}
    s1("Validate request input (FR-01-01)")
    d1{"Input valid?"}
    s4("Reject request and show the reason for each invalid field (FR-01-06)")
    s2("Create Pending blood request (FR-01-01)")
    o1["Blood request [Pending]"]
    s3("Place request in NBC request queue (FR-01-01)")
    stop1@{ shape: fr-circ }
  end
  start --> a1 --> m1 --> s1 --> d1
  d1 -->|"[input valid]"| s2 --> o1 --> s3 --> stop1
  d1 -->|"[input invalid]"| s4 --> a2 --> m1
```

Reading notes:

- The invalid-input branch is approved as FR-01-06 (W1). The loop is drawn as option B of Q1.
- `m1` is a merge. The first submission and every resubmission enter validation through it, so the loop returns through a merge and `s1` has one incoming flow.
- Valid values are defined by BR-01 (blood type, product, urgency, quantity) and BR-10 (needed-by, contact, reason), so `d1` traces to both.

## 2. Node trace

| Node | Kind | Text | Traces to |
|---|---|---|---|
| m1 | Merge | (first submission or resubmission) | none (control node) |
| s1 | Action | Validate request input | FR-01-01; BR-01, BR-10 |
| d1 | Decision | Input valid? | FR-01-01, FR-01-06; BR-01, BR-10 |
| s4 | Action | Reject request and show the reason for each invalid field | FR-01-06 |
| s2 | Action | Create Pending blood request | FR-01-01 |
| o1 | Object node | Blood request [Pending] | FR-01-01 |
| s3 | Action | Place request in NBC request queue | FR-01-01 |

## 3. Branches and acceptance tests

Values in these rows are examples that follow BR-01 and BR-10.

| Branch | Given | When | Then |
|---|---|---|---|
| d1 [input valid] | Hospital Staff of one hospital is signed in | Staff submits blood type O-, product red blood cells, quantity 4, urgency Urgent, needed by 30/11/2026 12:00, contact Nguyễn Văn A, phone 0123 456 789 | One request with status Pending exists for O- x 4, Urgent, and it appears in the NBC request queue |
| d1 [input invalid] | Hospital Staff is signed in | Staff submits the same values with quantity 0 and an empty contact phone, then corrects the quantity to 4, enters the phone and resubmits | After the first submission no request exists and a reason is shown for the quantity field and for the phone field, with the other values kept (FR-01-06). After the resubmission one Pending request exists for O- x 4, Urgent |
| d1 [input invalid] (BR-10) | Hospital Staff is signed in | Staff submits a needed-by date and time in the past, all other values valid | No request is created and the reason for the needed-by field is shown |

## 4. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
|---|---|---|
| Blood request | Pending | Pending, Dispatched, Not approved, Received (W0 BR-02; no separate Approved state) |

## 5. Screens touched

| Actor action | Screen ID |
|---|---|
| Submit blood request | S-F01-01 |
| Correct the invalid fields and resubmit | S-F01-01 (error state) |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: Is there a maximum quantity per request, and are duplicate requests blocked? SD-UC-01 v2 mentions a maximum under BR-10, but BR-10 in W0 has none (S-F01-01 open question 9)] To: client / integrator | No | No maximum and no duplicate check |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders on mermaid.live | | | |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass | | |
| No action has two incoming flows (merge used instead) | Pass | | |
| Every fork has a matching join | n/a (no forks) | | |
| Every system action cites an FR | Pass (s4 cites FR-01-06) | | |
| Every FR of this use case appears in the diagram | 2 of 2 (FR-01-01, FR-01-06) | | |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass on reading (Hospital Staff, NBSP; Context Diagram v4.1) | | |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass on reading (SD-UC-01 v2: alt "valid request" / "invalid data" = d1) | | |
| AI critic prompt run, findings resolved or logged as open questions | | | |
