# Activity Diagram: AD-UC-11 Receive Below-Threshold Alert

| Field | Value |
| :---- | :---- |
| AD ID | `AD-UC-11` |
| Use case | `UC-11 Receive Below-Threshold Alert` |
| Feature | `F-02 Inventory Monitoring & Usage` |
| Primary actor | NBC Blood Storage and Distribution Department and NBC Operations (both receive the alert). The flow is called at the end of AD-UC-17, after each synchronisation |
| Lanes | NBSP · NBC Blood Storage and Distribution Department · NBC Operations |
| Inputs (L3-L5) | Context Diagram (Fig. 2) · Use Case Diagram (Fig. 3, UC-11 follows UC-17) · Sequence: `SD-UC-11` · W1 row FR-02-04 (EARS v2, approved 2026-11-01) · Decisions of 2026-11-06 |
| Trigger | A synchronisation has just updated the NBC inventory snapshot (end of AD-UC-17) |
| Preconditions | The snapshot is updated. A threshold exists for each blood type: the values are loaded when the system is deployed and are not changed in release 1 (decided 2026-11-06) |
| Success end state | For every blood type below its threshold, an alert exists and the Dept and NBC Operations have received an in-system notification |
| Failure end state(s) | No blood type is below its threshold: no alert is created |
| FRs covered | `FR-02-04` |
| Business rules applied | None with a BR ID. Assumptions: thresholds are fixed values loaded at deployment (no screen and no use case to configure them); alerts are informational only, allocation is manual via UC-07 and UC-08 |
| Version / status | v0.8 · Draft (actor split and recipients decided 2026-11-06) |
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
    s3("Send in-system notification to the Dept and NBC Operations (FR-02-04)")
    f1@{ shape: fork }
    j1@{ shape: join }
    stop1@{ shape: fr-circ }
    stop2@{ shape: fr-circ }
  end
  subgraph DEP["NBC Blood Storage and Distribution Department"]
    a1("Receive below-threshold alert")
  end
  subgraph OPS["NBC Operations"]
    a2("Receive below-threshold alert")
  end
  start --> s1 --> d1
  d1 -->|"[below threshold]"| s2 --> o1 --> s3 --> f1
  f1 --> a1 --> j1
  f1 --> a2 --> j1
  j1 --> stop1
  d1 -->|"[not below threshold]"| stop2
```

Reading notes:

- The activity has no actor trigger. It is called by AD-UC-17 (step "Check thresholds and create alerts") after a valid synchronisation.
- The two recipients receive the alert at the same time and in no required order, so the notification forks and joins.
- The diagram creates one alert per synchronisation while a blood type stays below its threshold. Whether repeats are wanted is open question 1.
- Thresholds are not changed by anyone in release 1; they are values loaded at deployment (decision 2026-11-06).
- Alerts are informational only; allocation is done manually in UC-07 and UC-08.

## 2\. Node trace

| Node | Kind | Text | Traces to |
| :---- | :---- | :---- | :---- |
| s1 | Action | Compare each blood type stock with its threshold | FR-02-04 |
| d1 | Decision | Any blood type below threshold? | FR-02-04 |
| s2 | Action | Create alert for each blood type below threshold | FR-02-04 |
| s3 | Action | Send in-system notification to the Dept and NBC Operations | FR-02-04 (EARS still says "responsible NBC Staff": update) |
| f1 / j1 | Fork / Join | Both recipients are notified, no required order | control nodes |
| a1 | Action (Dept lane) | Receive below-threshold alert | UC-11 main flow |
| a2 | Action (Operations lane) | Receive below-threshold alert | UC-11 main flow |

## 3\. Branches and acceptance tests

| Branch | Given | When | Then |
| :---- | :---- | :---- | :---- |
| d1 \[below threshold\] | Threshold of O- is 30 units; synchronised stock of O- is 22 units | The synchronisation has updated the snapshot | An alert is created for O-; the Dept and NBC Operations both receive an in-system notification |
| d1 \[not below threshold\] | Threshold of O- is 30 units; synchronised stock of O- is 45 units | The synchronisation has updated the snapshot | No alert is created and the activity ends |

## 4\. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
| :---- | :---- | :---- |
| Alert | Created | Created (other states to be defined; add to the state table in activity-index.md) |

## 5\. Screens touched

| Actor action | Screen ID |
| :---- | :---- |
| Receive below-threshold alert (Dept and NBC Operations) | S-F02-04 |

## 6\. Open questions

| \# | Question | Blocking? | Default applied |
| :---- | :---- | :---- | :---- |
| 1 | \[NEEDS CLARIFICATION: is a new alert created at every sync while a blood type stays below threshold?\] To: client | No | One new alert per sync until the stock recovers (as drawn); repeats may pile up |
| 2 | RESOLVED 2026-11-06: recipients are the Dept and NBC Operations (Operations plan allocation using alerts) | No | Both, as drawn. FR-02-04 EARS and the UC list actor of UC-11 must be updated |
| 3 | \[NEEDS CLARIFICATION: is `Alert [Created]` the final state name, and which other states exist?\] To: team | No | `Created` used until the team fixes the list |
| 4 | RESOLVED 2026-11-01: UC-17 approved; the synchronisation steps live in AD-UC-17 | No | AD-UC-17 |
| 5 | RESOLVED 2026-11-01: EARS of FR-02-04 approved (per blood type, after a synchronisation) | No | FR-02-04 EARS v2 |
| 6 | SUPERSEDED 2026-11-06: "NBC Staff" is split into the NBC Blood Storage and Distribution Department and the Operations (Forecast) side | No | NBC Blood Storage and Distribution Department, NBC Operations |
| 7 | RESOLVED 2026-11-06: thresholds are loaded at deployment and are not changed; there is no use case to configure them. The Context Diagram flow "NBC Operations configure thresholds" must be removed or reworded | No | Fixed values loaded at deployment |

## 7\. Verification log

| Check | Result | By | Date |
| :---- | :---- | :---- | :---- |
| Renders on mermaid.live | To be confirmed by the author |  |  |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass on reading (d1); confirm after render |  |  |
| No action has two incoming flows (merge used instead) | Pass on reading (a1 and a2 each have one incoming flow; j1 is a join; stop1 and stop2 are final nodes) |  |  |
| Every fork has a matching join | Pass (f1 to j1) |  |  |
| Every system action cites an FR | Pass on reading |  |  |
| Every FR of this use case appears in the diagram | 1 of 1 |  |  |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass after rename 2026-11-06 (confirm the UC list text) |  |  |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | To update `SD-UC-11`: two recipients (alt "below threshold" = d1) |  |  |
| AI critic prompt run, findings resolved or logged as open questions | To be run by the author |  |  |
