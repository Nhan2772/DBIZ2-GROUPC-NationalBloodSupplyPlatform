# W4 Priorities: NBSP

Lesson 5 (block W4). Cut by lowering priority, never by lowering estimates. Every change names its rule.

| Rule | MoSCoW meaning | Test question |
|---|---|---|
| R1 Must | The release has no viable use without it | If it is absent on delivery day, is the release useless? |
| R2 Should | Important, but a workaround exists | Painful to leave out, yet the solution still works? |
| R3 Could | Desirable, low impact if dropped | Would anyone notice within the first month? |
| R4 Won't | Agreed out of scope for this release | Did the client accept deferring it? |

| Feature | L2 ID | L2 function name | Initial priority | Effective priority | Justification (required if changed) | Person-hours | Cost (VND) |
|---|---|---|---|---|---|---|---|
| F-01 | FR-01-01 | Request blood provision | Must | Must |  | 10.8 | 2.840.000 |
| F-01 | FR-01-02 | View request queue | Must | Must |  | 8.8 | 2.440.000 |
| F-01 | FR-01-03 | Approve and dispatch blood order | Must | Must |  | 26.8 | 7.640.000 |
| F-01 | FR-01-04 | View shipment details | Must | Must | v2 (D-11): no message is sent on dispatch, so this screen is the only place the hospital sees ETA and expiry and the list it confirms receipt from (FR-01-05). | 5.4 | 1.420.000 |
| F-01 | FR-01-05 | Confirm blood received | Must | Must |  | 8.8 | 2.440.000 |
| F-01 | FR-01-06 | Reject invalid blood request with reasons | Must | Must |  | 5.4 | 1.420.000 |
| F-01 | FR-01-07 | Reject confirmation of a shipment that is not Dispatched | Must | Must |  | 3.4 | 1.020.000 |
| F-01 | FR-01-08 | Block dispatch when eligible stock is insufficient | Must | Must |  | 7.4 | 1.920.000 |
| F-01 | FR-01-09 | Record blood order non-approval with reason | Must | Should | R2 (v2, D-04): a phone call is the workaround, but requests left Pending forever block the queue and distort demand data. | 7.4 | 1.920.000 |
| F-02 | FR-02-01 | Record blood usage | Must | Must |  | 10.8 | 2.840.000 |
| F-02 | FR-02-02 | View role-scoped inventory dashboard | Must | Should | R2: stock figures can still be asked by phone as today; painful, not fatal. | 10.8 | 2.840.000 |
| F-02 | FR-02-03 | View near-expiry blood units | Should | Could | R3: the Dept already sees expiry dates in the Blood Management Software, so nobody would notice its absence in the first month. | 11.8 | 3.240.000 |
| F-02 | FR-02-04 | Receive below-threshold alert | Must | Could | R3: the Dept already sees NBC stock in the Blood Management Software; the alert only saves checking time. | 8.8 | 2.440.000 |
| F-02 | FR-02-05 | Synchronise NBC inventory from Blood Management Software | Must | Must |  | 12.6 | 3.780.000 |
| F-02 | FR-02-07 | Record opening balance and stock adjustment | Should | Should | R2 (new in v2, D-14): workaround is an onboarding import script run by the project team. | 5.4 | 1.420.000 |
| F-02 | FR-02-06 | Reject invalid or excessive blood usage record with reasons | Must | Should | R2: hospital staff check quantities before saving; the database also refuses a negative lot quantity, so inventory cannot go below zero. | 5.4 | 1.420.000 |
| F-03 | FR-03-01 | Log scheduled or periodic blood demand | Must | Could | R3 (v2, D-25): the first forecast runs on usage records and dispatch history; scheduled demand improves it later. | 10.8 | 2.840.000 |
| F-03 | FR-03-02 | Review clinical demand data and anomaly flags | Must | Should | R2: the analyst can review demand data in a spreadsheet outside the system in the first release. | 17.6 | 4.880.000 |
| F-03 | FR-03-03 | Record demand-data validation decision | Must | Should | R2: the analyst keeps the validation decisions in their own notes meanwhile. | 8.8 | 2.440.000 |
| F-03 | FR-03-04 | Respond to data verification request | Must | Could | R3: the analyst can phone the hospital and correct the record directly. | 10.8 | 2.840.000 |
| F-03 | FR-03-05 | Generate draft blood-demand forecast | Must | Must |  | 16.0 | 4.800.000 |
| F-03 | FR-03-06 | Review and approve blood forecast | Must | Must |  | 10.8 | 2.840.000 |
| F-03 | FR-03-07 | View validated blood forecast | Must | Must |  | 10.8 | 2.840.000 |
| F-03 | FR-03-08 | Close validation period at forecast cut-off | Must | Must |  | 3.4 | 1.020.000 |
| F-04 | FR-04-01 | Sign in with hospital account | Must | Must |  | 8.8 | 2.440.000 |
| F-04 | FR-04-02 | Sign in with NBC account | Must | Must |  | 5.4 | 1.420.000 |
| F-04 | FR-04-03 | Enforce role-based authorisation | Must | Must |  | 6.8 | 2.040.000 |
| F-04 | FR-04-04 | Maintain audit trail for validation and forecast decisions | Must | Could | R3: history stays in the database and application logs; no lookup screen is needed in the first month. | 6.8 | 2.040.000 |
| F-04 | FR-04-05 | Operate deployment and regression-test gate | Should | Could | R3: release tests can be run by hand before each release. | 9.2 | 2.760.000 |

v2 totals: Must 162.2 h (58.8%) · Should 55.4 h (20.1%) · Could 58.2 h (21.1%) · total 275.8 h, 76.240.000 VND.
