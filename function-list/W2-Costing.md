# W2 Costing and roll-up: NBSP

> DBIZ2 Group C. Function list and effective priorities exactly as in the team's costing sheet (29 L2, 54 L3). FR-02-07 is not yet in W1 or W4: add it there so the three files agree.

Lesson 5 (block W2). Fully derived from [W1](w1-function-list-2.md), the effective priorities in [W4](w4-priorities-v1.md) and the class Parameters:

- Person-hours = base hours x AI leverage factor + base hours x verification factor.
- AI leverage: FE 0.35, BE 0.7, INT 1.0. Verification factor: 0.15 when the EARS criterion is precise, 0.4 when it is vague.
- Rate: FE Junior 200.000 VND/h; BE and INT Senior 300.000 VND/h.

## Cost per L3 work item

| L3 ID | Feature | L2 ID | Layer | Base hours | AI factor | AI-adj. hours | Verif. factor | Verif. hours | Person-hours | Band | Rate | Cost (VND) | Current priority | Effective priority | Risk order | Task key |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| FR-01-01-FE | F-01 | FR-01-01 | FE | 8 | 0.35 | 2.8 | 0.15 | 1.2 | 4.0 | Junior | 200.000 | 800.000 | Must | Must | 3 | 3009 |
| FR-01-01-BE | F-01 | FR-01-01 | BE | 8 | 0.70 | 5.6 | 0.15 | 1.2 | 6.8 | Senior | 300.000 | 2.040.000 | Must | Must | 2 | 2010 |
| FR-01-02-FE | F-01 | FR-01-02 | FE | 4 | 0.35 | 1.4 | 0.15 | 0.6 | 2.0 | Junior | 200.000 | 400.000 | Must | Must | 3 | 3011 |
| FR-01-02-BE | F-01 | FR-01-02 | BE | 8 | 0.70 | 5.6 | 0.15 | 1.2 | 6.8 | Senior | 300.000 | 2.040.000 | Must | Must | 2 | 2012 |
| FR-01-03-FE | F-01 | FR-01-03 | FE | 8 | 0.35 | 2.8 | 0.15 | 1.2 | 4.0 | Junior | 200.000 | 800.000 | Must | Must | 3 | 3013 |
| FR-01-03-BE | F-01 | FR-01-03 | BE | 16 | 0.70 | 11.2 | 0.15 | 2.4 | 13.6 | Senior | 300.000 | 4.080.000 | Must | Must | 2 | 2014 |
| FR-01-03-INT | F-01 | FR-01-03 | INT | 8 | 1.00 | 8.0 | 0.15 | 1.2 | 9.2 | Senior | 300.000 | 2.760.000 | Must | Must | 1 | 1015 |
| FR-01-04-FE | F-01 | FR-01-04 | FE | 4 | 0.35 | 1.4 | 0.15 | 0.6 | 2.0 | Junior | 200.000 | 400.000 | Must | Must | 3 | 3016 |
| FR-01-04-BE | F-01 | FR-01-04 | BE | 4 | 0.70 | 2.8 | 0.15 | 0.6 | 3.4 | Senior | 300.000 | 1.020.000 | Must | Must | 2 | 2017 |
| FR-01-05-FE | F-01 | FR-01-05 | FE | 4 | 0.35 | 1.4 | 0.15 | 0.6 | 2.0 | Junior | 200.000 | 400.000 | Must | Must | 3 | 3018 |
| FR-01-05-BE | F-01 | FR-01-05 | BE | 8 | 0.70 | 5.6 | 0.15 | 1.2 | 6.8 | Senior | 300.000 | 2.040.000 | Must | Must | 2 | 2019 |
| FR-01-06-FE | F-01 | FR-01-06 | FE | 4 | 0.35 | 1.4 | 0.15 | 0.6 | 2.0 | Junior | 200.000 | 400.000 | Must | Must | 3 | 3020 |
| FR-01-06-BE | F-01 | FR-01-06 | BE | 4 | 0.70 | 2.8 | 0.15 | 0.6 | 3.4 | Senior | 300.000 | 1.020.000 | Must | Must | 2 | 2021 |
| FR-01-07-BE | F-01 | FR-01-07 | BE | 4 | 0.70 | 2.8 | 0.15 | 0.6 | 3.4 | Senior | 300.000 | 1.020.000 | Must | Must | 2 | 2022 |
| FR-01-08-FE | F-01 | FR-01-08 | FE | 4 | 0.35 | 1.4 | 0.40 | 1.6 | 3.0 | Junior | 200.000 | 600.000 | Must | Must | 3 | 3023 |
| FR-01-08-BE | F-01 | FR-01-08 | BE | 4 | 0.70 | 2.8 | 0.40 | 1.6 | 4.4 | Senior | 300.000 | 1.320.000 | Must | Must | 2 | 2024 |
| FR-01-09-FE | F-01 | FR-01-09 | FE | 4 | 0.35 | 1.4 | 0.40 | 1.6 | 3.0 | Junior | 200.000 | 600.000 | Must | Should | 3 | |
| FR-01-09-BE | F-01 | FR-01-09 | BE | 4 | 0.70 | 2.8 | 0.40 | 1.6 | 4.4 | Senior | 300.000 | 1.320.000 | Must | Should | 2 | |
| FR-02-01-FE | F-02 | FR-02-01 | FE | 8 | 0.35 | 2.8 | 0.15 | 1.2 | 4.0 | Junior | 200.000 | 800.000 | Must | Must | 3 | 3027 |
| FR-02-01-BE | F-02 | FR-02-01 | BE | 8 | 0.70 | 5.6 | 0.15 | 1.2 | 6.8 | Senior | 300.000 | 2.040.000 | Must | Must | 2 | 2028 |
| FR-02-02-FE | F-02 | FR-02-02 | FE | 8 | 0.35 | 2.8 | 0.15 | 1.2 | 4.0 | Junior | 200.000 | 800.000 | Must | Should | 3 | |
| FR-02-02-BE | F-02 | FR-02-02 | BE | 8 | 0.70 | 5.6 | 0.15 | 1.2 | 6.8 | Senior | 300.000 | 2.040.000 | Must | Should | 2 | |
| FR-02-03-FE | F-02 | FR-02-03 | FE | 4 | 0.35 | 1.4 | 0.40 | 1.6 | 3.0 | Junior | 200.000 | 600.000 | Should | Could | 3 | |
| FR-02-03-BE | F-02 | FR-02-03 | BE | 8 | 0.70 | 5.6 | 0.40 | 3.2 | 8.8 | Senior | 300.000 | 2.640.000 | Should | Could | 2 | |
| FR-02-04-FE | F-02 | FR-02-04 | FE | 4 | 0.35 | 1.4 | 0.15 | 0.6 | 2.0 | Junior | 200.000 | 400.000 | Must | Could | 3 | |
| FR-02-04-BE | F-02 | FR-02-04 | BE | 8 | 0.70 | 5.6 | 0.15 | 1.2 | 6.8 | Senior | 300.000 | 2.040.000 | Must | Could | 2 | |
| FR-02-05-BE | F-02 | FR-02-05 | BE | 4 | 0.70 | 2.8 | 0.15 | 0.6 | 3.4 | Senior | 300.000 | 1.020.000 | Must | Must | 2 | 2035 |
| FR-02-05-INT | F-02 | FR-02-05 | INT | 8 | 1.00 | 8.0 | 0.15 | 1.2 | 9.2 | Senior | 300.000 | 2.760.000 | Must | Must | 1 | 1036 |
| FR-02-07-FE | F-02 | FR-02-07 | FE | 4 | 0.35 | 1.4 | 0.15 | 0.6 | 2.0 | Junior | 200.000 | 400.000 | Should | Should | 3 | |
| FR-02-07-BE | F-02 | FR-02-07 | BE | 4 | 0.70 | 2.8 | 0.15 | 0.6 | 3.4 | Senior | 300.000 | 1.020.000 | Should | Should | 2 | |
| FR-02-06-FE | F-02 | FR-02-06 | FE | 4 | 0.35 | 1.4 | 0.15 | 0.6 | 2.0 | Junior | 200.000 | 400.000 | Must | Should | 3 | |
| FR-02-06-BE | F-02 | FR-02-06 | BE | 4 | 0.70 | 2.8 | 0.15 | 0.6 | 3.4 | Senior | 300.000 | 1.020.000 | Must | Should | 2 | |
| FR-03-01-FE | F-03 | FR-03-01 | FE | 8 | 0.35 | 2.8 | 0.15 | 1.2 | 4.0 | Junior | 200.000 | 800.000 | Must | Could | 3 | |
| FR-03-01-BE | F-03 | FR-03-01 | BE | 8 | 0.70 | 5.6 | 0.15 | 1.2 | 6.8 | Senior | 300.000 | 2.040.000 | Must | Could | 2 | |
| FR-03-02-FE | F-03 | FR-03-02 | FE | 8 | 0.35 | 2.8 | 0.15 | 1.2 | 4.0 | Junior | 200.000 | 800.000 | Must | Should | 3 | |
| FR-03-02-BE | F-03 | FR-03-02 | BE | 16 | 0.70 | 11.2 | 0.15 | 2.4 | 13.6 | Senior | 300.000 | 4.080.000 | Must | Should | 2 | |
| FR-03-03-FE | F-03 | FR-03-03 | FE | 4 | 0.35 | 1.4 | 0.15 | 0.6 | 2.0 | Junior | 200.000 | 400.000 | Must | Should | 3 | |
| FR-03-03-BE | F-03 | FR-03-03 | BE | 8 | 0.70 | 5.6 | 0.15 | 1.2 | 6.8 | Senior | 300.000 | 2.040.000 | Must | Should | 2 | |
| FR-03-04-FE | F-03 | FR-03-04 | FE | 8 | 0.35 | 2.8 | 0.15 | 1.2 | 4.0 | Junior | 200.000 | 800.000 | Must | Could | 3 | |
| FR-03-04-BE | F-03 | FR-03-04 | BE | 8 | 0.70 | 5.6 | 0.15 | 1.2 | 6.8 | Senior | 300.000 | 2.040.000 | Must | Could | 2 | |
| FR-03-05-BE | F-03 | FR-03-05 | BE | 8 | 0.70 | 5.6 | 0.15 | 1.2 | 6.8 | Senior | 300.000 | 2.040.000 | Must | Must | 2 | 2047 |
| FR-03-05-INT | F-03 | FR-03-05 | INT | 8 | 1.00 | 8.0 | 0.15 | 1.2 | 9.2 | Senior | 300.000 | 2.760.000 | Must | Must | 1 | 1048 |
| FR-03-06-FE | F-03 | FR-03-06 | FE | 8 | 0.35 | 2.8 | 0.15 | 1.2 | 4.0 | Junior | 200.000 | 800.000 | Must | Must | 3 | 3049 |
| FR-03-06-BE | F-03 | FR-03-06 | BE | 8 | 0.70 | 5.6 | 0.15 | 1.2 | 6.8 | Senior | 300.000 | 2.040.000 | Must | Must | 2 | 2050 |
| FR-03-07-FE | F-03 | FR-03-07 | FE | 8 | 0.35 | 2.8 | 0.15 | 1.2 | 4.0 | Junior | 200.000 | 800.000 | Must | Must | 3 | 3051 |
| FR-03-07-BE | F-03 | FR-03-07 | BE | 8 | 0.70 | 5.6 | 0.15 | 1.2 | 6.8 | Senior | 300.000 | 2.040.000 | Must | Must | 2 | 2052 |
| FR-03-08-BE | F-03 | FR-03-08 | BE | 4 | 0.70 | 2.8 | 0.15 | 0.6 | 3.4 | Senior | 300.000 | 1.020.000 | Must | Must | 2 | 2053 |
| FR-04-01-FE | F-04 | FR-04-01 | FE | 4 | 0.35 | 1.4 | 0.15 | 0.6 | 2.0 | Junior | 200.000 | 400.000 | Must | Must | 3 | 3054 |
| FR-04-01-BE | F-04 | FR-04-01 | BE | 8 | 0.70 | 5.6 | 0.15 | 1.2 | 6.8 | Senior | 300.000 | 2.040.000 | Must | Must | 2 | 2055 |
| FR-04-02-FE | F-04 | FR-04-02 | FE | 4 | 0.35 | 1.4 | 0.15 | 0.6 | 2.0 | Junior | 200.000 | 400.000 | Must | Must | 3 | 3056 |
| FR-04-02-BE | F-04 | FR-04-02 | BE | 4 | 0.70 | 2.8 | 0.15 | 0.6 | 3.4 | Senior | 300.000 | 1.020.000 | Must | Must | 2 | 2057 |
| FR-04-03-BE | F-04 | FR-04-03 | BE | 8 | 0.70 | 5.6 | 0.15 | 1.2 | 6.8 | Senior | 300.000 | 2.040.000 | Must | Must | 2 | 2058 |
| FR-04-04-BE | F-04 | FR-04-04 | BE | 8 | 0.70 | 5.6 | 0.15 | 1.2 | 6.8 | Senior | 300.000 | 2.040.000 | Must | Could | 2 | |
| FR-04-05-INT | F-04 | FR-04-05 | INT | 8 | 1.00 | 8.0 | 0.15 | 1.2 | 9.2 | Senior | 300.000 | 2.760.000 | Should | Could | 1 | |

## Roll-up per feature

| Feature | Name | L2 | L3 | Base h | Must p-h | Should p-h | Could p-h | In-scope p-h | In-scope cost (VND) | Must share | Transactional FP | Base h per FP |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| F-01 | Blood Ordering & Fulfillment | 9 | 18 | 108 | 76.8 | 7.4 | 0 | 84.2 | 23.060.000 | 91% | 20 | 5.4 |
| F-02 | Inventory Monitoring & Usage | 7 | 14 | 84 | 23.4 | 21.6 | 20.6 | 65.6 | 17.980.000 | 36% | 26 | 3.2 |
| F-03 | Clinical Demand & Forecasting | 8 | 15 | 120 | 41.0 | 26.4 | 21.6 | 89.0 | 24.500.000 | 46% | 31 | 3.9 |
| F-04 | Access & Authorization | 5 | 7 | 44 | 21.0 | 0 | 16.0 | 37.0 | 10.700.000 | 57% | 8 | 5.5 |
| **Project** | | **29** | **54** | **356** | **162.2** | **55.4** | **58.2** | **275.8** | **76.240.000** | **59%** | **85** | **4.2** |

## The three headline figures

| Scenario | Scope | Person-hours | Cost (VND) | Meaning |
|---|---|---|---|---|
| Committed | Must only | 162.2 | 45.160.000 | What the team guarantees to deliver |
| Expected | Must + Should | 217.6 | 60.080.000 | The figure given to the client; the base cash flow in W6 |
| Ceiling | Must + Should + Could | 275.8 | 76.240.000 | If the Could pool is never consumed |

## Threshold check (after W4)

| Indicator | Value | Threshold | Verdict |
|---|---|---|---|
| Must share of in-scope effort | 58.8% | at most 60% | PASS |
| Could share (contingency pool) | 21.1% | target 20% | PASS |
| In-scope effort vs team capacity | 275.8 p-h | at most 300 p-h | PASS |
| Hidden technical work, share of effort | 31.3% | at least 10% | PASS |
| L3 rows above 16 hours | 0 | 0 | PASS |
| Feature count | 4 | 3 to 6 | PASS |
| Function-point cross-check: (85 transactional FP + 57 data FP) x 3 h = 426 h, plus 64 h of NC work = 490 h vs 356 bottom-up base h | 27.3% apart | at most 30% | PASS |

Assumptions behind two figures: FR-02-07 is counted as an external input (EI, 4 FP) and is not flagged as hidden work, because it has no W1 row yet. If the W1 row sets a different type, update the FP line and the hidden-work share (counting it as hidden gives 33.3%).

Three observations the team carries into W4:

1. Must sits at 58.8% of the effort, close to the 60% line: only about 8 p-h of new Must work fits before the test fails, so any new Must needs a Must to move down.
2. The Could pool is only 1.1 points above its 20% target. Raising any Could to Should breaks it: for example FR-03-04 (10.8 p-h) to Should lowers the Could share to 17.2%, as open question 4 of AD-UC-13 already warns.
3. In-scope effort of 275.8 p-h leaves only 24.2 p-h (8.1%) of headroom against the 300 p-h capacity, and the function-point variance (27.3%) is close to the 30% limit. The bottom-up total is the lower side of the range, so plan on the Expected scenario (217.6 p-h) and keep the Could items as the buffer.
