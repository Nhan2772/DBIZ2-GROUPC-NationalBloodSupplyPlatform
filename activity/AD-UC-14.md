# **Activity Diagram: AD-UC-14 Respond to Data Verification Request**

| Field | Value |
| :---- | :---- |
| AD ID | AD-UC-14 |
| Use case | UC-14 Respond to Data Verification Request |
| Feature | F-03 \<feature name, confirm in W0 Table 2\> (function list ID FL:F-14) |
| Primary actor | Hospital Staff |
| Lanes | Hospital Staff · NBSP (name taken from the workbook, confirm against the Context Diagram) · no external systems |
| Inputs (L3-L5) | Context Diagram v\<n\> · Use Case Diagram v\<n\> (UC-14 «extend» UC-13) · Sequence: SD-UC-14 · Architecture/ADR: not used · W1 rows: FR-03-04 |
| Trigger | Hospital Staff submits a confirmation or correction for a data verification request (FR-03-04). |
| Preconditions | Hospital Staff is signed in (assumed; UC-06). The Forecasting Analyst has requested verification (UC-13 / FR-03-03). The request belongs to that hospital (assumed). |
| Success end state | The response is saved, the request is marked Answered and the NBC Forecasting Analyst is notified (to confirm, see Q1). |
| Failure end state(s) | The response is incomplete: an error is shown and the request stays Open, see Q2. |
| FRs covered | FR-03-04 |
| Business rules applied | none (the workbook lists no BR rows) |
| Version / status | v0.2 · Draft |

## 

## 

## **1\. Diagram**

swimlane-beta TB

  subgraph HS\["Hospital Staff"\]

    start@{ shape: sm-circ }

    a1("Open a data verification request")

    a2("Review the flagged records")

    a3("Submit a confirmation or a correction with a note")

  end

  subgraph SYS\["NBSP"\]

    d1{"Response complete?"}

    s1("Save the response and mark the request as Answered (FR-03-04)")

    s2("Notify the NBC Forecasting Analyst (FR-03-04)")

    s3("Show an error: required information is missing; the request stays Open (FR-03-04)")

    stop1@{ shape: fr-circ }

  end

  start \--\> a1 \--\> a2 \--\> a3 \--\> d1

  d1 \--\>\|"\[complete\]"\| s1 \--\> s2 \--\> stop1

  d1 \--\>\|"\[else\]"\| s3 \--\> stop1

## **2\. Node trace**

| Node | Kind | Text | Traces to |
| ----- | ----- | ----- | ----- |
| a3 | Action | Submit a confirmation or a correction with a note | FR-03-04 (WHEN condition) |
| d1 | Decision | Response complete? | FR-03-04, to confirm, see Q2 |
| s1 | Action | Save the response and mark the request as Answered | FR-03-04, to confirm, see Q1 |
| s2 | Action | Notify the NBC Forecasting Analyst | FR-03-04, to confirm, see Q1 |
| s3 | Action | Show an error: required information is missing; the request stays Open | none, see Q2 |

## **3\. Branches and acceptance tests**

| Branch | Given | When | Then |
| ----- | ----- | ----- | ----- |
| d1 \[complete\] | A verification request has status Open and flags 2 records (example values) | Hospital Staff confirms one record, corrects the other with a value and a note, and submits | The response is saved, the request status is Answered and the Analyst is notified |
| d1 \[else\] | A verification request has status Open | Hospital Staff submits a correction without a value | An error is shown, the request stays Open and nothing is saved |

## **4\. Object states used**

| Object | States in this diagram | Full state list (all diagrams) |
| ----- | ----- | ----- |
| Verification request | Open → Answered | Open, Answered (assumed, see Q3) |

## **5\. Screens touched**

| Actor action | Screen ID |
| ----- | ----- |
| Open a data verification request |  |
| Review the flagged records |  |
| Submit the response |  |

## 

## 

## **6\. Open questions**

| \# | Question | Blocking? | Default applied |
| ----- | ----- | ----- | ----- |
| Q1 | \[NEEDS CLARIFICATION: FR-03-04 is cut off in the screenshot. What is the third option after "confirmation, correction or ..."? What exactly does the system SHALL do after submission (save, change status, notify the Analyst)?\] | No | Save, mark Answered, notify the Analyst. |
| Q2 | \[NEEDS CLARIFICATION: Does FR-03-04 define a validation of the response, and which fields are required?\] | No | A corrected value is required when the data is not confirmed. |
| Q3 | \[NEEDS CLARIFICATION: Is Answered the only closing status, or can a request also be Expired or Cancelled?\] | No | Only Open and Answered are used. |

## **7\. Verification log**

| Check | Result | By | Date |
| ----- | ----- | ----- | ----- |
|  |  |  |  |
| Every decision has guards on all outgoing flows, no overlap, no gap |  |  | 2026-10-25 |
| No action has two incoming flows (merge used instead) |  |  | 2026-10-25 |
| Every fork has a matching join |  |  | 2026-10-25 |
| Every system action cites an FR |  |  | 2026-10-25 |
| Every FR of this use case appears in the diagram |  |  | 2026-10-25 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes |  |  | 2026-10-25 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions |  |  |  |
| AI critic prompt run, findings resolved or logged as open questions |  |  |  |

