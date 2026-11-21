# Activity Diagram: AD-UC-15 Review and Approve Blood Forecast

| Field | Value |
|---|---|
| AD ID | `AD-UC-15` |
| Use case | `UC-15 Review and Approve Blood Forecast` |
| Feature | `F-03 Clinical Demand & Forecasting` (W0 Table 2) |
| Primary actor | NBC Forecasting Analyst |
| Lanes | NBSP · NBC Forecasting Analyst |
| Inputs | Sequence: `SD-UC-15` · W1 rows FR-03-05, FR-03-06, FR-04-04 · Decision log D-21 |
| Trigger | The demand data of a period is Finalised (FR-03-08); NBSP generates the draft (FR-03-05) and notifies the Analyst. |
| Preconditions | The Analyst is signed in (UC-12). An active deployed forecast model exists. |
| Success end state | Approved items are published as a new version; rejected items are not published and keep their reason; undecided items stay Draft. |
| Failure end state(s) | No item approved: nothing is published. |
| FRs covered | `FR-03-05` · `FR-03-06` · `FR-04-04` |
| Business rules applied | BR-07 (only approved forecasts are published) |
| Version / status | v1.0 · Draft (18/11/2026: per-item review as in W1 FR-03-06 and SD-UC-15; replaces the batch-level v0.1) |

## 1. Diagram

```mermaid
swimlane-beta TB
  subgraph SYS["NBSP"]
    start@{ shape: sm-circ }
    s0("Run the active model on accepted demand data and NBC allocation history; create Draft items NBC-wide and per hospital; list skipped hospitals (FR-03-05)")
    s1("Notify the Analyst that the draft batch is ready (FR-03-05, D-20)")
    s2("Show Normal and Needs Review groups, flag reasons and previous accuracy (FR-03-06)")
    m1{" "}
    s3("Store approval, adjusted value or rejection with reason, reviewer and time (FR-03-06, FR-04-04)")
    s4("Publish approved items as a new version; keep earlier versions; exclude rejected and Draft items (FR-03-06)")
    o1["Forecast version [Published]"]
    stop1@{ shape: fr-circ }
  end
  subgraph ANA["NBC Forecasting Analyst"]
    a1("Open the draft batch")
    a2("Review the next Needs Review item")
    d1{"Decision on item?"}
    a3("Approve as is")
    a4("Adjust the value with a reason and approve")
    a5("Reject with a reason")
    m2{" "}
    d2{"More Needs Review items?"}
    a6("Approve the Normal group in one action")
    a7("Publish")
  end
  start --> s0 --> s1 --> a1 --> s2 --> m1 --> a2 --> d1
  d1 -->|"[approve]"| a3 --> m2
  d1 -->|"[adjust]"| a4 --> m2
  d1 -->|"[reject]"| a5 --> m2
  m2 --> s3 --> d2
  d2 -->|"[yes]"| m1
  d2 -->|"[no]"| a6 --> a7 --> s4 --> o1 --> stop1
```

Reading notes:

- FR-03-05 runs without an actor; it is drawn here because it is the trigger of UC-15.
- A rejected item is not regenerated in release 1; its reason goes to the Data/ML team outside NBSP (D-21).
- Approving the Normal group is a separate bulk action after the Needs Review loop.

## 2. Node trace

| Node | Kind | Text | Traces to |
|---|---|---|---|
| s0 | Action | Run the active model, create Draft items | FR-03-05 |
| s1 | Action | Notify the Analyst | FR-03-05, D-20 |
| s2 | Action | Show groups, flag reasons, previous accuracy | FR-03-06 |
| d1 | Decision | Decision on item? | FR-03-06 |
| s3 | Action | Store decision with reviewer and time | FR-03-06, FR-04-04 |
| a6 | Action | Approve the Normal group | FR-03-06 |
| s4 | Action | Publish approved items as a new version | FR-03-06, BR-07 |

## 3. Branches and acceptance tests

| Branch | Given | When | Then |
|---|---|---|---|
| d1 [approve] | Needs Review item: Hospital A, O+, December, 120 units | Analyst approves | Item Approved; decision stored |
| d1 [adjust] | Same item | Analyst sets 100 with reason "planned surgery cancelled" | Item Approved with system value 120 and adjusted value 100 stored |
| d1 [reject] | Same item | Analyst rejects with reason "model ignored holiday" | Item Rejected, not published, reason stored |
| d2 [no] → publish | 3 Needs Review items decided, 40 Normal items | Analyst approves the Normal group and publishes | New version with the approved items; UC-16 shows it; counts of approved, rejected and Draft shown |

## 4. Object states used

| Object | States in this diagram | Full state list |
|---|---|---|
| Forecast item | Draft, Approved, Rejected | Draft, Approved, Rejected (D-23) |
| Forecast version | Published | Published (D-23) |

## 5. Screens touched

| Actor action | Screen ID |
|---|---|
| Open, review, decide, publish | S-F03-04 |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | Can two Analysts review the same batch at the same time? | No | Last saved decision per item wins; each decision is audited |
