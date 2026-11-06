# Activity Diagram: AD-UC-17 Synchronise NBC Inventory

| Field | Value |
| :---- | :---- |
| AD ID | `AD-UC-17` |
| Use case | `UC-17 Synchronise NBC Inventory` |
| Feature | `F-02 Inventory Monitoring & Usage` |
| Primary actor | Blood Management Software (external system). The flow is started by time: the scheduled inventory synchronisation |
| Lanes | NBSP · Blood Management Software |
| Inputs (L3-L5) | Context Diagram (Fig. 2, API synchronisation with the Blood Management Software) · Use Case Diagram (Fig. 3, UC-17 approved 2026-11-01) · Sequence: `SD-UC-17` · Assumption "NBC inventory is synchronised periodically" · W1 row FR-02-05 (EARS v2 with IF clause, approved 2026-11-01) |
| Trigger | The scheduled inventory synchronisation runs |
| Preconditions | The connection to the Blood Management Software (BMS) API is configured; the synchronisation interval is configured |
| Success end state | The NBSP inventory snapshot holds the latest NBC stock with its source timestamp, and the threshold check (AD-UC-11) has run |
| Failure end state(s) | BMS returns an error or invalid data: the previous snapshot stays unchanged, the failure is logged and the threshold check does not run |
| FRs covered | `FR-02-05` |
| Business rules applied | None with a BR ID. Assumption: UC-17 only reads from BMS and never writes back (reservations belong to UC-08) |
| Version / status | v0.4 · Draft (UC-17 and the IF clause of FR-02-05 approved 2026-11-01) |
| Author | To assign (UC-17 has no owner in W0 yet) |

## 1\. Diagram

```mermaid
swimlane-beta TB
  subgraph SYS["NBSP"]
    t1@{ shape: hourglass }
    s1("Request latest NBC inventory (FR-02-05)")
    d0{"Valid inventory data received?"}
    s2("Update inventory snapshot with source timestamp (FR-02-05)")
    s3("Check thresholds and create alerts (AD-UC-11)")
    s4("Keep previous snapshot and log the failure (FR-02-05)")
    stop1@{ shape: fr-circ }
  end
  subgraph BMS["Blood Management Software"]
    b1("Return latest inventory and source timestamp, or an error (FR-02-05)")
  end
  t1 -->|"every N min (interval to be confirmed)"| s1 --> b1 --> d0
  d0 -->|"[valid]"| s2 --> s3 --> stop1
  d0 -->|"[error or invalid data]"| s4 --> stop1
```

&nbsp;

Reading notes:

- The activity starts from a time event, not from a person. BMS is a lane because it answers the request.  
- b1 returns either the data or an error. A reply that does not arrive within the time limit counts as an error (open question 3).  
- `s3` is a call step: the comparison with the thresholds and the alert are drawn in AD-UC-11, not copied here.  
- Two flows enter `stop1`. This is allowed: a final node is not an action.

## 2\. Node trace

| Node | Kind | Text | Traces to |
| :---- | :---- | :---- | :---- |
| t1 | Time event | Scheduled synchronisation starts the activity | FR-02-05 / ASM periodic sync |
| s1 | Action | Request latest NBC inventory | FR-02-05 |
| b1 | Action (BMS lane) | Return latest inventory and source timestamp, or an error | FR-02-05 |
| d0 | Decision | Valid inventory data received? | FR-02-05 (IF clause, EARS v2) |
| s2 | Action | Update inventory snapshot with source timestamp | FR-02-05 |
| s3 | Action (call step) | Check thresholds and create alerts | AD-UC-11 (FR-02-04) |
| s4 | Action | Keep previous snapshot and log the failure | FR-02-05 (IF clause, EARS v2) |

## 3\. Branches and acceptance tests

| Branch | Given | When | Then |
| :---- | :---- | :---- | :---- |
| d0 \[valid\] | BMS returns O- \= 45 units with source timestamp 2026-10-31 08:00 | The scheduled sync runs | The snapshot is updated with timestamp 08:00 and the threshold check starts (AD-UC-11) |
| d0 \[error or invalid data\] | The snapshot holds O- \= 45 units from 07:45; BMS returns O- \= \-5 units, or an error | The scheduled sync runs | The snapshot still shows O- \= 45 units at 07:45, the failure is logged and no threshold check runs |

## 4\. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
| :---- | :---- | :---- |
| Inventory snapshot | None drawn (the snapshot is replaced, it has no status) | None |

## 5\. Screens touched

| Actor action | Screen ID |
| :---- | :---- |
|  |  |

## 6\. Open questions

| \# | Question | Blocking? | Default applied |
| :---- | :---- | :---- | :---- |
| 1 | \[NEEDS CLARIFICATION: what is the synchronisation interval, 15 minutes or 1 hour?\] To: client | No | Interval is configurable; not fixed in code |
| 2 | \[NEEDS CLARIFICATION: what makes inventory data invalid (negative or non-integer units, unknown blood type)?\] To: client | No | Negative or non-integer units and unknown blood types are invalid |
| 3 | \[NEEDS CLARIFICATION: how long does NBSP wait for BMS, and is a failed synchronisation retried before the next interval?\] To: client | No | One attempt per interval; the next interval retries |
| 4 | \[NEEDS CLARIFICATION: is anyone notified after repeated failures? The dashboards show the synchronisation timestamp, so stale data is visible\] To: client | No | No notification in release 1 |
| 5 | \[NEEDS CLARIFICATION: the Context Diagram shows two-way API synchronisation, FR-02-05 says NBSP only reads. Confirm that UC-17 stays read-only and that reservations (UC-08) are the only write\] To: team | No | UC-17 is read-only |
| 6 | RESOLVED 2026-11-01: UC-17 approved as a new use case; AD-UC-11 starts from the call made by this diagram | No | UC-17 |
| 7 | RESOLVED 2026-11-01: the IF clause for an error or invalid data is added to the EARS of FR-02-05 | No | FR-02-05 EARS v2 |

## 7\. Verification log

| Check | Result | By | Date |
| :---- | :---- | :---- | :---- |
| Renders on mermaid.live | To be confirmed by the author |  |  |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass on reading (d0); confirm after render |  |  |
| No action has two incoming flows (merge used instead) | Pass on reading (only the final node stop1 has two incoming flows, allowed) |  |  |
| Every fork has a matching join | Pass (no fork) |  |  |
| Every system action cites an FR | Pass on reading (s3 is a call step to AD-UC-11) |  |  |
| Every FR of this use case appears in the diagram | 1 of 1 |  |  |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass |  |  |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass on reading after SD-UC-17 is added (alt "BMS error or invalid data" \= d0) |  |  |
| AI critic prompt run, findings resolved or logged as open questions | To be run by the author |  |  |

---

