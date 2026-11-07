# Screen List: NBSP (National Blood Supply Chain Platform)

> DBIZ2 Group C. DRAFT proposal: screens are inferred from the 17 use cases in W0 Table 1 and the
> functions in W1. Priorities are the "Effective priority" in W4 at the time of writing. Confirm
> every row against the Use Case Diagram, the sequence diagrams and the team's usage flows.

Screen ID pattern: `S-<feature>-<nn>`. The ID takes the feature that owns the use case, even when
two actors share the screen. `S-F00-<nn>` is kept for a screen that belongs to no feature (none yet).

| Screen ID | Screen name | Actor | Feature | Shown when (purpose) | Entry points | Use cases / FRs served | Priority | Screen spec |
|---|---|---|---|---|---|---|---|---|
| S-F04-01 | Sign in | Hospital Staff, NBC Blood storage & Distribution dept, NBC Forecasting Analyst, NBC Operations | F-04 | First visit, or session expired; hospital and NBC accounts sign in here | Any link while signed out | UC-06, UC-12 · FR-04-01, FR-04-02 | Must | `screens/screen-spec-S-F04-01.md` |
| S-F01-01 | Blood request form | Hospital Staff | F-01 | Hospital Staff needs blood: enters blood type, quantity and urgency and submits | Home after sign in | UC-01 · FR-01-01, FR-01-06 | Must | `screens/screen-spec-S-F01-01.md` |
| S-F01-02 | Shipment details | Hospital Staff | F-01 | Hospital Staff follows a request, sees the shipment status and confirms that the blood arrived | Home after sign in; success state of S-F01-01 | UC-02, UC-03 · FR-01-04, FR-01-05, FR-01-07 | Must | `screens/screen-spec-S-F01-02.md` |
| S-F01-03 | Request queue | NBC Blood storage & Distribution dept | F-01 | NBC staff looks at pending blood requests within their scope, by urgency and status | Home after sign in (NBC) | UC-07 · FR-01-02 | Must | `screens/screen-spec-S-F01-03.md` |
| S-F01-04 | Order review and dispatch | NBC Blood storage & Distribution dept | F-01 | NBC staff opens one request, checks eligible stock, then approves and dispatches or does not approve | S-F01-03 | UC-08 · FR-01-03, FR-01-08, FR-01-09 | Must | `screens/screen-spec-S-F01-04.md` |
| S-F02-01 | Blood usage record | Hospital Staff | F-02 | Hospital Staff records the blood units used | Menu (Hospital Staff) | UC-04 · FR-02-01, FR-02-06 | Must | `screens/screen-spec-S-F02-01.md` |
| S-F02-02 | Inventory dashboard | Hospital Staff, NBC Blood storage & Distribution dept | F-02 | A user checks blood stock; the content depends on the role | Home after sign in; menu | UC-09 · FR-02-02 | Should | `screens/screen-spec-S-F02-02.md` |
| S-F02-03 | Near-expiry units | NBC Blood storage & Distribution dept | F-02 | NBC staff reviews units close to their expiry date | S-F02-02; menu (NBC) | UC-10 · FR-02-03 | Could | `screens/screen-spec-S-F02-03.md` |
| S-F03-01 | Blood demand log | Hospital Staff | F-03 | Hospital Staff logs scheduled or periodic blood demand (surgical schedules) | Menu (Hospital Staff) | UC-05 · FR-03-01 | Should | `screens/screen-spec-S-F03-01.md` |
| S-F03-02 | Demand data review | NBC Forecasting Analyst | F-03 | The analyst reviews submitted demand data and anomaly flags and records a validation decision | Home after sign in (analyst); menu | UC-13 · FR-03-02, FR-03-03 | Should | `screens/screen-spec-S-F03-02.md` |
| S-F03-03 | Verification response | Hospital Staff | F-03 | Hospital Staff answers a data-verification request raised by the analyst | Notification or link about the request; menu | UC-14 · FR-03-04 | Could | `screens/screen-spec-S-F03-03.md` |
| S-F03-04 | Forecast review and approval | NBC Forecasting Analyst | F-03 | The analyst checks the draft blood-demand forecast and approves it | Menu (analyst); notification that a draft is ready | UC-15 · FR-03-06 | Must | `screens/screen-spec-S-F03-04.md` |
| S-F03-05 | Validated forecast | NBC Operations | F-03 | NBC Operations reads the approved forecast | Home after sign in (NBC Operations); menu | UC-16 · FR-03-07 | Must | `screens/screen-spec-S-F03-05.md` |

## Modals and overlays

| Modal | Opened from screen | Purpose |
|---|---|---|
| Reject order | S-F01-04 | Enter the reason when the order is not approved (FR-01-09) |

## What is deliberately not a screen

| Candidate | Why it is not a screen |
|---|---|
| "Request submitted" | Success state of S-F01-01 |
| "Invalid blood request" | Error state of S-F01-01 (FR-01-06): a reason per invalid field |
| "Shipment not Dispatched" | Error state of S-F01-02 (FR-01-07) |
| "Insufficient stock" | Error state of S-F01-04 (FR-01-08): dispatch is blocked |
| "Usage record rejected" | Error state of S-F02-01 (FR-02-06) |
| Receive below-threshold alert (UC-11) | A notification to NBC staff; shown on S-F02-02 or sent by message. Delivery channel to confirm |
| Synchronise NBC inventory (UC-17) | Periodic job with the Blood Management Software, no user interface |
| Generate draft forecast (FR-03-05) | Triggered by the system; the user only sees the result in S-F03-04 |
| Close validation period (FR-03-08) | Triggered at the forecast cut-off, no user interface |
| Enforce role-based authorisation (FR-04-03) | Cross-cutting rule applied on every screen |
| Audit trail (FR-04-04) | Recorded in the background; becomes a screen only if the team requires a viewer |
