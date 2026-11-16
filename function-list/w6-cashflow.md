# W6 Financial Appraisal: Cash Flow, NPV, IRR and Payback

Inputs: scenario costs from W5_Handover (Must / Must+Should / full scope), per-feature effort from W2_Rollup.

## 1. Assumptions

| Assumption | Value | Unit | Source / note |
|---|---|---|---|
| Discount rate (hurdle rate) | 12.0% | % per year | Retained from the previous W6 version. Replace with the course / NBC hurdle rate if one is specified. |
| Evaluation horizon | 5 | years (1 to 5) | Years beyond the horizon are switched off in section 2 (benefits and running cost become 0). |
| Annual running cost as % of build cost | 12.0% | % of build / year | Retained from the previous W6. Hosting, support, maintenance. Starts in Year 1. Not a supplier quote. |
| Contingency rate on the Must set | 0.0% | % of Must cost | W5 section 2: 0%, because the Could pool is already the scope contingency (Parameters §8). No double count. |
| Must-set cost (contingency base) | 43.740.000 | VND | Committed scenario cost from W2_Rollup. |
| Other one-time cost (rollout, migration, training) | - | VND | Not in W2/W5. Enter hospital onboarding, data migration, training, security audit if quoted. Added to Year 0. |
| Benefit realisation factor (haircut on modelled benefits) | 100.0% | % of modelled | 100% = benefits as modelled below. Lower it for a conservative view (see also section 4). |

## Pessimistic: Must only

| Line item | Year 0 | Year 1 | Year 2 | Year 3 | Year 4 | Year 5 |
|---|---|---|---|---|---|---|
| Benefits | 0 | 13.524.969 | 27.049.937 | 38.320.744 | 45.083.229 | 45.083.229 |
| Running cost | 0 | -5.248.800 | -5.248.800 | -5.248.800 | -5.248.800 | -5.248.800 |
| Investment (build + contingency + other) | -43.740.000 | 0 | 0 | 0 | 0 | 0 |
| Net cash flow | -43.740.000 | 8.276.169 | 21.801.137 | 33.071.944 | 39.834.429 | 39.834.429 |
| Cumulative net cash flow | -43.740.000 | -35.463.831 | -13.662.694 | 19.409.250 | 59.243.679 | 99.078.108 |
| PV of net cash flow | -43.740.000 | 7.389.436 | 17.379.733 | 23.539.957 | 25.315.500 | 22.603.125 |

## Base: Must + Should

| Line item | Year 0 | Year 1 | Year 2 | Year 3 | Year 4 | Year 5 |
|---|---|---|---|---|---|---|
| Benefits | 0 | 17.585.602 | 35.171.203 | 49.825.872 | 58.618.672 | 58.618.672 |
| Running cost | 0 | -6.979.200 | -6.979.200 | -6.979.200 | -6.979.200 | -6.979.200 |
| Investment (build + contingency + other) | -58.160.000 | 0 | 0 | 0 | 0 | 0 |
| Net cash flow | -58.160.000 | 10.606.402 | 28.192.003 | 42.846.672 | 51.639.472 | 51.639.472 |
| Cumulative net cash flow | -58.160.000 | -47.553.598 | -19.361.595 | 23.485.077 | 75.124.549 | 126.764.021 |
| PV of net cash flow | -58.160.000 | 9.470.002 | 22.474.493 | 30.497.414 | 32.817.818 | 29.301.623 |

## Optimistic: full scope

| Line item | Year 0 | Year 1 | Year 2 | Year 3 | Year 4 | Year 5 |
|---|---|---|---|---|---|---|
| Benefits | 0 | 22.392.000 | 44.784.000 | 63.444.000 | 74.640.000 | 74.640.000 |
| Running cost | 0 | -8.978.400 | -8.978.400 | -8.978.400 | -8.978.400 | -8.978.400 |
| Investment (build + contingency + other) | -74.820.000 | 0 | 0 | 0 | 0 | 0 |
| Net cash flow | -74.820.000 | 13.413.600 | 35.805.600 | 54.465.600 | 65.661.600 | 65.661.600 |
| Cumulative net cash flow | -74.820.000 | -61.406.400 | -25.600.800 | 28.864.800 | 94.526.400 | 160.188.000 |
| PV of net cash flow | -74.820.000 | 11.976.429 | 28.544.005 | 38.767.538 | 41.729.134 | 37.258.155 |

## Results

| Metric | Pessimistic: Must only | Base: Must + Should | Optimistic: full scope | How it is calculated |
|---|---|---|---|---|
| Year-0 investment (VND) | 43.740.000 | 58.160.000 | 74.820.000 | Build cost from W5 + contingency + other one-time cost |
| Steady-state benefit per year (VND) | 45.083.229 | 58.618.672 | 74.640.000 | Section 1.6: sum over features of full-scope benefit x coverage, x realisation factor |
| Total benefits over horizon (VND) | 169.062.108 | 219.820.021 | 279.900.000 | Sum of the Benefits row (after ramp-up) |
| Total running cost over horizon (VND) | 26.244.000 | 34.896.000 | 44.892.000 | Sum of the Running cost row |
| Net cash flow over horizon, undiscounted (VND) | 99.078.108 | 126.764.021 | 160.188.000 | Sum of Net cash flow, Year 0 to Year 5 |
| PV of benefits (VND) | 115.148.500 | 149.719.804 | 190.640.384 | Benefits x discount factor |
| PV of all costs: investment + running (VND) | 62.660.749 | 83.318.454 | 107.185.123 | Costs x discount factor |
| NPV at 12% (VND) | 52.487.750 | 66.401.350 | 83.455.261 | Year-0 flow + NPV() of Year 1-5 flows at the discount rate in B10 |
| IRR | 43.2% | 41.9% | 41.3% | IRR() over Year 0-5 net cash flow; "n/a" if no sign change |
| Simple payback (years) | 2.41 | 2.45 | 2.47 | Last year with negative cumulative cash flow + unrecovered amount / next year's cash flow |
| Simple payback (months) | 29.0 | 29.4 | 29.6 | Years x 12 |
| Discounted payback (years) | 2.81 | 2.86 | 2.88 | Same method on cumulative PV |
| Profitability index | 2.20 | 2.14 | 2.12 | 1 + NPV / Year-0 investment; above 1.00 creates value (PV of net inflows / Year-0 investment) |
| Benefit-cost ratio (PV benefits / PV all costs) | 1.84 | 1.80 | 1.78 | Above 1.00 means PV of benefits exceeds PV of all costs |
| Break-even benefit level (% of modelled benefits for NPV = 0) | 54.4% | 55.6% | 56.2% | 1 - NPV / PV of benefits. Below 100% = headroom; above 100% = NPV already negative |
| Decision signal | ACCEPT | ACCEPT | ACCEPT | ACCEPT when NPV > 0 and IRR > discount rate |
