# Screen List: NBSP — NBC Blood Storage and Distribution Department

Screen ID pattern: `S-<feature>-<nn>`, for example `S-F01-02` is the second screen of feature F-01.
Screens shared by every feature (sign in, settings) use `S-F00-<nn>`.

> **Scope:** screens used by the NBC Blood Storage and Distribution Department (UC-07 to UC-12). The Operations (Forecast) side (NBC Forecasting Analyst, NBC Operations) has its own list and refers to the shared screens S-F00-01 and S-F02-02. Thresholds are loaded at deployment and never change: there is no threshold screen.
> Screen numbers are provisional: S-F01-01 and S-F01-02 are reserved for the hospital screens, S-F02-01 for "Record blood usage". Merge with the other owners' lists before final.
> UC-17 (Synchronise NBC Inventory) has no screen: it is started by time (see `AD-UC-17.md`).

| Screen ID | Screen name | Actor | Feature | Shown when (purpose) | Entry points | Use cases / FRs served | Priority | Screen spec |
|---|---|---|---|---|---|---|---|---|
| S-F00-01 | Sign in | All users (shared: Hospital Staff, the Dept, NBC Forecasting Analyst, NBC Operations) | F-00 | The user is not signed in, signed out, or the session expired. Enter username and password; role and department decide the start screen. | NBSP URL; sign out; session expiry | UC-12 / FR-04-02, FR-04-03 (UC-06 / FR-04-01 for hospitals) | Must | `screens/screen-spec-S-F00-01.md` |
| S-F01-03 | Request Queue | NBC Blood Storage and Distribution Department | F-01 | After sign-in, and whenever the staff wants the pending hospital requests, sorted by urgency. | S-F00-01 (start screen of the Dept); main navigation; S-F01-04; S-F02-04 | UC-07 / FR-01-02 | Must | `screens/screen-spec-S-F01-03.md` |
| S-F01-04 | Review & Dispatch Request | NBC Blood Storage and Distribution Department | F-01 | The staff opens one pending request: sees the request and the available units, selects units, enters the ETA and approves and dispatches in one action. | S-F01-03 | UC-08 / FR-01-03 | Must | `screens/screen-spec-S-F01-04.md` |
| S-F02-02 | Inventory Dashboard | NBC Blood Storage and Distribution Department, NBC Operations (shared: Hospital Staff sees own scope) | F-02 | The user wants stock by blood type within the role's scope (NBC and each hospital for the Dept and NBC Operations), with the last synchronisation timestamp. | Main navigation; S-F02-04 | UC-09 / FR-02-02, FR-02-05 (timestamp) | Should | `screens/screen-spec-S-F02-02.md` |
| S-F02-03 | Near-Expiry Units | NBC Blood Storage and Distribution Department | F-02 | The staff wants units approaching expiry with remaining days. | Main navigation | UC-10 / FR-02-03 | Could | `screens/screen-spec-S-F02-03.md` |
| S-F02-04 | Alerts | NBC Blood Storage and Distribution Department | F-02 | An alert was created after a synchronisation (a blood type is below its fixed threshold), or the staff opens the list of alerts. Informational only. | In-system notification (bell); main navigation | UC-11 / FR-02-04 | Could | `screens/screen-spec-S-F02-04.md` |

Priority is the effective priority from W4.

## Modals and overlays

| Modal | Opened from screen | Purpose |
|---|---|---|
| Confirm approve & dispatch | S-F01-04 | Screen safeguard before the single "approve and dispatch" action reserves the units in the Blood Management Software (not a step of AD-UC-08) |

Not modals and not screens: alert detail (a panel in S-F02-04), the inventory filter (part of S-F02-02), the notification bell (in the header of every screen).

## States to describe in the Screen Specs (from the ADs)

| Screen | State | Source |
|---|---|---|
| S-F01-04 | Stock shortage: available units are fewer than requested, dispatch blocked, request stays Pending | AD-UC-08 v0.4, s2 |
| S-F01-04 | Request no longer Pending or a selected unit no longer available: submission rejected, reselect | AD-UC-08 v0.4, s4 |
| S-F01-04 | Stock service (BMS) unavailable: approval blocked, request stays Pending | AD-UC-08 v0.4, s6; W1 FR-01-03 |
| S-F01-04 | Success: request Dispatched, confirmation shown | AD-UC-08 v0.4, s7 |
| S-F02-02 | Data is not real time: always show the synchronisation timestamp; the previous snapshot is kept when a sync fails | AD-UC-17, s4 and Q4 |
| S-F02-04 | Several alerts for the same blood type may pile up (one per sync) | AD-UC-11, Q1 |

## Screens touched (to paste into section 5 of the AD files)

Already filled in AD-UC-08 v0.4 and AD-UC-11 v0.9. AD-UC-17: none (started by time).

## Checks before you save

- [x] Every Screen ID is unique.
- [x] No row is a state (error, empty, loading, success) or a confirmation modal.
- [x] Every Must use case has at least one screen (UC-07, UC-08, UC-12). UC-17 is a time-triggered activity with no screen.
- [ ] Every actor action in the activity diagrams maps to a screen. Done for AD-UC-08, AD-UC-11 and AD-UC-17; AD-UC-12 is not needed (shared sign-in); the ADs of UC-07, 09, 10 do not exist yet.
- [x] Every screen serves at least one FR.
