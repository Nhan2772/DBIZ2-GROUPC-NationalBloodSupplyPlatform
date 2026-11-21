# W5 Baseline and handover: National Blood Supply Chain Platform (NBSP)

Lesson 5 (block W5). The baseline every later lesson and DBIZ3 refer to.

## 1. Three scenarios

| Scenario | Person-hours | Cost (VND) | Role in the cash-flow analysis (W6) |
|---|---|---|---|
| Pessimistic: Must only | 162.2 | 45.160.000 | Lowest investment; delivers only the committed operating spine |
| Base: Must + Should | 217.6 | 60.080.000 | The client figure; base cash flow (Session 8 / NPV baseline) |
| Optimistic: full scope | 275.8 | 76.240.000 | If the Could pool is never consumed as contingency |

The MoSCoW tiers are the three scenarios. No invented ±20% scenario is added. Figures are the W2 v2 roll-up (decision log D-25); the v1 figures were 156.8 h / 43.740.000, 210.2 h / 58.160.000 and 270.4 h / 74.820.000 VND.

## 2. Assumptions

| Assumption | Value | Source |
|---|---|---|
| Junior / Senior / Expert rate | 200.000 / 300.000 / 400.000 VND per hour | Class Parameters §3 (class-fixed labour rate) |
| AI leverage FE / BE / INT | 0.35 / 0.7 / 1.0 | Class Parameters §2 |
| Verification factor precise / vague | 0.15 / 0.4 | Class Parameters §4 |
| Hours per function point (cross-check only) | 3 | Class Parameters §5 |
| Team capacity | 300 person-hours | Class Parameters §6 (hard project capacity) |
| Money contingency | 0% (none added; the Could scope already acts as contingency) | Class Parameters §8 |

## 3. tasks.md seed (effective priority Must)

Risk order: 1 = INT (integration / external dependency), 2 = BE (backend / state logic), 3 = FE (frontend). DBIZ3 starts with the riskiest rows, so unknowns are touched early rather than left to the end. All 32 Must L3 items are listed (162.2 person-hours).

| L3 ID | Layer | Person-hours | Risk order |
|---|---|---|---|
| FR-01-03-INT | INT | 9.2 | 1 |
| FR-02-05-INT | INT | 9.2 | 1 |
| FR-03-05-INT | INT | 9.2 | 1 |
| FR-01-01-BE | BE | 6.8 | 2 |
| FR-01-02-BE | BE | 6.8 | 2 |
| FR-01-03-BE | BE | 13.6 | 2 |
| FR-01-04-BE | BE | 3.4 | 2 |
| FR-01-05-BE | BE | 6.8 | 2 |
| FR-01-06-BE | BE | 3.4 | 2 |
| FR-01-07-BE | BE | 3.4 | 2 |
| FR-01-08-BE | BE | 4.4 | 2 |
| FR-02-01-BE | BE | 6.8 | 2 |
| FR-02-05-BE | BE | 3.4 | 2 |
| FR-03-05-BE | BE | 6.8 | 2 |
| FR-03-06-BE | BE | 6.8 | 2 |
| FR-03-07-BE | BE | 6.8 | 2 |
| FR-03-08-BE | BE | 3.4 | 2 |
| FR-04-01-BE | BE | 6.8 | 2 |
| FR-04-02-BE | BE | 3.4 | 2 |
| FR-04-03-BE | BE | 6.8 | 2 |
| FR-01-01-FE | FE | 4.0 | 3 |
| FR-01-02-FE | FE | 2.0 | 3 |
| FR-01-03-FE | FE | 4.0 | 3 |
| FR-01-04-FE | FE | 2.0 | 3 |
| FR-01-05-FE | FE | 2.0 | 3 |
| FR-01-06-FE | FE | 2.0 | 3 |
| FR-01-08-FE | FE | 3.0 | 3 |
| FR-02-01-FE | FE | 4.0 | 3 |
| FR-03-06-FE | FE | 4.0 | 3 |
| FR-03-07-FE | FE | 4.0 | 3 |
| FR-04-01-FE | FE | 2.0 | 3 |
| FR-04-02-FE | FE | 2.0 | 3 |

## 4. Open questions for the client

Carry each question back into its owning feature specification.

| # | Question | Blocking? | Owning feature | Default applied |
|---|---|---|---|---|
| 1 | Exactly how many days before expiry qualifies a unit as "near-expiry"? | No | F-02 | Keep the expiry window configurable; do not hard-code a day count until NBC confirms it. |
| 2 | What response deadline should a hospital receive for a data-verification request? | No | F-03 | Use a configurable deadline; overdue logic is implemented without fixing the duration in code. |
| 3 | What exact schedule/time defines the forecast cut-off for unresolved demand records? | Yes | F-03 | Use a configurable cycle cut-off; unresolved records are excluded as specified in UC-13. |
| 4 | Which anomaly rules and thresholds are approved for clinical-demand validation? | Yes | F-03 | Implement the rule engine as configurable; business thresholds remain pending analyst/NBC confirmation. |
| 5 | Which blood products can a hospital request (for example whole blood, red cells, platelets, plasma)? | No | F-01 | Keep the product list as reference data loaded at deployment; FR-01-01 validates against it. |
| 6 | What stock threshold applies to each blood type for the below-threshold alert? | No | F-02 | Load NBC's current reorder levels at deployment (thresholds are not edited in NBSP, decided 06/11/2026). |
| 7 | Who supplies each hospital's opening balance, and in what format? | No | F-02 | The project team imports it with the onboarding script (D-14); FR-02-07 covers later entries. |
| 8 | May the Dept override the FEFO unit proposal when dispatching? | No | F-01 | NBSP proposes units in FEFO order and lets the Dept change the selection; the chosen units are recorded. |
