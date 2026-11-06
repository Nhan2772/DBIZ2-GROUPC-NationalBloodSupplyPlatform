# Activity Diagram: AD-UC-11 Receive Below-Threshold Alert

| Field | Value |
| :---- | :---- |
| AD ID | `AD-UC-11` |
| Use case | `UC-11 Receive Below-Threshold Alert` |
| Feature | `F-02 Inventory Monitoring & Usage` |
| Primary actor | NBC Staff (receives the alert). The flow is called at the end of AD-UC-17, after each synchronisation |
| Lanes | NBSP · NBC Staff |
| Inputs (L3-L5) | Context Diagram (Fig. 2\) · Use Case Diagram (Fig. 3, UC-11 follows UC-17) · Sequence: `SD-UC-11` · W1 row FR-02-04 (EARS v2, approved 2026-11-01) |
| Trigger | A synchronisation has just updated the NBC inventory snapshot (end of AD-UC-17) |
| Preconditions | The snapshot is updated; a threshold is configured for each blood type (set by an administrator, W0 Table 3\) |
| Success end state | For every blood type below its threshold, an alert exists and NBC Staff have received an in-system notification |
| Failure end state(s) | No blood type is below its threshold: no alert is created |
| FRs covered | `FR-02-04` |
| Business rules applied | None with a BR ID. Assumption: alerts are informational only; allocation is manual via UC-07 and UC-08 |
| Version / status | v0.6 · Draft (UC-17, EARS changes and lane name "NBC Staff" approved 2026-11-01) |
| Author | Lê Thanh Nga (UC-11 owner in W0) |

## 1\. Diagram

```mermaid
swimlane-beta TB
  subgraph SYS["NBSP"]
    start@{ shape: sm-circ }
    s1("Compare each blood type stock with its threshold (FR-02-04)")
    d1{"Any blood type below threshold?"}
    s2("Create alert for each blood type below threshold (FR-02-04)")
    o1["Alert [Created]"]
    s3("Send in-system notification to responsible NBC Staff (FR-02-04)")
    stop2@{ shape: fr-circ }
  end
  subgraph ACT["NBC Staff"]
    a1("Receive below-threshold alert")
    stop1@{ shape: fr-circ }
  end
  start --> s1 --> d1
  d1 -->|"[below threshold]"| s2 --> o1 --> s3 --> a1 --> stop1
  d1 -->|"[not below threshold]"| stop2
```

&nbsp;

Reading notes:

- The activity has no actor trigger. It is called by AD-UC-17 (step "Check thresholds and create alerts") after a valid synchronisation.  
- The diagram creates one alert per synchronisation while a blood type stays below its threshold. Whether repeats are wanted is open question 1\.  
- Alerts are informational only; allocation is done manually in UC-07 and UC-08.

## 2\. Node trace

| Node | Kind | Text | Traces to |
| :---- | :---- | :---- | :---- |
| s1 | Action | Compare each blood type stock with its threshold | FR-02-04 |
| d1 | Decision | Any blood type below threshold? | FR-02-04 |
| s2 | Action | Create alert for each blood type below threshold | FR-02-04 |
| s3 | Action | Send in-system notification to responsible NBC Staff | FR-02-04 |
| a1 | Action (actor lane) | Receive below-threshold alert | UC-11 main flow |

## 3\. Branches and acceptance tests

| Branch | Given | When | Then |
| :---- | :---- | :---- | :---- |
| d1 \[below threshold\] | Threshold of O- is 30 units; synchronised stock of O- is 22 units | The synchronisation has updated the snapshot | An alert is created for O- and an in-system notification is sent to NBC Staff |
| d1 \[not below threshold\] | Threshold of O- is 30 units; synchronised stock of O- is 45 units | The synchronisation has updated the snapshot | No alert is created and the activity ends |

## 4\. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
| :---- | :---- | :---- |
| Alert | Created | Created (other states to be defined; add to the state table in activity-index.md) |

## 5\. Screens touched

| Actor action | Screen ID |
| :---- | :---- |
|  |  |

## 6\. Open questions

| \# | Question | Blocking? | Default applied |
| :---- | :---- | :---- | :---- |
| 1 | \[NEEDS CLARIFICATION: is a new alert created at every sync while a blood type stays below threshold?\] To: client | No | One new alert per sync until the stock recovers (as drawn); repeats may pile up |
| 2 | \[NEEDS CLARIFICATION: who is the "responsible NBC Staff" (all NBC Staff, or by role)?\] To: client | No | All NBC Staff |
| 3 | \[NEEDS CLARIFICATION: is `Alert [Created]` the final state name, and which other states exist?\] To: team | No | `Created` used until the team fixes the list |
| 4 | RESOLVED 2026-11-01: UC-17 approved; the synchronisation steps live in AD-UC-17 | No | AD-UC-17 |
| 5 | RESOLVED 2026-11-01: EARS of FR-02-04 approved ("after a synchronisation ... is below its configured threshold", per blood type) | No | FR-02-04 EARS v2 |
| 6 | RESOLVED 2026-11-01: lane and actor name is "NBC Staff" in the UC list, Use Case Diagram and Context Diagram | No | NBC Staff |

## 7\. Verification log

| Check | Result | By | Date |
| :---- | :---- | :---- | :---- |
| Renders on mermaid.live | To be confirmed by the author |  |  |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass on reading (d1); confirm after render |  |  |
| No action has two incoming flows (merge used instead) | Pass on reading (only the final node stop2 is a target of one flow; no node has two) |  |  |
| Every fork has a matching join | Pass (no fork) |  |  |
| Every system action cites an FR | Pass on reading |  |  |
| Every FR of this use case appears in the diagram | 1 of 1 |  |  |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass (actor name "NBC Staff" approved) |  |  |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass on reading after SD-UC-11 is updated (alt "below threshold" \= d1) |  |  |
| AI critic prompt run, findings resolved or logged as open questions | To be run by the author |  |  |

&nbsp;