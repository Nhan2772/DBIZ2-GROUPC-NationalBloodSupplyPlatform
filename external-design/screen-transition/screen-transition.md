# Screen Transition Diagram: NBSP (National Blood Supply Chain Platform)

> DBIZ2 Group C. Draft. Screen IDs follow `screen-list.md` (13 screens; Sign in is `S-F04-01`) and the merged `usage-flow.md`.

NBSP has no home screen in the screen list, so each diagram has a "Main menu" node. It stands for the navigation bar that is available on every screen of that actor, and each arrow from it is a menu item. This is an assumption to confirm with the team.

## 1. Hospital Staff

```mermaid
flowchart LR
  entry(["Opens NBSP"])
  notif(["Verification request notification from NBC"])
  S04_01["S-F04-01<br>Sign in"]
  S01_01["S-F01-01<br>Blood request form"]
  S01_02["S-F01-02<br>Shipment details"]
  S02_01["S-F02-01<br>Blood usage record"]
  S02_02["S-F02-02<br>Inventory dashboard"]
  S03_01["S-F03-01<br>Blood demand log"]
  S03_03["S-F03-03<br>Verification response"]
  entry --> S04_01
  S04_01 -->|"signed in, to order blood"| S01_01
  S04_01 -->|"signed in, to record usage"| S02_01
  S04_01 -->|"signed in, to log blood demand"| S03_01
  S01_01 -->|"[request submitted] View shipment"| S01_02
  S01_02 -->|"Request blood"| S01_01
  S02_01 -->|"[usage saved] View inventory"| S02_02
  S02_02 -->|"Menu: Record usage"| S02_01
  S03_01 -->|"Menu: Verification requests"| S03_03
  S03_03 -->|"Menu: Blood demand"| S03_01
  notif -->|"[signed in]"| S03_03
  notif -->|"[session expired]"| S04_01
  S04_01 -->|"[came from a notification] signed in"| S03_03
```

Main task "order blood": S-F01-01 → S-F01-02. One step from the start screen; confirming that the blood arrived is an action on S-F01-02.

## 2. NBC Blood Storage and Distribution Department

```mermaid
flowchart LR
  entry(["Opens NBSP"])
  alert(["Below-threshold alert received"])
  S04_01["S-F04-01<br>Sign in"]
  S01_03["S-F01-03<br>Request queue"]
  S01_04["S-F01-04<br>Order review and dispatch"]
  S02_02["S-F02-02<br>Inventory dashboard"]
  S02_03["S-F02-03<br>Near-expiry units"]
  entry --> S04_01
  S04_01 -->|"signed in, to fulfil requests"| S01_03
  S04_01 -->|"signed in, to check stock"| S02_02
  S01_03 -->|"opens a request"| S01_04
  S01_04 -->|"[dispatched, or left Pending with the reason] Back to queue"| S01_03
  S01_03 -->|"Check stock"| S02_02
  S02_02 -->|"Back to queue"| S01_03
  S02_02 -->|"Near-expiry units"| S02_03
  S02_03 -->|"back"| S02_02
  S02_03 -->|"Menu: Requests"| S01_03
  alert -->|"[signed in]"| S02_02
  alert -->|"[session expired]"| S04_01
  S04_01 -->|"[came from an alert] signed in"| S02_02
```

Main task "dispatch a blood order": S-F01-03 → S-F01-04. Two screens from the start screen.

## 3. NBC Forecasting Analyst

```mermaid
flowchart LR
    ENTRY(["Opens NBSP"])
    NOTICE(["Notification: Forecast draft ready"])

    S_F04_01["S-F04-01<br>Sign in"]
    S_F03_02["S-F03-02<br>Demand Data Review"]
    S_F03_04["S-F03-04<br>Forecast Review and Approval"]

    ENTRY -->|"No active session"| S_F04_01
    ENTRY -->|"Active session: Demand data"| S_F03_02
    ENTRY -->|"Active session: Forecast"| S_F03_04

    S_F04_01 -->|"Signed in: Select Demand data"| S_F03_02
    S_F04_01 -->|"Signed in: Select Forecast"| S_F03_04

    S_F03_02 -->|"Menu: Forecast"| S_F03_04
    S_F03_04 -->|"Menu: Demand data"| S_F03_02

    NOTICE -->|"Open with active session"| S_F03_04
    NOTICE -->|"Sign in required"| S_F04_01

```

On S-F03-02 the analyst records the validation decision or sends a data-verification request. The request leaves NBSP as an email to the hospital and enters the Hospital Staff diagram at S-F03-03 (see section 1); it is not an arrow here.

## 4. NBC Operations

```mermaid
flowchart LR
  entry(["Opens NBSP"])
  alert(["Below-threshold alert received"])
  S04_01["S-F04-01<br>Sign in"]
  S03_05["S-F03-05<br>Validated forecast"]
  S02_02["S-F02-02<br>Inventory dashboard"]
  entry --> S04_01
  S04_01 -->|"signed in, to view the forecast"| S03_05
  S04_01 -->|"signed in, to check stock"| S02_02
  S03_05 -->|"Menu: Inventory"| S02_02
  S02_02 -->|"Menu: Forecast"| S03_05
  alert -->|"[signed in]"| S02_02
  alert -->|"[session expired]"| S04_01
  S04_01 -->|"[came from an alert] signed in"| S02_02
```

NBC Operations shares S-F02-02 with the Dept (decided 2026-11-06); the screen is owned in section 2. The alert (UC-11) is a notification with no screen of its own, so the entry point "Below-threshold alert received" leads to S-F02-02.

## Checks

- Every screen in the Screen List appears: yes (13 of 13).
- Every screen reachable from an entry point: yes (menu items and email or alert links count as entry points).
- No dead ends: yes. Every screen has the main menu; the screens with no other arrow out (S-F01-04 returns to the queue, S-F03-03 and S-F03-05 end their task) are left through the menu.
- Usage-flow paths: every arrow of the flows of the four actors in [usage-flow.md](usage-flow.md) has a path (table below).
- Sign in is shared by all actors; signed-out users who open an email or alert link see S-F04-01 first and then return to the screen they asked for (screen-level rule in S-F04-01).
- Screens shared by several actors (S-F04-01, S-F02-02) appear in each actor's diagram.

## Usage-flow paths

| Actor | Usage-flow arrow | Path on this diagram |
|---|---|---|
| Hospital Staff | UC-06 to UC-01 | S-F04-01 Sign in → S-F01-01 Blood request form ("signed in") |
| Hospital Staff | UC-01 to UC-02 | S-F01-01 → S-F01-02 Shipment details ("[request submitted] View shipment") |
| Hospital Staff | UC-02 to UC-03 | Same screen: S-F01-02 Shipment details (confirm blood received) |
| Hospital Staff | UC-06 to UC-04 | S-F04-01 → S-F01-01, then menu "Record usage" → S-F02-01 Blood usage record |
| Hospital Staff | UC-06 to UC-05 | S-F04-01 → S-F01-01, then menu "Blood demand" → S-F03-01 Blood demand log |
| Hospital Staff | UC-06 to UC-14 | Entry point "Link in verification email" → S-F03-03 Verification response (after S-F04-01 when signed out) |
| NBC Dept | UC-12 to UC-07 | S-F04-01 Sign in → S-F01-03 Request queue ("signed in as NBC Dept") |
| NBC Dept | UC-07 to UC-08 | S-F01-03 → S-F01-04 Order review and dispatch ("opens a request") |
| NBC Dept | UC-07 to UC-09 | S-F01-03 → S-F02-02 Inventory dashboard ("Check stock") |
| NBC Dept | UC-09 to UC-08 | S-F02-02 → S-F01-03 ("Back to queue") → S-F01-04 |
| NBC Dept | UC-08 to UC-07 | S-F01-04 → S-F01-03 ("Back to queue") |
| NBC Dept | UC-12 to UC-09 | S-F04-01 → S-F01-03, then menu "Inventory" → S-F02-02 |
| NBC Dept | UC-09 to UC-10 | S-F02-02 → S-F02-03 Near-expiry units ("Near-expiry units") |
| NBC Dept | UC-09 or UC-10 to Flow 1 | S-F02-02 → S-F01-03 ("Back to queue"), or menu "Requests" |
| NBC Dept | alert (UC-11) to UC-09 | Entry point "Below-threshold alert received" (a notification, no screen) → S-F02-02 Inventory dashboard |
| NBC Forecasting Analyst | UC-12 to UC-13 | S-F04-01 Sign in → S-F03-02 Demand data review ("signed in as analyst") |
| NBC Forecasting Analyst | UC-12 to UC-15 | S-F04-01 → S-F03-02, then menu "Forecast" → S-F03-04 Forecast review and approval; or entry point "Notification: forecast draft ready" → S-F03-04 (after S-F04-01 when signed out) |
| NBC Forecasting Analyst | UC-13 and UC-15 | Not joined by an arrow in the usage flow (asynchronous handoff); on the diagram S-F03-04 → S-F03-02 ("back") is only navigation |
| NBC Operations | UC-12 to UC-16 | S-F04-01 → S-F03-05 Validated forecast ("signed in as NBC Operations") |
| NBC Operations | UC-12 to UC-09 | S-F04-01 → S-F03-05, then menu "Inventory" → S-F02-02 Inventory dashboard |
| NBC Operations | alert (UC-11) to UC-09 | Entry point "Below-threshold alert received" (a notification, no screen) → S-F02-02 |

The Hospital Staff diagram has 7 screens and the Dept diagram has 5, so one diagram per actor stays readable; the actors share Sign in and the dashboard.
