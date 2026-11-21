# Activity Diagram: AD-UC-11 Receive Below-Threshold Alert

| Field | Value |
| :---- | :---- |
| AD ID | `AD-UC-11` |
| Use case | `UC-11 Receive Below-Threshold Alert` |
| Feature | `F-02 Inventory Monitoring & Usage` |
| Primary actor | NBC Blood Storage and Distribution Department and NBC Operations (receive the alert, D-16). The flow is called at the end of AD-UC-17, after each synchronisation |
| Lanes | NBSP · NBC Blood Storage and Distribution Department · NBC Operations |
| Inputs (L3-L5) | Context Diagram (Fig. 2) · Use Case Diagram (Fig. 3, UC-11 follows UC-17) · Sequence: `SD-UC-11` · W1 row FR-02-04 (EARS v2, approved 2026-11-01) · Decisions of 2026-11-06 |
| Trigger | A synchronisation has just updated the NBC inventory snapshot (end of AD-UC-17) |
| Preconditions | The snapshot is updated. A threshold exists for each blood type: the values are loaded when the system is deployed and are not changed in release 1 (decided 2026-11-06) |
| Success end state | For every blood type that has just fallen below its threshold, an Open alert exists and the Dept and NBC Operations are notified (in-app badge and email, D-20); alerts whose stock is back at or above the threshold are Resolved |
| Failure end state(s) | No blood type is below its threshold: no alert is created |
| FRs covered | `FR-02-04` |
| Business rules applied | None with a BR ID. Assumptions: thresholds are fixed values loaded at deployment (no screen and no use case to configure them); alerts are informational only, allocation is manual via UC-07 and UC-08 |
| Version / status | v1.0 · Draft (18/11/2026: NBC Operations is a recipient again, one alert per episode; decision log D-16, D-17) |
| Author | Lê Thanh Nga (UC-11 owner in W0) |

## 1\. Diagram

```mermaid
swimlane-beta TB
  subgraph SYS["NBSP"]
    start@{ shape: sm-circ }
    s0("Resolve Open alerts whose stock is back at or above threshold (FR-02-04)")
    s1("Compare each blood type stock with its threshold (FR-02-04)")
    d1{"Any blood type below threshold without an Open alert?"}
    s2("Create an Open alert for each such blood type (FR-02-04)")
    o1["Alert [Open]"]
    s3("Notify the Dept and NBC Operations by in-app badge and email (FR-02-04)")
    f1@{ shape: fork }
    stop2@{ shape: fr-circ }
  end
  subgraph DEP["NBC Blood Storage and Distribution Department"]
    a1("Receive below-threshold alert")
    stop1@{ shape: fr-circ }
  end
  subgraph OPS["NBC Operations"]
    a2("Receive below-threshold alert")
    stop3@{ shape: fr-circ }
  end
  start --> s0 --> s1 --> d1
  d1 -->|"[newly below threshold]"| s2 --> o1 --> s3 --> f1
  f1 --> a1 --> stop1
  f1 --> a2 --> stop3
  d1 -->|"[else]"| stop2
```

Reading notes:

- The activity has no actor trigger. It is called by AD-UC-17 (step "Check thresholds and create alerts") after a valid synchronisation.
- The Dept and NBC Operations both receive the alert (D-16): Operations plans donor mobilisation when stock is low. The fork ends in two activity finals, one per recipient.
- One alert per episode (D-17): no new alert while one is Open for the blood type; the alert is Resolved at the first sync where stock is back at or above the threshold.
- Thresholds are not changed by anyone in release 1; they are values loaded at deployment (decision 2026-11-06).
- Alerts are informational only; allocation is done manually in UC-07 and UC-08.

## 2\. Node trace

| Node | Kind | Text | Traces to |
| :---- | :---- | :---- | :---- |
| s1 | Action | Compare each blood type stock with its threshold | FR-02-04 |
| d1 | Decision | Any blood type below threshold? | FR-02-04 |
| s2 | Action | Create alert for each blood type below threshold | FR-02-04 |
| s0 | Action | Resolve Open alerts whose stock is back at or above threshold | FR-02-04 |
| s3 | Action | Notify the Dept and NBC Operations by in-app badge and email | FR-02-04 |
| a2 | Action (Operations lane) | Receive below-threshold alert | UC-11 main flow |
| a1 | Action (Dept lane) | Receive below-threshold alert | UC-11 main flow |

## 3\. Branches and acceptance tests

| Branch | Given | When | Then |
| :---- | :---- | :---- | :---- |
| d1 \[below threshold\] | Threshold of O- is 30 units; synchronised stock of O- is 22 units | The synchronisation has updated the snapshot | An alert is created for O- and the Dept receives an in-system notification |
| d1 \[not below threshold\] | Threshold of O- is 30 units; synchronised stock of O- is 45 units | The synchronisation has updated the snapshot | No alert is created and the activity ends |

## 4\. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
| :---- | :---- | :---- |
| Alert | Open | Open, Resolved (decision log D-23) |

## 5\. Screens touched

| Actor action | Screen ID |
| :---- | :---- |
| Receive below-threshold alert | S-F02-02 (banner) and email |

## 6\. Open questions

| \# | Question | Blocking? | Default applied |
| :---- | :---- | :---- | :---- |
| 1 | \[NEEDS CLARIFICATION: is a new alert created at every sync while a blood type stays below threshold?\] To: client | No | RESOLVED 18/11 (D-17): one alert per episode |
| 2 | SUPERSEDED 18/11 (D-16): the Dept and NBC Operations both receive the alert | No | Both, as drawn |
| 3 | RESOLVED 18/11 (D-23): states Open, Resolved | No | Open, Resolved |
| 4 | RESOLVED 2026-11-01: UC-17 approved; the synchronisation steps live in AD-UC-17 | No | AD-UC-17 |
| 5 | RESOLVED 2026-11-01: EARS of FR-02-04 approved (per blood type, after a synchronisation) | No | FR-02-04 EARS v2 |
| 6 | SUPERSEDED 2026-11-06: "NBC Staff" is split into the NBC Blood Storage and Distribution Department and the Operations (Forecast) side | No | NBC Blood Storage and Distribution Department, NBC Operations |
| 7 | RESOLVED 2026-11-06: thresholds are loaded at deployment and are not changed; there is no use case to configure them. The Context Diagram flow "NBC Operations configure thresholds" must be removed or reworded | No | Fixed values loaded at deployment |

## 7\. Verification log

| Check | Result | By | Date |
| :---- | :---- | :---- | :---- |
| Renders on mermaid.live | To be confirmed by the author |  |  |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass on reading (d1); confirm after render |  |  |
| No action has two incoming flows (merge used instead) | Pass on reading (no node has two incoming flows) |  |  |
| Every fork has a matching join | Pass (no fork) |  |  |
| Every system action cites an FR | Pass on reading |  |  |
| Every FR of this use case appears in the diagram | 1 of 1 |  |  |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass after rename 2026-11-06 (confirm the UC list text) |  |  |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass on reading after SD-UC-11 keeps one recipient (alt "below threshold" = d1) |  |  |
| AI critic prompt run, findings resolved or logged as open questions | To be run by the author |  |  |
