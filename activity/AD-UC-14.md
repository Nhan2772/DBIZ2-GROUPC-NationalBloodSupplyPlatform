# Activity Diagram: AD-UC-14 Respond to Data Verification Request

| Field | Value |
|---|---|
| AD ID | `AD-UC-14` |
| Use case | `UC-14 Respond to Data Verification Request` (extends UC-13) |
| Feature | `F-03 Clinical Demand & Forecasting` (W0 Table 2) |
| Primary actor | Hospital Staff |
| Lanes | Hospital Staff · NBSP · NBC Forecasting Analyst |
| Inputs | Use Case Diagram · Sequence: `SD-UC-14` · W1 rows FR-03-04, FR-04-04 · Decision log D-20, D-22, D-23 |
| Trigger | (a) Hospital Staff opens a verification request from the link in the verification email, or from the menu (D-20). (b) Time: the configured response deadline passes. |
| Preconditions | Hospital Staff is signed in (UC-06). The Analyst has requested verification (UC-13, FR-03-03). The request belongs to that hospital (FR-04-03). The request is Open. |
| Success end state | The response and change history are saved, the request is Responded and the Analyst is notified. |
| Failure end state(s) | (a) A correction or supplement has no value or note: nothing is saved, the request stays Open. (b) The deadline passes without a response: the request is Overdue and the Analyst is notified. |
| FRs covered | `FR-03-04` · `FR-04-04` |
| Business rules applied | none with a BR ID (deadline is configuration, D-19) |
| Version / status | v1.0 · Draft (18/11/2026: redrawn from W1 FR-03-04 and SD-UC-14; replaces v0.2 drawn from a cut-off screenshot) |

## 1. Diagram

```mermaid
swimlane-beta TB
  subgraph HS["Hospital Staff"]
    start@{ shape: sm-circ }
    a1("Open the verification request")
    a2("Review the flagged record, reason and analyst note")
    d1{"Response type?"}
    a3("Confirm the data is correct")
    a4("Enter the corrected value and a note")
    a5("Enter additional information")
    m2{" "}
  end
  subgraph SYS["NBSP"]
    m1{" "}
    s0("Show the flagged record, reason and analyst note (FR-03-04)")
    d2{"Response complete?"}
    s1("Show what is missing; request stays Open (FR-03-04)")
    s2("Save the response and change history; set the request Responded (FR-03-04, FR-04-04)")
    o1["Verification request [Responded]"]
    s3("Notify the Analyst (FR-03-04, D-20)")
    t1@{ shape: hourglass }
    s4("Set the request Overdue and notify the Analyst (FR-03-04)")
    o2["Verification request [Overdue]"]
    stop1@{ shape: fr-circ }
    x1@{ shape: cross-circ }
  end
  subgraph ANA["NBC Forecasting Analyst"]
    n1("Review the response in UC-13")
    n2("Decide on the overdue record in UC-13")
  end
  start --> a1 --> s0 --> a2 --> m1 --> d1
  d1 -->|"[confirm]"| a3 --> m2
  d1 -->|"[correct]"| a4 --> m2
  d1 -->|"[supplement]"| a5 --> m2
  m2 --> d2
  d2 -->|"[incomplete]"| s1 --> m1
  d2 -->|"[complete]"| s2 --> o1 --> s3 --> n1 --> stop1
  t1 -->|"deadline passed, request still Open"| s4 --> o2 --> n2 --> x1
```

Reading notes:

- The deadline branch (t1) is a separate time event; it only fires while the request is still Open, and it ends its own branch (flow final).
- One request concerns one flagged record (UC-13 handles records one at a time).
- After a correction, the Analyst still decides in UC-13 whether to accept or exclude the record; UC-14 does not change the record status.

## 2. Node trace

| Node | Kind | Text | Traces to |
|---|---|---|---|
| s0 | Action | Show the flagged record, reason and analyst note | FR-03-04 |
| d1 | Decision (hospital) | Response type? | FR-03-04 (confirm, correct, supplement) |
| d2 | Decision | Response complete? | FR-03-04 |
| s1 | Action | Show what is missing; request stays Open | FR-03-04 |
| s2 | Action | Save the response and change history; set Responded | FR-03-04, FR-04-04 |
| s3 | Action | Notify the Analyst | FR-03-04, D-20 |
| t1 | Time event | Deadline passed | FR-03-04 (IF clause) |
| s4 | Action | Set the request Overdue and notify the Analyst | FR-03-04 |

## 3. Branches and acceptance tests

| Branch | Given | When | Then |
|---|---|---|---|
| d1 [confirm] | Request Open for the 04 Oct O+ record flagged "Spike" | Staff confirms the value is correct | Request Responded; history saved; Analyst notified |
| d1 [correct] | Same request | Staff enters 14 instead of 40 with note "typing error" | Corrected value and history saved; request Responded; Analyst notified |
| d1 [supplement] | Same request | Staff adds "mass-casualty case on 04 Oct" | Information saved; request Responded; Analyst notified |
| d2 [incomplete] | Same request | Staff chooses Correct but leaves the value empty | Missing value shown; nothing saved; request stays Open |
| t1 | Request sent 10 Oct, deadline 14 Oct, no response | 14 Oct passes | Request Overdue; Analyst notified |

## 4. Object states used

| Object | States in this diagram | Full state list |
|---|---|---|
| Verification request | Open, Responded, Overdue | Open, Responded, Overdue (decision log D-23) |

## 5. Screens touched

| Actor action | Screen ID |
|---|---|
| Open, review and respond | S-F03-03 |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | What response deadline does the hospital receive? | No (W5 Q2) | Configurable (D-19) |
| 2 | Can the hospital still respond after the request is Overdue? | No | No: the Analyst decides in UC-13 |
