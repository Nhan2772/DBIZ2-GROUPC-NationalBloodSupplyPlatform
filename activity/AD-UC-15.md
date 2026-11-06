# **Activity Diagram: AD-UC-15 Review and Approve Blood Forecast**

| Field | Value |
| ----- | ----- |
| AD ID | AD-UC-15 |
| Use case | UC-15 Review and Approve Blood Forecast |
| Feature | F-03 \<feature name, confirm in W0 Table 2\> (function list ID FL:F-15) |
| Primary actor | NBC Forecasting Analyst |
| Lanes | NBC Forecasting Analyst · NBSP (name taken from the workbook, confirm against the Context Diagram) · no external systems |
| Inputs (L3-L5) | Context Diagram v\<n\> · Use Case Diagram v\<n\> · Sequence: SD-UC-15 · Architecture/ADR: ASM:active model (from FR-03-05) · W1 rows: FR-03-05, FR-03-06 |
| Trigger | Demand data for a forecasting period is finalised and the system creates a versioned Draft forecast (FR-03-05). The Analyst then reviews the Draft forecast batch (FR-03-06). |
| Preconditions | The Analyst is signed in (assumed; UC-12). Demand data for the forecasting period is finalised (UC-13). |
| Success end state | The Draft forecast is Approved and available to NBC Operations (UC-16), or it is Rejected and the reason is stored (to confirm, see Q1). |
| Failure end state(s) | none identified, see Q2 |
| FRs covered | FR-03-05, FR-03-06 |
| Business rules applied | none  |

## **1\. Diagram**

swimlane-beta TB  
  subgraph ANA\["NBC Forecasting Analyst"\]  
    a1("Open the Draft forecast batch")  
    a2("Review the Draft forecasts")  
    d1{"Batch acceptable?"}  
    a3("Approve the forecast")  
    a4("Enter a rejection reason and reject the forecast")  
  end  
  subgraph SYS\["NBSP"\]  
    start@{ shape: sm-circ }  
    s0("Run the active model on accepted demand data and NBC allocation history, create a versioned Draft forecast and notify the Analyst (FR-03-05)")  
    s1("Set the status to Approved and make the forecast available as validated (FR-03-06)")  
    s2("Save the rejection reason and set the status to Rejected (FR-03-06)")  
    stop1@{ shape: fr-circ }  
  end  
  start \--\> s0 \--\> a1 \--\> a2 \--\> d1  
  d1 \--\>\|"\[approve\]"\| a3 \--\> s1 \--\> stop1  
  d1 \--\>\|"\[reject\]"\| a4 \--\> s2 \--\> stop1

Reading notes:

* FR-03-05 is the trigger of UC-15 (function list: "UC:UC-15 trigger"). The system runs it when demand data is finalised, so the diagram starts in the NBSP lane. If your lecturer wants it out of this diagram, remove start, s0 and its two flows, and move FR-03-05 into the Trigger field only.  
* The "active model" comes from the ASM:active model entry of FR-03-05. Model training happens outside NBSP (Data/ML Team).  
* FR-03-06 is cut off in the screenshot ("WHEN the Forecasting Analyst reviews a Draft forecast batch, the ..."). The approve and reject outcomes below are my assumption, see Q1.  
* The decision is a human choice by the Analyst, so it sits in the Analyst lane.

## **2\. Node trace**

| Node | Kind | Text | Traces to |
| ----- | ----- | ----- | ----- |
| s0 | Action | Run the active model on accepted demand data and NBC allocation history, create a versioned Draft forecast and notify the Analyst | FR-03-05 |
| a1 | Action | Open the Draft forecast batch | FR-03-06 (WHEN condition) |
| d1 | Decision | Batch acceptable? | FR-03-06, to confirm, see Q1 |
| s1 | Action | Set the status to Approved and make the forecast available as validated | FR-03-06, to confirm, see Q1 |
| s2 | Action | Save the rejection reason and set the status to Rejected | FR-03-06, to confirm, see Q1 |

## **3\. Branches and acceptance tests**

| Branch | Given | When | Then |
| ----- | ----- | ----- | ----- |
| d1 \[approve\] | A Draft forecast (version 1\) exists for blood group O+ for November 2026 (example values) | The Analyst reviews the batch and approves it | The status becomes Approved and NBC Operations can see the forecast as validated |
| d1 \[reject\] | A Draft forecast (version 1\) exists for blood group O+ for November 2026 (example values) | The Analyst enters the reason "demand spike not reflected" and rejects | The status becomes Rejected, the reason is stored and NBC Operations cannot see the forecast |

## **4\. Object states used**

| Object | States in this diagram | Full state list (all diagrams) |
| ----- | ----- | ----- |
| Blood forecast | Draft (created by FR-03-05) → Approved; Draft → Rejected | Draft, Approved (validated), Rejected (assumed, see Q1) |

## 

## **5\. Screens touched**

| Actor action | Screen ID |
| ----- | ----- |
| Open the Draft forecast batch |  |
| Review the Draft forecasts |  |
| Approve or reject the forecast |  |

## **6\. Open questions**

| \# | Question | Blocking? | Default applied |
| ----- | ----- | ----- | ----- |
| Q1 | \[NEEDS CLARIFICATION: FR-03-06 and the end of FR-03-05 are cut off in the screenshot. Who is notified in FR-03-05? What does the system SHALL do in FR-03-06 (approve, reject, reason, statuses)? Is a whole batch approved at once, or each forecast on its own?\] | No | The Analyst is notified; approve or reject the forecast, the reason is stored. |
| Q2 | \[NEEDS CLARIFICATION: Can the Analyst review a forecast that is already Approved or Rejected? What happens when two Analysts open the same batch?\] | No | Only forecasts with status Draft can be reviewed. |
| Q3 | \[NEEDS CLARIFICATION: Is the rejection reason mandatory, and what happens to a Rejected forecast: is a new version created?\] | No | Mandatory free text. No new version is drawn here. |

## **7\. Verification log**

| Check | Result | By | Date |
| ----- | ----- | ----- | ----- |
|  |  |  |  |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass (read check) |  | 2026-10-25 |
| No action has two incoming flows (merge used instead) | Pass (read check; only the final node has two) |  | 2026-10-25 |
| Every fork has a matching join | Pass (no forks) |  | 2026-10-25 |
| Every system action cites an FR | Pass (read check; FR text is cut off, see Q1) |  | 2026-10-25 |
| Every FR of this use case appears in the diagram | 2 of 2 |  | 2026-10-25 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Actor lane matches the use case diagram; system lane name to confirm |  | 2026-10-25 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Not checked (sequence diagram not provided) |  |  |
| AI critic prompt run, findings resolved or logged as open questions | Not run yet |  |  |

