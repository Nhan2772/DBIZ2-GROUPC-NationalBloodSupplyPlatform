# Screen List: NBSP (National Blood Supply Chain Platform)

 
Screen ID pattern: `S-<feature>-<nn>`. Screen spec path pattern: `screens/s-fxx-nn/screen-spec-s-fxx-nn.md`, images in `screens/s-fxx-nn/img/`; a path in the table whose file does not exist yet is the planned location. The ID takes the feature that owns the use case, even when
two actors share the screen. `S-F00-<nn>` is kept for a screen that belongs to no feature (none yet).
 
| Screen ID | Screen name | Actor | Feature | Shown when (purpose) | Entry points | Use cases / FRs served | Priority | Screen spec |
|---|---|---|---|---|---|---|---|---|
| S-F04-01 | Sign in | Hospital Staff, NBC Blood Storage and Distribution Department, NBC Forecasting Analyst, NBC Operations | F-04 | First visit, or session expired; hospital and NBC accounts sign in here | Any link while signed out | UC-06, UC-12 · FR-04-01, FR-04-02 | Must | `screens/s-f04-01/screen-spec-s-f04-01.md` |
| S-F01-01 | Blood request form | Hospital Staff | F-01 | Hospital Staff needs blood: enters blood type, quantity and urgency and submits | Home after sign in | UC-01 · FR-01-01, FR-01-06 | Must | `screens/s-f01-01/screen-spec-s-f01-01.md` |
| S-F01-02 | Shipment details | Hospital Staff | F-01 | Hospital Staff sees the list of the hospital's requests with their status (Pending, Dispatched, Received, Not approved with its reason), opens one, sees the shipment details once Dispatched and confirms that the blood arrived. The list is part of FR-01-04 (estimate unchanged, see note below) | Home after sign in; success state of S-F01-01 | UC-02, UC-03 · FR-01-04, FR-01-05, FR-01-07 | Must | `screens/s-f01-02/screen-spec-s-f01-02.md` |
| S-F01-03 | Request queue | NBC Blood Storage and Distribution Department | F-01 | NBC staff looks at pending blood requests within their scope, by urgency and status | Home after sign in (NBC) | UC-07 · FR-01-02 | Must | `screens/s-f01-03/screen-spec-s-f01-03.md` |
| S-F01-04 | Order review and dispatch | NBC Blood Storage and Distribution Department | F-01 | NBC staff opens one request, checks eligible stock, then approves and dispatches or does not approve | S-F01-03 | UC-08 · FR-01-03, FR-01-08, FR-01-09 | Must | `screens/s-f01-04/screen-spec-s-f01-04.md` |
| S-F02-01 | Blood usage record | Hospital Staff | F-02 | Hospital Staff records the blood units used (Hospital Staff Flow 2) | Menu (Hospital Staff); success state of S-F03-01 ("Record usage") | UC-04 · FR-02-01, FR-02-06, FR-02-07 (opening balance tab: entered once, per blood type quantity and earliest expiry date, stored as one lot; later changes by the NBSP administrator) | Must | `screens/s-f02-01/screen-spec-s-f02-01.md` |
| S-F02-02 | Inventory dashboard | Hospital Staff, NBC Blood Storage and Distribution Department, NBC Operations | F-02 | A user checks blood stock; the content depends on the role | Home after sign in; menu; success state of S-F02-01 ("View inventory"); below-threshold alert | UC-09 · FR-02-02 | Should | `screens/s-f02-02/screen-spec-s-f02-02.md` |
| S-F02-03 | Near-expiry units | NBC Blood Storage and Distribution Department | F-02 | NBC staff reviews units close to their expiry date | S-F02-02; menu (NBC) | UC-10 · FR-02-03 | Could | `screens/s-f02-03/screen-spec-s-f02-03.md` |
| S-F03-01 | Blood demand log | Hospital Staff | F-03 | Hospital Staff logs scheduled or periodic blood demand (surgical schedules) (Hospital Staff Flow 2) | Menu (Hospital Staff); success state of S-F02-01 ("Log blood demand") | UC-05 · FR-03-01 | Could | `screens/s-f03-01/screen-spec-s-f03-01.md` |
| S-F03-02 | Demand data review | NBC Forecasting Analyst | F-03 | The analyst reviews submitted demand data and anomaly flags and records a validation decision | Home after sign in (analyst); menu | UC-13 · FR-03-02, FR-03-03 | Must | `screens/s-f03-02/screen-spec-s-f03-02.md` |
| S-F03-03 | Verification response | Hospital Staff | F-03 | Hospital Staff answers a data-verification request raised by the analyst (Hospital Staff Flow 3) | Link in the verification email; menu | UC-14 · FR-03-04 | Could | `screens/s-f03-03/screen-spec-s-f03-03.md` |
| S-F03-04 | Forecast review and approval | NBC Forecasting Analyst | F-03 | The analyst checks the draft blood-demand forecast and approves it | Menu (analyst); email that a draft is ready | UC-15 · FR-03-06 | Must | `screens/s-f03-04/screen-spec-s-f03-04.md` |
| S-F03-05 | Validated forecast | NBC Operations | F-03 | NBC Operations reads the approved forecast | Home after sign in (NBC Operations); menu | UC-16 · FR-03-07 | Must | `screens/s-f03-05/screen-spec-s-f03-05.md` |
 
Note on S-F01-02: the request list is not a separate FR. It is counted inside FR-01-04 (W1 FR-01-04-FE, 4 h unchanged; SD-UC-02 "open My Requests and select"). Risk: the 4 h may be short; re-estimate only if the screen spec shows more than a simple list with a status filter.
 
## Modals and overlays
 
| Modal | Opened from screen | Purpose |
|---|---|---|
| Confirm approve & dispatch | S-F01-04 | Shows the request ID, the number and blood type of the selected units and the ETA, and asks the Dept to confirm; on confirm NBSP reserves the units in the Blood Management Software and sets the request to Dispatched (FR-01-03, AD-UC-08 section 5) |
| Not approve request | S-F01-04 | Choose the reason (No eligible stock, Request data incorrect, Cancelled by hospital, Other + note) and confirm Not approved (FR-01-09, D-04) |
 
## What is deliberately not a screen
 
| Candidate | Why it is not a screen |
|---|---|
| "Request submitted" | Success state of S-F01-01 |
| "Invalid blood request" | Error state of S-F01-01 (FR-01-06): a reason per invalid field |
| "Shipment not Dispatched" | Error state of S-F01-02 (FR-01-07) |
| "Insufficient stock" | Error state of S-F01-04 (FR-01-08): dispatch is blocked |
| "Usage record rejected" | Error state of S-F02-01 (FR-02-06) |
| "Invalid demand plan" | Error state of S-F03-01: nothing is saved and the reason is shown (FR-03-01, IF clause) |
| Receive below-threshold alert (UC-11) | An email to the Dept and NBC Operations (the channel that reaches them). Inside NBSP only S-F02-02 shows it: a banner above the data and a "Dưới ngưỡng" badge on each affected blood-type tile; no badge on the menu and no alert centre (D-16, D-20; W1 FR-02-04-FE) |
| Synchronise NBC inventory (UC-17) | Periodic job with the Blood Management Software, no user interface |
| Generate draft forecast (FR-03-05) | Triggered by the system; the user only sees the result in S-F03-04 |
| Close validation period (FR-03-08) | Triggered at the forecast cut-off, no user interface |
| Enforce role-based authorisation (FR-04-03) | Cross-cutting rule applied on every screen |
| Audit trail (FR-04-04) | Recorded in the background; becomes a screen only if the team requires a viewer |
 
