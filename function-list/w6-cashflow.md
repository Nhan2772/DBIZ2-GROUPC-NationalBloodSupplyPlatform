# W6 Financial Appraisal: Cash Flow, NPV, IRR and Payback

Inputs: scenario costs from W5_Handover (Must / Must+Should / full scope), per-feature effort from W2_Rollup.

> **Status (21/11/2026).**
> - **Cost side: updated.** The investment uses the W5 v2 scenario costs (45.16 / 60.08 / 76.24 million VND, decision log D-25, previously 43.74 / 58.16 / 74.82). Running cost at 12% of build: 5.419.200 / 7.209.600 / 9.148.800 VND per year (previously 5.248.800 / 6.979.200 / 8.978.400).
> - **Benefit side: not yet recalculated.** The Benefits rows are still the v1 figures. They cannot be recalculated yet because "Section 1.6" (the benefit derivation the Results table refers to) does not exist, and the feature coverage behind the benefits has changed since v1 (for example FR-01-04 raised to Must, FR-02-04 lowered to Could). Write Section 1.6 and recalculate the benefits last, once W1 to W5 are final.
> - **Other one-time cost:** still empty. Enter hospital onboarding (opening balance import, training) there once it is quoted.
> - All results below are therefore **provisional**.

## 1. Assumptions

| Assumption | Value | Unit | Source / note |
|---|---|---|---|
| Discount rate (hurdle rate) | 12.0% | % per year | Retained from the previous W6 version. Replace with the course / NBC hurdle rate if one is specified. |
| Evaluation horizon | 5 | years (1 to 5) | Years beyond the horizon are switched off in section 2 (benefits and running cost become 0). |
| Annual running cost as % of build cost | 12.0% | % of build / year | Retained from the previous W6. Hosting, support, maintenance. Starts in Year 1. Not a supplier quote. |
| Contingency rate on the Must set | 0.0% | % of Must cost | W5 section 2: 0%, because the Could pool is already the scope contingency (Parameters §8). No double count. |
| Must-set cost (contingency base) | 45.160.000 | VND | Committed scenario cost from W2_Rollup (v2). |
| Other one-time cost (rollout, migration, training) | - | VND | Not in W2/W5. Enter hospital onboarding, data migration, training, security audit if quoted. Added to Year 0. |
| Benefit realisation factor (haircut on modelled benefits) | 100.0% | % of modelled | 100% = benefits as modelled below. Lower it for a conservative view (see also section 4). |

## Pessimistic: Must only

| Line item | Year 0 | Year 1 | Year 2 | Year 3 | Year 4 | Year 5 |
|---|---|---|---|---|---|---|
| Benefits (v1, not yet recalculated) | 0 | 13.524.969 | 27.049.937 | 38.320.744 | 45.083.229 | 45.083.229 |
| Running cost | 0 | -5.419.200 | -5.419.200 | -5.419.200 | -5.419.200 | -5.419.200 |
| Investment (build + contingency + other) | -45.160.000 | 0 | 0 | 0 | 0 | 0 |
| Net cash flow | -45.160.000 | 8.105.769 | 21.630.737 | 32.901.544 | 39.664.029 | 39.664.029 |
| Cumulative net cash flow | -45.160.000 | -37.054.231 | -15.423.494 | 17.478.050 | 57.142.079 | 96.806.108 |
| PV of net cash flow | -45.160.000 | 7.237.294 | 17.243.891 | 23.418.669 | 25.207.207 | 22.506.435 |

## Base: Must + Should

| Line item | Year 0 | Year 1 | Year 2 | Year 3 | Year 4 | Year 5 |
|---|---|---|---|---|---|---|
| Benefits (v1, not yet recalculated) | 0 | 17.585.602 | 35.171.203 | 49.825.872 | 58.618.672 | 58.618.672 |
| Running cost | 0 | -7.209.600 | -7.209.600 | -7.209.600 | -7.209.600 | -7.209.600 |
| Investment (build + contingency + other) | -60.080.000 | 0 | 0 | 0 | 0 | 0 |
| Net cash flow | -60.080.000 | 10.376.002 | 27.961.603 | 42.616.272 | 51.409.072 | 51.409.072 |
| Cumulative net cash flow | -60.080.000 | -49.703.998 | -21.742.395 | 20.873.877 | 72.282.949 | 123.692.021 |
| PV of net cash flow | -60.080.000 | 9.264.288 | 22.290.819 | 30.333.421 | 32.671.395 | 29.170.888 |

## Optimistic: full scope

| Line item | Year 0 | Year 1 | Year 2 | Year 3 | Year 4 | Year 5 |
|---|---|---|---|---|---|---|
| Benefits (v1, not yet recalculated) | 0 | 22.392.000 | 44.784.000 | 63.444.000 | 74.640.000 | 74.640.000 |
| Running cost | 0 | -9.148.800 | -9.148.800 | -9.148.800 | -9.148.800 | -9.148.800 |
| Investment (build + contingency + other) | -76.240.000 | 0 | 0 | 0 | 0 | 0 |
| Net cash flow | -76.240.000 | 13.243.200 | 35.635.200 | 54.295.200 | 65.491.200 | 65.491.200 |
| Cumulative net cash flow | -76.240.000 | -62.996.800 | -27.361.600 | 26.933.600 | 92.424.800 | 157.916.000 |
| PV of net cash flow | -76.240.000 | 11.824.286 | 28.408.163 | 38.646.251 | 41.620.842 | 37.161.466 |

## Results

> Provisional: the cost side uses W5 v2; the benefit side is still v1 (see the note at the top). NPV, IRR, payback, PI, BCR and break-even will change when the benefits are recalculated.

| Metric | Pessimistic: Must only | Base: Must + Should | Optimistic: full scope | How it is calculated |
|---|---|---|---|---|
| Year-0 investment (VND) | 45.160.000 | 60.080.000 | 76.240.000 | Build cost from W5 + contingency + other one-time cost |
| Steady-state benefit per year (VND) | 45.083.229 | 58.618.672 | 74.640.000 | Section 1.6 (to write): sum over features of full-scope benefit x coverage, x realisation factor. v1 figure, not yet recalculated |
| Total benefits over horizon (VND) | 169.062.108 | 219.820.021 | 279.900.000 | Sum of the Benefits row (after ramp-up) |
| Total running cost over horizon (VND) | 27.096.000 | 36.048.000 | 45.744.000 | Sum of the Running cost row |
| Net cash flow over horizon, undiscounted (VND) | 96.806.108 | 123.692.021 | 157.916.000 | Sum of Net cash flow, Year 0 to Year 5 |
| PV of benefits (VND) | 115.148.500 | 149.719.804 | 190.640.384 | Benefits x discount factor |
| PV of all costs: investment + running (VND) | 64.695.003 | 86.068.995 | 109.219.377 | Costs x discount factor |
| NPV at 12% (VND) | 50.453.497 | 63.650.810 | 81.421.007 | Year-0 flow + NPV() of Year 1-5 flows at the discount rate in B10 |
| IRR | 41.4% | 40.1% | 40.2% | IRR() over Year 0-5 net cash flow; "n/a" if no sign change |
| Simple payback (years) | 2.47 | 2.51 | 2.50 | Last year with negative cumulative cash flow + unrecovered amount / next year's cash flow |
| Simple payback (months) | 29.6 | 30.1 | 30.0 | Years x 12 |
| Discounted payback (years) | 2.88 | 2.94 | 2.93 | Same method on cumulative PV |
| Profitability index | 2.12 | 2.06 | 2.07 | 1 + NPV / Year-0 investment; above 1.00 creates value (PV of net inflows / Year-0 investment) |
| Benefit-cost ratio (PV benefits / PV all costs) | 1.78 | 1.74 | 1.75 | Above 1.00 means PV of benefits exceeds PV of all costs |
| Break-even benefit level (% of modelled benefits for NPV = 0) | 56.2% | 57.5% | 57.3% | 1 - NPV / PV of benefits. Below 100% = headroom; above 100% = NPV already negative |
| Decision signal | ACCEPT (provisional) | ACCEPT (provisional) | ACCEPT (provisional) | ACCEPT when NPV > 0 and IRR > discount rate |
