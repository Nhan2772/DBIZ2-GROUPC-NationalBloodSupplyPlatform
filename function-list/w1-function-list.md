# W1 Function List: NBSP

## L2 functions

| Feature | L2 ID | L2 function name | Actor | EARS acceptance criterion | Source trace | Initial priority | FP type |
|---|---|---|---|---|---|---|---|
| F-01 | FR-01-01 | Request blood provision | Hospital Staff | WHEN Hospital Staff submits a blood product, blood type, quantity, urgency, needed-by date and time, and a contact person, NBSP SHALL validate the input, create a Pending blood request and place it in the NBC request queue. | UC:UC-01; SD:UC-01 | Must | EI |
| F-01 | FR-01-02 | View request queue | NBC Blood Storage and Distribution Department | WHEN an authorised NBC Blood Storage and Distribution Department user opens the request queue, NBSP SHALL display pending requests within their scope ordered by urgency and then submission time. | UC:UC-07; SD:UC-07 | Must | EQ |
| F-01 | FR-01-03 | Approve and dispatch blood order | NBC Blood Storage and Distribution Department | WHEN an authorised NBC Blood Storage and Distribution Department user approves and dispatches a pending request, selects available blood units and enters an ETA, NBSP SHALL reserve the units, create shipment details and set the request to Dispatched; NBSP SHALL propose eligible units in FEFO order (earliest expiry first); IF the request is no longer Pending or a selected unit is no longer available, NBSP SHALL reject the action and show the reason; IF the Blood Management Software is unavailable or returns an error, NBSP SHALL block dispatch and keep the request Pending, and IF NBSP fails to save the shipment after the units were reserved, NBSP SHALL cancel the reservation. | UC:UC-08; SD:UC-08; ASM:BMS API | Must | EI |
| F-01 | FR-01-04 | View shipment details | Hospital Staff | WHEN Hospital Staff opens a dispatched request, NBSP SHALL display the assigned units, their expiry dates, shipment status and ETA without changing stored data. | UC:UC-02; SD:UC-02 | Must | EQ |
| F-01 | FR-01-05 | Confirm blood received | Hospital Staff | WHEN Hospital Staff confirms arrival of a dispatched shipment, NBSP SHALL mark the request Received and add the received units to that hospital inventory. | UC:UC-03; SD:UC-03 | Must | EI |
| F-01 | FR-01-06 | Reject invalid blood request with reasons | NBSP | IF the submitted blood product, blood type, quantity, urgency, needed-by date and time or contact person fails validation, NBSP SHALL create no blood request, SHALL show the reason for each invalid field and SHALL let Hospital Staff correct and resubmit. | AD:AD-UC-01; UC:UC-01; SD:UC-01 | Must | NC |
| F-01 | FR-01-07 | Reject confirmation of a shipment that is not Dispatched | NBSP | IF the shipment is not in Dispatched status when Hospital Staff confirms arrival, NBSP SHALL reject the confirmation, SHALL change neither the request nor the hospital inventory and SHALL state the reason. | AD:AD-UC-03; UC:UC-03; SD:UC-03 | Must | NC |
| F-01 | FR-01-08 | Block dispatch when eligible stock is insufficient | NBSP | IF the available eligible blood units are insufficient to fulfill a blood order, NBSP SHALL prevent dispatch, SHALL display the shortage reason to the NBC Blood Storage and Distribution Department user, and SHALL leave the order and inventory unchanged. | AD:AD-UC-08; UC:UC-08 | Must | NC |
| F-01 | FR-01-09 | Record blood order non-approval with reason | NBSP | WHEN an authorised NBC Blood Storage and Distribution Department user declines a pending blood order, NBSP SHALL record the non-approval decision and reason, and SHALL prevent the order from proceeding to allocation and dispatch. | AD:AD-UC-08; UC:UC-08 | Must | NC |
| F-02 | FR-02-01 | Record blood usage | Hospital Staff | WHEN Hospital Staff records blood type, unit quantity, usage date, used/discarded status and reason, NBSP SHALL deduct the units from hospital inventory and append the transaction to usage history. | UC:UC-04; SD:UC-04 | Must | EI |
| F-02 | FR-02-02 | View role-scoped inventory dashboard | NBC Blood Storage and Distribution Department, NBC Operations, Hospital Staff | WHEN an authorised user opens the inventory dashboard, NBSP SHALL identify the user role and display only the inventory scope permitted for that role, including the latest NBC synchronisation timestamp. | UC:UC-09; Context | Must | EQ |
| F-02 | FR-02-03 | View near-expiry blood units | NBC Blood Storage and Distribution Department | WHEN a NBC Blood Storage and Distribution Department user opens the near-expiry list, NBSP SHALL display available units approaching expiry, calculate days remaining and order them by earliest expiry. | UC:UC-10; SD:UC-10 | Should | EO |
| F-02 | FR-02-04 | Receive below-threshold alert | NBC Blood Storage and Distribution Department, NBC Operations | WHEN, after a synchronisation, the stock of a blood type is below its threshold (loaded at deployment), NBSP SHALL open one alert for that blood type if none is Open, and SHALL notify the NBC Blood Storage and Distribution Department and NBC Operations by in-app badge and email; WHEN the stock of that blood type is back at or above the threshold, NBSP SHALL set the alert Resolved. | UC:UC-11; SD:UC-11 | Must | EO |
| F-02 | FR-02-05 | Synchronise NBC inventory from Blood Management Software | NBSP | WHEN the scheduled inventory synchronisation runs, NBSP SHALL read the latest NBC inventory and source timestamp from the external Blood Management Software and update the NBSP inventory snapshot without writing back to the source; IF the source returns an error or invalid data, NBSP SHALL keep the previous snapshot and log the failure. | UC:UC-17; Context:BMS; SD:UC-17; ASM:periodic sync | Must | EI |
| F-02 | FR-02-06 | Reject invalid or excessive blood usage record with reasons | NBSP | IF the usage record is incomplete or the quantity exceeds the hospital inventory, NBSP SHALL change neither the inventory nor the usage history, SHALL show the reason and SHALL let Hospital Staff correct and resubmit. | AD:AD-UC-04; UC:UC-04; SD:UC-04 | Must | NC |
| F-02 | FR-02-07 | Record hospital opening inventory balance | Hospital Staff | WHEN Hospital Staff enters the opening balance of each blood type for the hospital, NBSP SHALL validate the quantities and store them as the starting hospital inventory; IF a quantity is missing or invalid, NBSP SHALL save nothing and SHALL show the reason. | UC:UC-04; screen S-F02-01 (opening balance tab); W2; W4 | Should | EI |
| F-03 | FR-03-01 | Log scheduled or periodic blood demand | Hospital Staff | WHEN Hospital Staff submits a scheduled-surgery or periodic-transfusion plan with the required demand fields, NBSP SHALL validate and store the plan as forecasting input; IF a required demand field is missing or invalid, NBSP SHALL save no plan, SHALL show the reason and SHALL let Hospital Staff correct and resubmit. | UC:UC-05; SD:UC-05 | Must | EI |
| F-03 | FR-03-02 | Review clinical demand data and anomaly flags | Forecasting Analyst | WHEN the Forecasting Analyst selects a hospital, date range and blood group, NBSP SHALL display blood usage, transfusion-patient counts and scheduled demand, and SHALL flag each record that violates a configured validation rule with the triggering reason. | UC:UC-13; SD:UC-13 | Must | EO |
| F-03 | FR-03-03 | Record demand-data validation decision | Forecasting Analyst | WHEN the Forecasting Analyst accepts (optionally with a note), excludes or requests verification of a flagged record, NBSP SHALL store the status Accepted or Excluded, or mark the record Pending Verification and notify the hospital; IF a request is Overdue, NBSP SHALL let the analyst exclude the record or accept it with a note. | UC:UC-13; SD:UC-13 | Must | EI |
| F-03 | FR-03-04 | Respond to data verification request | Hospital Staff | WHEN Hospital Staff submits a confirmation, correction or supplement for a verification request, NBSP SHALL save the response and change history and notify the Forecasting Analyst; IF the configured deadline is missed, NBSP SHALL mark the request Overdue and notify the Forecasting Analyst. | UC:UC-14; SD:UC-14 | Must | EI |
| F-03 | FR-03-05 | Generate draft blood-demand forecast | NBSP | WHEN demand data for a forecasting period is finalised, NBSP SHALL run the active model using accepted demand data and NBC allocation history, create a versioned Draft forecast and notify the Forecasting Analyst. | UC:UC-15 trigger; SD:UC-15; ASM:active model | Must | EO |
| F-03 | FR-03-06 | Review and approve blood forecast | Forecasting Analyst | WHEN the Forecasting Analyst reviews a Draft forecast batch, NBSP SHALL support adjustment, approval or rejection with reasons, bulk approval of Normal items, and SHALL publish only approved items as a new immutable version. | UC:UC-15; SD:UC-15 | Must | EI |
| F-03 | FR-03-07 | View validated blood forecast | NBC Operations | WHEN NBC Operations selects a hospital, blood group and time horizon, NBSP SHALL display approved forecasts with approval timestamp, recent forecast accuracy, current inventory and the last inventory synchronisation timestamp; IF none exists, it SHALL display a no-approved-forecast message. | UC:UC-16; SD:UC-16 | Must | EO |
| F-03 | FR-03-08 | Close validation period at forecast cut-off | NBSP | WHEN no flagged record remains unresolved or the forecast cut-off is reached, NBSP SHALL exclude any record still unresolved from forecasting, flag it, and mark the demand data of the period Finalised. | UC:UC-13; SD:UC-13; ASM:forecast cut-off; split from FR-03-03 | Must | NC |
| F-04 | FR-04-01 | Sign in with hospital account | Hospital Staff | WHEN Hospital Staff submits valid hospital credentials, NBSP SHALL establish an authenticated session scoped to that hospital and expose only hospital-authorised functions. | UC:UC-06; SD:UC-06 | Must | EI |
| F-04 | FR-04-02 | Sign in with NBC account | NBC Blood Storage and Distribution Department, NBC Forecasting Analyst, NBC Operations | WHEN an NBC user submits valid NBC credentials, NBSP SHALL establish an authenticated session with the user role and department permissions. | UC:UC-12; SD:UC-12 | Must | EI |
| F-04 | FR-04-03 | Enforce role-based authorisation | NBSP | WHEN an authenticated user invokes an NBSP function or data query, NBSP SHALL permit only actions and data scopes authorised for that user role and department. | derived: UC-06/09/12/13/15/16 | Must | NC |
| F-04 | FR-04-04 | Maintain audit trail for validation and forecast decisions | NBSP | WHEN a clinical-data or forecast decision is saved, NBSP SHALL append the actor, timestamp, prior value, final value/status and note to an audit record without overwriting earlier history. | UC:UC-13/14/15; ASM:version history | Must | NC |
| F-04 | FR-04-05 | Operate deployment and regression-test gate | NBSP | WHEN a new NBSP release is promoted, the delivery pipeline SHALL run the defined automated unit, integration and smoke tests and SHALL block promotion if a critical test fails. | Method:C4 hidden tech; derived:all UCs | Should | NC |

## L3 work items

| L3 ID | L2 ID | Layer | L3 work item | Size | Base hours | EARS precise? | Hidden work? |
|---|---|---|---|---|---|---|---|
| FR-01-01-FE | FR-01-01 | FE | Portal form for blood product, blood type, quantity, urgency, needed-by, contact and submit action | M | 8 | Yes | No |
| FR-01-01-BE | FR-01-01 | BE | Validate request payload (blood product, blood type, quantity, urgency, needed-by, contact); create Pending request and queue record | M | 8 | Yes | No |
| FR-01-02-FE | FR-01-02 | FE | NBC request-queue screen with urgency/status display | S | 4 | Yes | No |
| FR-01-02-BE | FR-01-02 | BE | Authorised pending-request query and urgency/submission-time ordering | M | 8 | Yes | No |
| FR-01-03-FE | FR-01-03 | FE | Approve-and-dispatch screen for FEFO unit selection, ETA and validation feedback | M | 8 | Yes | No |
| FR-01-03-BE | FR-01-03 | BE | Revalidate request; persist shipment; update request to Dispatched | L | 16 | Yes | No |
| FR-01-03-INT | FR-01-03 | INT | Reserve selected NBC units through Blood Management Software API and handle timeout | M | 8 | Yes | Yes |
| FR-01-04-FE | FR-01-04 | FE | Hospital shipment-detail panel with unit expiry, ETA and status | S | 4 | Yes | No |
| FR-01-04-BE | FR-01-04 | BE | Read authorised shipment details and assigned unit metadata | S | 4 | Yes | No |
| FR-01-05-FE | FR-01-05 | FE | Confirm Received action and result state | S | 4 | Yes | No |
| FR-01-05-BE | FR-01-05 | BE | Atomically mark shipment Received and add units to hospital inventory | M | 8 | Yes | No |
| FR-01-06-FE | FR-01-06 | FE | Show a reason per invalid field and keep entered values so the user can correct and resubmit | S | 4 | Yes | No |
| FR-01-06-BE | FR-01-06 | BE | Enforce allowed values and mandatory fields for blood product, blood type, quantity, urgency, needed-by and contact; return a reason per invalid field | S | 4 | Yes | Yes |
| FR-01-07-BE | FR-01-07 | BE | Check shipment status is Dispatched before marking Received; reject a repeat confirmation so units are never added twice | S | 4 | Yes | Yes |
| FR-01-08-FE | FR-01-08 | FE | Display insufficient-stock reason and prevent dispatch confirmation | S | 4 | No | No |
| FR-01-08-BE | FR-01-08 | BE | Validate eligible blood availability before dispatch; reject insufficient-stock dispatch attempts without changing order or inventory | S | 4 | No | Yes |
| FR-01-09-FE | FR-01-09 | FE | Provide non-approval action and reason entry for the NBC Blood Storage and Distribution Department | S | 4 | No | No |
| FR-01-09-BE | FR-01-09 | BE | Record non-approval decision and reason; prevent further allocation and dispatch | S | 4 | No | Yes |
| FR-02-01-FE | FR-02-01 | FE | Usage/discard entry form with reason and quantity | M | 8 | Yes | No |
| FR-02-01-BE | FR-02-01 | BE | Validate usage; deduct hospital inventory; append usage/discard history | M | 8 | Yes | No |
| FR-02-02-FE | FR-02-02 | FE | Role-scoped inventory dashboard with hospital/blood-type filters | M | 8 | Yes | No |
| FR-02-02-BE | FR-02-02 | BE | Aggregate authorised hospital/NBC inventory and expose latest sync timestamp | M | 8 | Yes | No |
| FR-02-03-FE | FR-02-03 | FE | Near-expiry unit list with days-remaining display | S | 4 | No | No |
| FR-02-03-BE | FR-02-03 | BE | Query eligible units, calculate remaining days and sort by expiry | M | 8 | No | No |
| FR-02-04-FE | FR-02-04 | FE | In-app alert badge and banner on the inventory dashboard, with alert detail | S | 4 | Yes | No |
| FR-02-04-BE | FR-02-04 | BE | Threshold comparison after each sync; one Open alert per blood type, Resolved on recovery; email to recipients | M | 8 | Yes | Yes |
| FR-02-05-BE | FR-02-05 | BE | Map, validate and timestamp synchronised NBC inventory snapshot | S | 4 | Yes | Yes |
| FR-02-05-INT | FR-02-05 | INT | Scheduled connector to read NBC inventory from Blood Management Software | M | 8 | Yes | Yes |
| FR-02-06-FE | FR-02-06 | FE | Show the reason and keep entered values so the user can correct and resubmit the usage record | S | 4 | Yes | No |
| FR-02-06-BE | FR-02-06 | BE | Check mandatory fields, allowed values and available quantity before deducting; return the reason | S | 4 | Yes | No |
| FR-02-07-FE | FR-02-07 | FE | Opening-balance tab on the blood usage screen, with the reason for an invalid quantity | S | 4 | Yes | No |
| FR-02-07-BE | FR-02-07 | BE | Validate and store the opening balance per blood type as the starting hospital inventory | S | 4 | Yes | No |
| FR-03-01-FE | FR-03-01 | FE | Demand-plan form for surgery or periodic transfusion; show the reason and keep entered values when the plan is invalid | M | 8 | Yes | No |
| FR-03-01-BE | FR-03-01 | BE | Validate and store scheduled/periodic demand as forecasting input | M | 8 | Yes | No |
| FR-03-02-FE | FR-03-02 | FE | Analyst demand-data view with filters, flags and reason display | M | 8 | Yes | No |
| FR-03-02-BE | FR-03-02 | BE | Aggregate usage/patient/schedule data and execute configured anomaly rules | L | 16 | Yes | No |
| FR-03-03-FE | FR-03-03 | FE | Analyst actions: Accept, Exclude, Request Verification and note | S | 4 | Yes | No |
| FR-03-03-BE | FR-03-03 | BE | Persist validation status; create verification request; record overdue decision | M | 8 | Yes | No |
| FR-03-04-FE | FR-03-04 | FE | Hospital verification-request view and confirm/correct/supplement form | M | 8 | Yes | No |
| FR-03-04-BE | FR-03-04 | BE | Persist response/change history; update request status; notify analyst | M | 8 | Yes | No |
| FR-03-05-BE | FR-03-05 | BE | Forecast-cycle orchestration: load accepted demand and allocation history; store Draft version | M | 8 | Yes | Yes |
| FR-03-05-INT | FR-03-05 | INT | Invoke the active deployed forecast model and capture model output/version | M | 8 | Yes | Yes |
| FR-03-06-FE | FR-03-06 | FE | Forecast-review workspace for Normal/Needs Review, adjustments and reasons | M | 8 | Yes | No |
| FR-03-06-BE | FR-03-06 | BE | Apply approvals/rejections, immutable publication versioning and review audit | M | 8 | Yes | No |
| FR-03-07-FE | FR-03-07 | FE | Read-only validated-forecast dashboard with filters and inventory context | M | 8 | Yes | No |
| FR-03-07-BE | FR-03-07 | BE | Query approved forecast, accuracy and current inventory; handle no-result state | M | 8 | Yes | No |
| FR-03-08-BE | FR-03-08 | BE | Apply cut-off rule: exclude and flag unresolved records; set the period Finalised | S | 4 | Yes | Yes |
| FR-04-01-FE | FR-04-01 | FE | Hospital sign-in screen and authentication feedback | S | 4 | Yes | No |
| FR-04-01-BE | FR-04-01 | BE | Credential verification and hospital-scoped session creation | M | 8 | Yes | No |
| FR-04-02-FE | FR-04-02 | FE | NBC sign-in screen and authentication feedback | S | 4 | Yes | No |
| FR-04-02-BE | FR-04-02 | BE | Credential verification and role/department session creation | S | 4 | Yes | No |
| FR-04-03-BE | FR-04-03 | BE | Central role, department and data-scope permission checks on protected endpoints | M | 8 | Yes | Yes |
| FR-04-04-BE | FR-04-04 | BE | Append-only audit records for validation decisions, verification, forecast review and publication decisions | M | 8 | Yes | Yes |
| FR-04-05-INT | FR-04-05 | INT | CI/CD packaging, automated regression/integration tests and promotion gate | M | 8 | Yes | Yes |

29 L2 functions, 54 L3 work items, 356 base hours. No row is above the 16-hour ceiling.

NC rows (FR-01-06, FR-01-07, FR-01-08, FR-01-09 ...) score zero function points but carry real work: that gap is why the function-point count is only a cross-check.
