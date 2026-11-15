# W2 - Costing, item by item

| Mục | Nội dung |
|---|---|
| **INPUT** | Every L3 row from W1, plus the rates and factors from Parameters. Nothing is typed on this sheet. |
| **OUTPUT** | Cost of each L3 work item, a subtotal per L2 function, and an independent function-point cross-check of the whole estimate. |
| **PROCEDURE** | The sheet is fully derived. Read it in this order:<br>1. Identify the most expensive L3 row and whether cost is driven by size, layer, or verification.<br>2. Compare rows with the same size but different layers to see the AI leverage effect.<br>3. Find rows where EARS precise? = No and read the higher verification hours.<br>4. Compare the bottom-up and function-point totals at the bottom and explain any variance. |
| **COST LOGIC** | Person-hours = Base hours × AI factor + Base hours × verification factor. Cost = Person-hours × hourly rate. Compare Total Person-hours (J129) with Parameters!B45 team capacity. |

## 1. Chi phí từng hạng mục L3

| L3 ID | Feature | L2 ID | Layer | Base hours | AI factor | AI-adj. hours | Verif. factor | Verif. hours | Person-hours | Band | Rate | Cost (VND) | Current priority | Effective priority | Risk order | Task key |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| FR-01-01-FE | F-01 | FR-01-01 | FE | 8.0 | 0.35 | 2.80 | 0.15 | 1.20 | 4.0 | Junior | 200,000 | 800,000 | Must | Must | 3 | 3009 |
| FR-01-01-BE | F-01 | FR-01-01 | BE | 8.0 | 0.70 | 5.60 | 0.15 | 1.20 | 6.8 | Senior | 300,000 | 2,040,000 | Must | Must | 2 | 2010 |
| FR-01-02-FE | F-01 | FR-01-02 | FE | 4.0 | 0.35 | 1.40 | 0.15 | 0.60 | 2.0 | Junior | 200,000 | 400,000 | Must | Must | 3 | 3011 |
| FR-01-02-BE | F-01 | FR-01-02 | BE | 8.0 | 0.70 | 5.60 | 0.15 | 1.20 | 6.8 | Senior | 300,000 | 2,040,000 | Must | Must | 2 | 2012 |
| FR-01-03-FE | F-01 | FR-01-03 | FE | 8.0 | 0.35 | 2.80 | 0.15 | 1.20 | 4.0 | Junior | 200,000 | 800,000 | Must | Must | 3 | 3013 |
| FR-01-03-BE | F-01 | FR-01-03 | BE | 16.0 | 0.70 | 11.20 | 0.15 | 2.40 | 13.6 | Senior | 300,000 | 4,080,000 | Must | Must | 2 | 2014 |
| FR-01-03-INT | F-01 | FR-01-03 | INT | 8.0 | 1.00 | 8.00 | 0.15 | 1.20 | 9.2 | Senior | 300,000 | 2,760,000 | Must | Must | 1 | 1015 |
| FR-01-04-FE | F-01 | FR-01-04 | FE | 4.0 | 0.35 | 1.40 | 0.15 | 0.60 | 2.0 | Junior | 200,000 | 400,000 | Must | Could | 3 |  |
| FR-01-04-BE | F-01 | FR-01-04 | BE | 4.0 | 0.70 | 2.80 | 0.15 | 0.60 | 3.4 | Senior | 300,000 | 1,020,000 | Must | Could | 2 |  |
| FR-01-05-FE | F-01 | FR-01-05 | FE | 4.0 | 0.35 | 1.40 | 0.15 | 0.60 | 2.0 | Junior | 200,000 | 400,000 | Must | Must | 3 | 3018 |
| FR-01-05-BE | F-01 | FR-01-05 | BE | 8.0 | 0.70 | 5.60 | 0.15 | 1.20 | 6.8 | Senior | 300,000 | 2,040,000 | Must | Must | 2 | 2019 |
| FR-01-06-FE | F-01 | FR-01-06 | FE | 4.0 | 0.35 | 1.40 | 0.15 | 0.60 | 2.0 | Junior | 200,000 | 400,000 | Must | Must | 3 | 3020 |
| FR-01-06-BE | F-01 | FR-01-06 | BE | 4.0 | 0.70 | 2.80 | 0.15 | 0.60 | 3.4 | Senior | 300,000 | 1,020,000 | Must | Must | 2 | 2021 |
| FR-01-07-BE | F-01 | FR-01-07 | BE | 4.0 | 0.70 | 2.80 | 0.15 | 0.60 | 3.4 | Senior | 300,000 | 1,020,000 | Must | Must | 2 | 2022 |
| FR-01-08-FE | F-01 | FR-01-08 | FE | 4.0 | 0.35 | 1.40 | 0.40 | 1.60 | 3.0 | Junior | 200,000 | 600,000 | Must | Must | 3 | 3023 |
| FR-01-08-BE | F-01 | FR-01-08 | BE | 4.0 | 0.70 | 2.80 | 0.40 | 1.60 | 4.4 | Senior | 300,000 | 1,320,000 | Must | Must | 2 | 2024 |
| FR-01-09-FE | F-01 | FR-01-09 | FE | 4.0 | 0.35 | 1.40 | 0.40 | 1.60 | 3.0 | Junior | 200,000 | 600,000 | Must | Could | 3 |  |
| FR-01-09-BE | F-01 | FR-01-09 | BE | 4.0 | 0.70 | 2.80 | 0.40 | 1.60 | 4.4 | Senior | 300,000 | 1,320,000 | Must | Could | 2 |  |
| FR-02-01-FE | F-02 | FR-02-01 | FE | 8.0 | 0.35 | 2.80 | 0.15 | 1.20 | 4.0 | Junior | 200,000 | 800,000 | Must | Must | 3 | 3027 |
| FR-02-01-BE | F-02 | FR-02-01 | BE | 8.0 | 0.70 | 5.60 | 0.15 | 1.20 | 6.8 | Senior | 300,000 | 2,040,000 | Must | Must | 2 | 2028 |
| FR-02-02-FE | F-02 | FR-02-02 | FE | 8.0 | 0.35 | 2.80 | 0.15 | 1.20 | 4.0 | Junior | 200,000 | 800,000 | Must | Should | 3 |  |
| FR-02-02-BE | F-02 | FR-02-02 | BE | 8.0 | 0.70 | 5.60 | 0.15 | 1.20 | 6.8 | Senior | 300,000 | 2,040,000 | Must | Should | 2 |  |
| FR-02-03-FE | F-02 | FR-02-03 | FE | 4.0 | 0.35 | 1.40 | 0.40 | 1.60 | 3.0 | Junior | 200,000 | 600,000 | Should | Could | 3 |  |
| FR-02-03-BE | F-02 | FR-02-03 | BE | 8.0 | 0.70 | 5.60 | 0.40 | 3.20 | 8.8 | Senior | 300,000 | 2,640,000 | Should | Could | 2 |  |
| FR-02-04-FE | F-02 | FR-02-04 | FE | 4.0 | 0.35 | 1.40 | 0.15 | 0.60 | 2.0 | Junior | 200,000 | 400,000 | Must | Could | 3 |  |
| FR-02-04-BE | F-02 | FR-02-04 | BE | 8.0 | 0.70 | 5.60 | 0.15 | 1.20 | 6.8 | Senior | 300,000 | 2,040,000 | Must | Could | 2 |  |
| FR-02-05-BE | F-02 | FR-02-05 | BE | 4.0 | 0.70 | 2.80 | 0.15 | 0.60 | 3.4 | Senior | 300,000 | 1,020,000 | Must | Must | 2 | 2035 |
| FR-02-05-INT | F-02 | FR-02-05 | INT | 8.0 | 1.00 | 8.00 | 0.15 | 1.20 | 9.2 | Senior | 300,000 | 2,760,000 | Must | Must | 1 | 1036 |
| FR-02-06-FE | F-02 | FR-02-06 | FE | 4.0 | 0.35 | 1.40 | 0.15 | 0.60 | 2.0 | Junior | 200,000 | 400,000 | Must | Should | 3 |  |
| FR-02-06-BE | F-02 | FR-02-06 | BE | 4.0 | 0.70 | 2.80 | 0.15 | 0.60 | 3.4 | Senior | 300,000 | 1,020,000 | Must | Should | 2 |  |
| FR-03-01-FE | F-03 | FR-03-01 | FE | 8.0 | 0.35 | 2.80 | 0.15 | 1.20 | 4.0 | Junior | 200,000 | 800,000 | Must | Should | 3 |  |
| FR-03-01-BE | F-03 | FR-03-01 | BE | 8.0 | 0.70 | 5.60 | 0.15 | 1.20 | 6.8 | Senior | 300,000 | 2,040,000 | Must | Should | 2 |  |
| FR-03-02-FE | F-03 | FR-03-02 | FE | 8.0 | 0.35 | 2.80 | 0.15 | 1.20 | 4.0 | Junior | 200,000 | 800,000 | Must | Should | 3 |  |
| FR-03-02-BE | F-03 | FR-03-02 | BE | 16.0 | 0.70 | 11.20 | 0.15 | 2.40 | 13.6 | Senior | 300,000 | 4,080,000 | Must | Should | 2 |  |
| FR-03-03-FE | F-03 | FR-03-03 | FE | 4.0 | 0.35 | 1.40 | 0.15 | 0.60 | 2.0 | Junior | 200,000 | 400,000 | Must | Should | 3 |  |
| FR-03-03-BE | F-03 | FR-03-03 | BE | 8.0 | 0.70 | 5.60 | 0.15 | 1.20 | 6.8 | Senior | 300,000 | 2,040,000 | Must | Should | 2 |  |
| FR-03-04-FE | F-03 | FR-03-04 | FE | 8.0 | 0.35 | 2.80 | 0.15 | 1.20 | 4.0 | Junior | 200,000 | 800,000 | Must | Could | 3 |  |
| FR-03-04-BE | F-03 | FR-03-04 | BE | 8.0 | 0.70 | 5.60 | 0.15 | 1.20 | 6.8 | Senior | 300,000 | 2,040,000 | Must | Could | 2 |  |
| FR-03-05-BE | F-03 | FR-03-05 | BE | 8.0 | 0.70 | 5.60 | 0.15 | 1.20 | 6.8 | Senior | 300,000 | 2,040,000 | Must | Must | 2 | 2047 |
| FR-03-05-INT | F-03 | FR-03-05 | INT | 8.0 | 1.00 | 8.00 | 0.15 | 1.20 | 9.2 | Senior | 300,000 | 2,760,000 | Must | Must | 1 | 1048 |
| FR-03-06-FE | F-03 | FR-03-06 | FE | 8.0 | 0.35 | 2.80 | 0.15 | 1.20 | 4.0 | Junior | 200,000 | 800,000 | Must | Must | 3 | 3049 |
| FR-03-06-BE | F-03 | FR-03-06 | BE | 8.0 | 0.70 | 5.60 | 0.15 | 1.20 | 6.8 | Senior | 300,000 | 2,040,000 | Must | Must | 2 | 2050 |
| FR-03-07-FE | F-03 | FR-03-07 | FE | 8.0 | 0.35 | 2.80 | 0.15 | 1.20 | 4.0 | Junior | 200,000 | 800,000 | Must | Must | 3 | 3051 |
| FR-03-07-BE | F-03 | FR-03-07 | BE | 8.0 | 0.70 | 5.60 | 0.15 | 1.20 | 6.8 | Senior | 300,000 | 2,040,000 | Must | Must | 2 | 2052 |
| FR-03-08-BE | F-03 | FR-03-08 | BE | 4.0 | 0.70 | 2.80 | 0.15 | 0.60 | 3.4 | Senior | 300,000 | 1,020,000 | Must | Must | 2 | 2053 |
| FR-04-01-FE | F-04 | FR-04-01 | FE | 4.0 | 0.35 | 1.40 | 0.15 | 0.60 | 2.0 | Junior | 200,000 | 400,000 | Must | Must | 3 | 3054 |
| FR-04-01-BE | F-04 | FR-04-01 | BE | 8.0 | 0.70 | 5.60 | 0.15 | 1.20 | 6.8 | Senior | 300,000 | 2,040,000 | Must | Must | 2 | 2055 |
| FR-04-02-FE | F-04 | FR-04-02 | FE | 4.0 | 0.35 | 1.40 | 0.15 | 0.60 | 2.0 | Junior | 200,000 | 400,000 | Must | Must | 3 | 3056 |
| FR-04-02-BE | F-04 | FR-04-02 | BE | 4.0 | 0.70 | 2.80 | 0.15 | 0.60 | 3.4 | Senior | 300,000 | 1,020,000 | Must | Must | 2 | 2057 |
| FR-04-03-BE | F-04 | FR-04-03 | BE | 8.0 | 0.70 | 5.60 | 0.15 | 1.20 | 6.8 | Senior | 300,000 | 2,040,000 | Must | Must | 2 | 2058 |
| FR-04-04-BE | F-04 | FR-04-04 | BE | 8.0 | 0.70 | 5.60 | 0.15 | 1.20 | 6.8 | Senior | 300,000 | 2,040,000 | Must | Could | 2 |  |
| FR-04-05-INT | F-04 | FR-04-05 | INT | 8.0 | 1.00 | 8.00 | 0.15 | 1.20 | 9.2 | Senior | 300,000 | 2,760,000 | Should | Could | 1 |  |

**TOTAL:** Base hours **348.0** · AI-adj. hours **211.2** · Verif. hours **59.2** · Person-hours **270.4** · Cost **74,820,000 VND**

## 2. Cost per L2 function — the client-facing 'cost of one function'

| L2 ID | L2 function name | Feature | FP type | L3 items | Person-hours | Cost (VND) | Current priority |
|---|---|---|---|---|---|---|---|
| FR-01-01 | Request blood provision | F-01 | EI | 2 | 10.8 | 2,840,000 | Must |
| FR-01-02 | View request queue | F-01 | EQ | 2 | 8.8 | 2,440,000 | Must |
| FR-01-03 | Approve and dispatch blood order | F-01 | EI | 3 | 26.8 | 7,640,000 | Must |
| FR-01-04 | View shipment details | F-01 | EQ | 2 | 5.4 | 1,420,000 | Must |
| FR-01-05 | Confirm blood received | F-01 | EI | 2 | 8.8 | 2,440,000 | Must |
| FR-01-06 | Reject invalid blood request with reasons | F-01 | NC | 2 | 5.4 | 1,420,000 | Must |
| FR-01-07 | Reject confirmation of a shipment that is not Dispatched | F-01 | NC | 1 | 3.4 | 1,020,000 | Must |
| FR-01-08 | Block dispatch when eligible stock is insufficient | F-01 | NC | 2 | 7.4 | 1,920,000 | Must |
| FR-01-09 | Record blood order non-approval with reason | F-01 | NC | 2 | 7.4 | 1,920,000 | Must |
| FR-02-01 | Record blood usage | F-02 | EI | 2 | 10.8 | 2,840,000 | Must |
| FR-02-02 | View role-scoped inventory dashboard | F-02 | EQ | 2 | 10.8 | 2,840,000 | Must |
| FR-02-03 | View near-expiry blood units | F-02 | EO | 2 | 11.8 | 3,240,000 | Should |
| FR-02-04 | Receive below-threshold alert | F-02 | EO | 2 | 8.8 | 2,440,000 | Must |
| FR-02-05 | Synchronise NBC inventory from Blood Management Software | F-02 | EI | 2 | 12.6 | 3,780,000 | Must |
| FR-02-06 | Reject invalid or excessive blood usage record with reasons | F-02 | NC | 2 | 5.4 | 1,420,000 | Must |
| FR-03-01 | Log scheduled or periodic blood demand | F-03 | EI | 2 | 10.8 | 2,840,000 | Must |
| FR-03-02 | Review clinical demand data and anomaly flags | F-03 | EO | 2 | 17.6 | 4,880,000 | Must |
| FR-03-03 | Record demand-data validation decision | F-03 | EI | 2 | 8.8 | 2,440,000 | Must |
| FR-03-04 | Respond to data verification request | F-03 | EI | 2 | 10.8 | 2,840,000 | Must |
| FR-03-05 | Generate draft blood-demand forecast | F-03 | EO | 2 | 16.0 | 4,800,000 | Must |
| FR-03-06 | Review and approve blood forecast | F-03 | EI | 2 | 10.8 | 2,840,000 | Must |
| FR-03-07 | View validated blood forecast | F-03 | EO | 2 | 10.8 | 2,840,000 | Must |
| FR-03-08 | Close validation period at forecast cut-off | F-03 | NC | 1 | 3.4 | 1,020,000 | Must |
| FR-04-01 | Sign in with hospital account | F-04 | EI | 2 | 8.8 | 2,440,000 | Must |
| FR-04-02 | Sign in with NBC account | F-04 | EI | 2 | 5.4 | 1,420,000 | Must |
| FR-04-03 | Enforce role-based authorisation | F-04 | NC | 1 | 6.8 | 2,040,000 | Must |
| FR-04-04 | Maintain audit trail for validation and forecast decisions | F-04 | NC | 1 | 6.8 | 2,040,000 | Must |
| FR-04-05 | Operate deployment and regression-test gate | F-04 | NC | 1 | 9.2 | 2,760,000 | Should |

## 3. Function-point cross-check — a second estimate from a different direction

| Chỉ tiêu | Giá trị | Ghi chú |
|---|---|---|
| Transactional function points (EI / EO / EQ) | 81.0 | Counted once per L2 row, from W1 column Q |
| Data function points (ILF / EIF) | 57.0 | From W0 Table 4 — counted once for the whole product |
| Unadjusted function points | 138.0 | Transactional + data |
| Hours per FP (class parameter) | 3.0 | Pre-AI gross hours |
| Functional work — function-point estimate (h) | 414.0 | What function points can see |
| Non-functional (NC) work — bottom-up (h) | 64.0 | The 0 function point problem: concurrency, authorisation, deployment. IFPUG needs a separate method (SNAP) for this |
| Function-point basis total (h) | 478.0 | Functional + non-functional |
| Bottom-up basis total (h) | 348.0 | Sum of W1 column L |
| Variance | (27.2%) | Within tolerance - the two methods agree |
| Active estimation basis | Bottom-up | Set on Parameters section 9 |
| Base hours actually in use | 348.0 | What the cost columns are built from right now |

> Read it this way: function points size the FUNCTIONAL work only, so the NC line is added separately rather than folded in. Switching the basis on Parameters section 9 changes which of the two totals drives every cost in the workbook; NC rows stay on bottom-up either way, because there is no function-point number for them to come from.
