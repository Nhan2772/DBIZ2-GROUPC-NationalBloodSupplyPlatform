# Change log: 18/11/2026 consistency pass

Applies [decision-log.md](decision-log.md) v1.0. Part A lists naming and reference fixes (no change of meaning). Part B lists logic and flow changes; each cites the decision behind it. Review Part B before committing.

## A. Naming, IDs and links (no change of meaning)

| File | Change |
|---|---|
| W0, W1, SD-UC-07, 08, 09, 10, 12, screen list | "NBC Staff" and "NBC Blood storage & Distribution dept" → "NBC Blood Storage and Distribution Department" (the Dept); sign-in actor → "NBC users" (D-01) |
| W0 Table 1 | Actors: UC-09 + NBC Operations; UC-11 + NBC Operations; UC-12 + Analyst, Operations; UC-17 = Time, secondary BMS. Table 2 F-02 actors fixed (D-01) |
| W1 | Removed legacy `FL:F-xx` trace codes (D-03); FR-03-05 actor → System; FR-02-02 actors listed in full |
| SD-UC-01 to SD-UC-17 | Lifelines renamed to NBSP Web App, NBSP Backend, NBSP Database, NBSP Scheduler, Forecast Model, Blood Management Software (D-24) |
| SD-UC-09, SD-UC-10 | Header "UC-XX" → UC-09, UC-10; SD-UC-10 FRs → FR-02-03 |
| SD-UC-11, 12, 13 | Removed source notes pointing to chat histories |
| SD-UC-02, SD-UC-06 | Links to non-existent AD-UC-02 / AD-UC-06 → activity index ("not needed") |
| W5, activity index | Product name → "National Blood Supply Chain Platform" (D-02) |
| Project brief | "about 170 hospitals" → "180+" (D-02); link to W6 fixed; link to decision log |
| `cvca` (root) | Moved to `context/cvca.md` (fixes two broken links); node "Blood Center Management" → "Blood Management Software (BMS)" |
| Screen transition | Link `usage-flow.md` → `../usage-flow/usage-flow-total.md` |
| AD-UC-05 | Placeholders `FR-05-xx` → FR-03-01; feature F-03; lane "Hospital Blood Mgmt Portal" → NBSP |
| AD-UC-11 | Screen S-F02-04 (does not exist) → S-F02-02 |
| AD-UC-01, 03, 04 | Branches marked "proposed" now trace to the approved FR-01-06, FR-01-07, FR-02-06 |
| New `architecture/architecture.md` | Lifeline list and diagram (was linked from 6 files but missing) |
| README | Reading order with links |

## B. Logic and flow changes

| # | File | Change | Decision |
|---|---|---|---|
| B1 | W0 BR-01, BR-02, BR-05; new BR-09 | Product type required; urgency = Emergency, Urgent, Routine; Not approved state; FEFO rule | D-04, D-07, D-08, D-12 |
| B2 | W0 Table 3 | New Won't-have rows: hospital cancellation, products other than red cells, hospital-level alerts; list of configurable values | D-06, D-12, D-18, D-19 |
| B3 | W1 FR-01-03 | EARS adds FEFO pre-selection, idempotency key, "no longer valid" rejection, timeout, release of reservation | D-07, D-09 |
| B4 | W1 FR-01-04 | "approved or dispatched" → "dispatched" | D-11 |
| B5 | W1 FR-01-09 | Final state Not approved with reason list, shown to the hospital | D-04 |
| B6 | W1 FR-02-04 | Recipients Dept and NBC Operations; one Open alert per episode, Resolved on recovery; badge and email. L3 FE: no notification centre | D-16, D-17, D-20 |
| B7 | W1 FR-03-01 | IF clause for invalid plan data (UC-05 had no FR for its error branch) | — |
| B8 | W1 new FR-02-07 | Opening balance and stock adjustment, 2 L3 rows (5.4 person-hours, 1,420,000 VND). W1 totals: 29 L2, 54 L3, 356 base hours | D-14 |
| B9 | W4 v2 | FR-01-04 Could → Must; FR-01-09 Could → Should; FR-03-01 Should → Could; FR-02-07 added (Should); justifications of FR-02-03, FR-02-04, FR-02-06 rewritten | D-25 |
| B10 | W2 v2 | Effective priorities and new rows; total 356.0 base h, 275.8 person-hours, 76,240,000 VND. FP cross-check variance marked "to recalculate" | D-25 |
| B11 | W5 | Scenarios 162.2 h / 45.16 M · 217.6 h / 60.08 M · 275.8 h / 76.24 M; 32 Must L3 items (FR-01-04 rows added); open questions 5 to 8 added | D-25 |
| B12 | W6 | Note at the top: tables still on v1 costs, recalculate in the workbook and add the missing benefit derivation ("Section 1.6") | D-25 |
| B13 | AD-UC-08 v0.5 | New Dept decision "Dispatch or not approve?" with Not approved branch (FR-01-09); shortage traces to FR-01-08; Q2 resolved | D-04, D-05, D-09 |
| B14 | SD-UC-08 | Decline branch kept as Not approved; FEFO pre-selection; note on releasing the reservation if the save fails | D-04, D-07, D-09 |
| B15 | AD-UC-11 v1.0, SD-UC-11 | NBC Operations added as a recipient (fork, two lanes); resolve step for recovered stock; alert only when no Open alert exists; states Open, Resolved | D-16, D-17 |
| B16 | AD-UC-14 v1.0 | Redrawn: confirm / correct / supplement, incomplete loop, deadline → Overdue; states Open, Responded, Overdue; now in a mermaid block | D-22, D-23 |
| B17 | AD-UC-15 v1.0 | Redrawn per item: Needs Review loop (approve, adjust, reject), bulk approve Normal, publish new version; replaces batch-level approve/reject | D-21, D-23 |
| B18 | AD-UC-03, AD-UC-04 | Q2 (partial receipt) and Q3 (FEFO) resolved | D-10, D-13 |
| B19 | SD-UC-04 | Rewritten: confirmation after save, through Backend, FEFO lot deduction, usage date; removed out-of-scope "Model Training Service" | D-13, D-24 |
| B20 | SD-UC-05 | Rewritten: confirmation after save, error branch traces to FR-03-01 | D-24 |
| B21 | SD-UC-07 | Rewritten: scope check through Backend; `alt` before the display | D-24 |
| B22 | Activity index | Rewritten: findings closed, priorities from W4 v2, only existing diagrams listed (others "not needed" with reason), state catalogue from D-23 | D-23 |
| B23 | Use case diagram | NBC Operations — UC-11 line added; SD status table updated | D-16 |
| B24 | Usage flow v2.3 | Dept Flow 1 end state includes Not approved; Hospital flows numbered 1 to 4; UC-05 error traces to FR-03-01; UC-11 recipients and channel; findings closed | D-04, D-16, D-20 |
| B25 | Screen list | S-F02-02 + NBC Operations; S-F02-01 + FR-02-07 tab; S-F03-01 priority Could; modal "Not approve request" with reason list; UC-11 channel fixed | D-04, D-14, D-16, D-20, D-25 |
| B26 | Screen transition | Hospital entry "order rejected" → "request not approved" | D-04 |
| B27 | UI conventions | Label `Not approved`; OQ-01, 02, 03, 07 marked resolved; S-F02-02 actors; alert channel | D-01, D-04, D-19, D-20 |
| B28 | Project brief | Goal reworded: release 1 warns on NBC stock; hospital-stock warnings later | D-18 |

## C. Not changed (still to do by the owner)

- W6 NPV, IRR and payback: recalculate in the workbook with the v2 costs; add the benefit derivation.
- W2 function-point cross-check: recalculate transactional FP with FR-02-07 (EI) and state the tolerance.
- Mid-term report headings (`UC-8`, `UC14`, `UC 12`): not in this repo.
- Project brief: roles and owned features of the five members.
- Screen specs (`screens/screen-spec-*.md`) do not exist yet.
