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
| F-01 | FR-01-04 | View shipment details | Must | Could | R3: dispatch already sends the shipment information to the hospital (UC-08), and receipt can be confirmed without this screen. | 5.4 | 1.420.000 |
| F-01 | FR-01-05 | Confirm blood received | Must | Must |  | 8.8 | 2.440.000 |
| F-01 | FR-01-06 | Reject invalid blood request with reasons | Must | Must |  | 5.4 | 1.420.000 |
| F-01 | FR-01-07 | Reject confirmation of a shipment that is not Dispatched | Must | Must |  | 3.4 | 1.020.000 |
| F-01 | FR-01-08 | Block dispatch when eligible stock is insufficient | Must | Must |  | 7.4 | 1.920.000 |
| F-01 | FR-01-09 | Record blood order non-approval with reason | Must | Could | R3: Dept has no non-approval path yet; a request NBC does not approve stays Pending or is handled by phone. | 7.4 | 1.920.000 |
| F-02 | FR-02-01 | Record blood usage | Must | Must |  | 10.8 | 2.840.000 |
| F-02 | FR-02-02 | View role-scoped inventory dashboard | Must | Should | R2: stock figures can still be asked by phone as today; painful, not fatal. | 10.8 | 2.840.000 |
| F-02 | FR-02-03 | View near-expiry blood units | Should | Could | R3: expiry dates can be read from the inventory dashboard, so nobody would notice its absence in the first month. | 11.8 | 3.240.000 |
| F-02 | FR-02-04 | Receive below-threshold alert | Must | Could | R3: NBC Staff can see low stock on the dashboard; the alert only saves checking time. | 8.8 | 2.440.000 |
| F-02 | FR-02-05 | Synchronise NBC inventory from Blood Management Software | Must | Must |  | 12.6 | 3.780.000 |
| F-02 | FR-02-06 | Reject invalid or excessive blood usage record with reasons | Must | Should | R2: hospital staff can check the numbers by hand before saving; the dashboard shows any mismatch later. | 5.4 | 1.420.000 |
| F-03 | FR-03-01 | Log scheduled or periodic blood demand | Must | Should | R2: the first forecast can run on usage records and dispatch history; scheduled demand improves it later. | 10.8 | 2.840.000 |
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
