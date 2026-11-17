# UI Conventions — NBSP: Hospital Staff, Flow 1

> **DBIZ2 Group C · Draft v1.0 · 09/11/2026** · *One-page convention sheet for workshop wireframes*  
> **Journey:** `UC-06` Sign in → `UC-01` Request Blood Provision → **NBC dispatches** → `UC-02` View Shipment Details → `UC-03` Confirm Blood Received.  
> **Screens:** `S-F04-01` Sign in (shared), `S-F01-01` Blood request, `S-F01-02` Shipment details **and** receipt confirmation. NBC dispatch is a **handoff between actors**, not an immediate Hospital screen transition.

## 1. Language and tone

- **UI copy:** English for this workshop; Vietnamese localisation is **to confirm** with the group. Address the user as **“you”**; use clear, professional, nontechnical terms.
- **Action labels = verb + object:** `Sign in`, `Request blood`, `View shipment details`, `Confirm blood received`. Never `OK`, `Submit` or `Process`.
- Distinguish **Pending** (request awaiting dispatch), **Dispatched** (NBC has dispatched), and **Received** (Hospital Staff confirmed arrival). Never equate *request submitted* with *shipment dispatched*.

## 2. Formats

| Item | Format | Example / guidance |
|---|---|---|
| Dates / expiry | `dd/MM/yyyy` | `09/11/2026` |
| Time and ETA | `HH:mm`, 24-hour; ETA includes date and time | `10/11/2026 15:30` (ICT/UTC+07) |
| Blood type | ABO + Rh | `A+`, `O-`, `AB+` |
| Quantity | Show `units` explicitly | `Quantity (units): 4` |
| Urgency | Use the configured options | Do not invent urgency levels |
| Request / shipment / unit ID | Use exact NBSP-assigned code | Do not invent an ID pattern |
| Money | Not used in this journey | No price field |

## 3. Components

| Component | Convention for Flow 1 |
|---|---|
| **Primary button** | At most one main action per screen: `Sign in` (`S-F04-01`), `Request blood` (`S-F01-01`), `Confirm blood received` (`S-F01-02`). Enable receipt confirmation only when status is **Dispatched**; Hospital Staff must confirm actual arrival. |
| **Form + inline validation** | `S-F01-01`: Blood type, quantity, urgency; show field-level reasons and preserve entered values if rejected (**FR-01-06**). |
| **Shipment summary** | `S-F01-02`: read-only unit IDs, blood types, expiry dates, ETA and status (**FR-01-04**). |
| **Status indicator** | Text **and** icon: `Pending` (clock), `Dispatched` (truck), `Received` (check); never colour alone. |
| **Confirmation modal** | Before recording receipt, show shipment summary with `Confirm blood received` / `Back`; **not** a new screen. |
| **Banner / success notice** | State **what happened + what to do next**; show success only after server confirmation. |

## 4. Messages

| Situation | Exact suggested UI copy |
|---|---|
| Request submitted (`FR-01-01`) | “Blood request submitted. Wait for NBC to dispatch it before viewing shipment details.” |
| Invalid request (`FR-01-06`) | “Blood request not created. Correct the highlighted blood type, quantity or urgency field, then request blood again.” |
| Not yet dispatched (`UC-02`) | “Shipment details are not available yet. NBC has not dispatched this request. Check again later.” |
| Premature receipt (`FR-01-07`) | “Receipt not recorded. Shipment is not marked Dispatched. Check its status and try again later.” |
| Duplicate receipt (`FR-01-07`) | “Receipt was already confirmed. No additional blood units were added.” |
| Receipt confirmed (`FR-01-05`) | “Blood receipt confirmed. Received units have been added to your hospital inventory.” |
| Uncertain network result | “Result not verified. Refresh the current status before trying again.” |

## 5. Accessibility

- Text contrast **≥ 4.5:1**; meaningful control boundaries/icons **≥ 3:1** (WCAG 2.2); icons and text accompany every status colour.
- Minimum interactive target **24×24 CSS px**; primary buttons approximately **44px high**; wireframes support **360px** screen width.
- Visible keyboard focus; persistent form labels; errors beside relevant fields and available to screen readers.

## 6. Design tokens — proposed, not yet group-approved

| Token | Value | Use |
|---|---|---|
| `colour/primary` | `#0B5E75` | Main actions, white text (contrast **7.3:1**) |
| `colour/text` / `text-muted` | `#1F2933` / `#475569` | Titles, body, hints |
| `colour/border` / `surface` | `#64748B` / `#F8FAFC` | Fields, cards |
| `colour/success` / `error` / `warning` | `#166534` / `#8A1C2D` / `#9A3412` | Statuses with text and icons |
| `font` / `sizes` | Arial; title `22px`, body `16px`, label `14px` | Typography |
| `spacing` / `radius` | 4px base; 16px padding; 8px gaps; 10px buttons | Layout |

**Boundary:** Do not add a Hospital **dispatch/approval** control, a separate receipt screen, or an automatic `Received` state. The exact urgency values, UI language and visual tokens require group approval; these conventions do not create new FRs.

**Trace:** `usage-flow-total.md`, Hospital Staff Flow 1 (UC-06/01/02/03); `ui-conventions.md` (LabDesk sample structure); W1 FR-01-01, FR-01-04, FR-01-05, FR-01-06 and FR-01-07.
