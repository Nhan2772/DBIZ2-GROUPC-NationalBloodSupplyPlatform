# NBSP — National Blood Supply Chain Platform


**NBSP** connects the National Blood Center (NBC) with receiving hospitals to manage blood requests, dispatch and receipt, hospital blood usage, inventory visibility, and demand forecasting. It uses the existing **Blood Management Software (BMS)** as an external inventory source rather than replacing it.

The four human user roles are **Hospital Staff**, **NBC Blood Storage and Distribution Department**, **NBC Forecasting Analyst**, and **NBC Operations**. BMS is an external system; scheduled synchronisation is a system-initiated process. The authoritative actors, functions, and boundaries are in [W0 Scope](function-list/w0-scope.md), the [Context Diagram](context/context-diagram.md), and the [Use Case Diagram](use-case/use-case-diagram.md).

**Who should read this file?** Human reviewers can start at [section 8](#8-reading-order-for-people). AI coding agents and contributors must read sections 1–7 **before** working on requirements, UI, code, or tests. This README is a navigation and governance guide, **not a replacement** for the files it links.

---

## 1. Rules for AI agents and contributors

1. **Treat repository documents as the source of truth.** Do not infer features, data fields, API contracts, thresholds, or states from industry conventions. If a source is missing or contradictory, record the uncertainty instead of turning a guess into a requirement.
2. **Read the owning document first.** Use the ownership table in section 3; proposed changes start with that owner and then propagate to dependent documents. Do not fix a conflict by silently rewriting a downstream screen.
3. **Trace by identifier.** Cite `StR-xx`, `SyR-xx`, `UC-xx`, `F-xx`, `FR-xx-xx`, `BR-xx`, `AD-UC-xx`, `SD-UC-xx`, and `S-Fxx-nn` wherever relevant. Preserve the existing identifiers.
4. **Never invent or renumber IDs.** Check the owning file and reuse existing IDs only for the same concept. Propose additions explicitly before assigning new IDs; mark removals or superseded material instead of recycling codes.
5. **Respect scope and effective priority.** [W0 Table 3](function-list/w0-scope.md#table-3-wont-have-recorded-never-dropped-silently) lists exclusions. [W1](function-list/w1-function-list.md) defines functions and EARS criteria; [W4](function-list/w4-priorities.md) determines the *effective* Must/Should/Could priority. Do not substitute W1's *initial* priority.
6. **Keep business state distinct from UI feedback.** Request statuses include `Pending`, `Dispatched`, `Received`, and `Not approved` (W0 `BR-02`). “Đang vận chuyển” is an explanatory UI label for `Dispatched`, **not a new persisted status**. Error, Empty, Loading, and Success are UI states, not additional business statuses.
7. **Follow external design for screens.** [Screen List](external-design/screen-list.md) owns Screen IDs and screen purpose; [Screen Transition](external-design/screen-transition.md) owns navigation; each Screen Spec owns elements, interactions, and UI states; [UI Conventions](external-design/ui-conventions.md) owns common language, layout, tokens, and messages. Do not create a new screen for every message or background job.
8. **Use English for documentation and Vietnamese for the app.** Technical identifiers, IDs, and system status codes remain unchanged; user-facing headings, buttons, labels, errors, and success messages follow the Vietnamese wording in UI Conventions.
9. **Keep design artifacts in sync.** Wireframes target desktop **1440 × 1024 px**; a screen may scroll vertically where the content requires it. For each screen, keep its Screen Spec, five state images, and numbered images aligned. Export images from the design source rather than editing PNGs by hand.
10. **Preserve decisions and history.** Do not retroactively rewrite accepted decisions, version history, change logs, or verification logs. When an earlier decision is superseded, document the replacement and update dependent sources.
11. **Escalate unknowns without guessing.** In the file being worked on, add an Open Question using `[NEEDS CLARIFICATION: ...]`, identify its owner and whether it blocks implementation. Distinguish an assumption from an approved decision.
12. **Report verification truthfully.** For each change, name files touched; list traceability checks, missing references, unverified diagrams/screens, and remaining open questions. Do not report a screen as implemented merely because its mockup exists.

## 2. Repository map

**Important:** The NBSP repository uses **top-level lesson folders**; unlike the LabDesk teaching example, these folders are **not nested inside `docs/`**.

```text
README.md                         This repository guide
project/
  project-brief.md               Product, problem, goal, team roles
context/
  cvca.md                        Stakeholder ecosystem and value flows
  context-diagram.md             System boundary and external information flows
use-case/
  use-case-diagram.md            Actors, use cases, relationships
  SD-UC-01.md ... SD-UC-17.md    Sequence diagrams (17 files)
architecture/
  architecture.md                Stakeholder/system requirements, layers, design, decisions
function-list/
  w0-scope.md                    UC inventory, features, exclusions, data groups, business rules
  w1-function-list.md            EARS criteria, L2 functions, L3 work items
  w2-cost.md                     Work estimates and costing
  w4-priorities.md               Effective MoSCoW priorities
  w5-handover.md                 Delivery baseline and implementation handover
  w6-cashflow.md                 Financial appraisal and scenarios
activity/
  activity-index.md              AD inventory and reasons for omitting an AD
  AD-UC-xx.md                    Activity diagrams (existing files; see section 7)
external-design/
  usage-flow.md                  Actor journeys
  screen-list.md                 Canonical list of 13 Screen IDs
  screen-transition.md           Navigation between screens
  ui-conventions.md             Project-wide UI conventions (English spec / Vietnamese UI)
  screens/
    s-fxx-nn/
      screen-spec-s-fxx-nn.md    Screen specification (see naming exceptions in section 7)
      img/
        s-fxx-nn-default.png
        s-fxx-nn-default-numbered.png
        s-fxx-nn-empty.png
        s-fxx-nn-empty-numbered.png
        s-fxx-nn-loading.png
        s-fxx-nn-loading-numbered.png
        s-fxx-nn-error.png
        s-fxx-nn-error-numbered.png
        s-fxx-nn-success.png
        s-fxx-nn-success-numbered.png
```

Only the image and Screen Spec **naming patterns** are illustrated above: some screens have additional states, overlays, or filenames. Inspect the actual folder and its Screen Spec before consuming it programmatically. All paths in this tree refer to existing top-level directories, **except that the `s-fxx-nn/` part is a pattern**.

## 3. Which file owns which fact

| Fact or decision | Owning file / section | Typical consumers |
|---|---|---|
| Product name, target client, problem, goal, team members | [`project/project-brief.md`](project/project-brief.md) | All documentation and product introductions |
| Stakeholder ecosystem and value flows | [`context/cvca.md`](context/cvca.md) | Architecture and system boundary |
| System boundary, external entities and data flows | [`context/context-diagram.md`](context/context-diagram.md) | Use cases, architecture, sequence diagrams |
| `StR-xx` stakeholder requirements and `SyR-xx` system requirements | [`architecture/architecture.md` §§1–2](architecture/architecture.md) | W0, W1 and traceability |
| Logical actors and use-case relations | [`use-case/use-case-diagram.md`](use-case/use-case-diagram.md) | W0, sequence diagrams and flows |
| Use-case inventory `UC-xx` and assigned feature `F-01` to `F-04` | [`function-list/w0-scope.md` Tables 1–2](function-list/w0-scope.md) | W1, Screen List, ADs |
| Won't-have / excluded functionality | [`function-list/w0-scope.md` Table 3](function-list/w0-scope.md) | Every implementation or design task |
| Persisted and external data groups | [`function-list/w0-scope.md` Table 4](function-list/w0-scope.md) | Data model, Screen Spec data sources |
| Business rules `BR-xx` | [`function-list/w0-scope.md` Business rules](function-list/w0-scope.md#business-rules) | ADs, backend validation, tests |
| Functions `FR-xx-xx`, EARS criteria, L3 work items | [`function-list/w1-function-list.md`](function-list/w1-function-list.md) | Screens, API implementation, testing |
| Effective priority and scope cuts | [`function-list/w4-priorities.md`](function-list/w4-priorities.md) | Release planning, W5, implementation order |
| Estimates, rates and work-item costing | [`function-list/w2-cost.md`](function-list/w2-cost.md) | W4, W5, financial model |
| Delivery scenarios and handover baseline | [`function-list/w5-handover.md`](function-list/w5-handover.md) | DBIZ3 task breakdown |
| Cash flow, NPV/IRR and forecast assumptions | [`function-list/w6-cashflow.md`](function-list/w6-cashflow.md) | Project business case |
| Functional/physical architecture and technical decisions | [`architecture/architecture.md` §§3–5](architecture/architecture.md) | Backend and integration implementation |
| Message-level behaviour for one UC | [`use-case/SD-UC-xx.md`](use-case/) | Backend interactions and tests |
| Process branches and object states | [`activity/AD-UC-xx.md`](activity/) | Screen states, backend and tests |
| Whether each UC needs an AD | [`activity/activity-index.md`](activity/activity-index.md) | Analysis completeness checks (verify against W0; see section 7) |
| Actor task order and cross-role handoffs | [`external-design/usage-flow.md`](external-design/usage-flow.md) | Screen transitions and end-to-end tests |
| Screen inventory, actors, priority, purpose | [`external-design/screen-list.md`](external-design/screen-list.md) | Screen Specs, navigation |
| Screen-to-screen routes | [`external-design/screen-transition.md`](external-design/screen-transition.md) | Screen interactions |
| Vietnamese wording, formats, components, five UI states, tokens | [`external-design/ui-conventions.md`](external-design/ui-conventions.md) | Every screen and UI test |
| Elements, data sources, per-screen states and interactions | `external-design/screens/<screen-id>/screen-spec.md` | Wireframes, frontend implementation and tests |

**If sources disagree:** identify the owning file; cite both conflicting locations; record a proposed resolution. Do not silently make one source agree with another, especially if the conflict affects an EARS criterion, business rule, permission, or priority.

## 4. Identifier conventions and traceability

| Kind | Pattern | Example | Authority |
|---|---|---|---|
| Stakeholder requirement | `StR-nn` | `StR-01` | Architecture §1 |
| System requirement | `SyR-nn` | `SyR-01` | Architecture §2 |
| Use case | `UC-nn` | `UC-08` | W0 Table 1 |
| Feature | `F-nn` | `F-02` | W0 Table 2 |
| L2 function | `FR-xx-yy` | `FR-02-03` | W1 |
| L3 work item | `FR-xx-yy-FE`, `-BE`, or `-INT` | `FR-02-03-BE` | W1 L3 work items |
| Business rule | `BR-nn` | `BR-03` | W0 Business rules |
| Sequence diagram | `SD-UC-nn` | `SD-UC-08` | `use-case/SD-UC-08.md` |
| Activity diagram | `AD-UC-nn` | `AD-UC-15` | `activity/AD-UC-15.md` |
| Screen | `S-Fxx-nn` | `S-F03-04` | Screen List |
| Screen-local rule | `SR-nnn` or the spec's local convention | `SR-001` | The relevant Screen Spec |
| Architecture decision | `ADR-nnn`, where referenced | `ADR-001` | Architecture §5; dedicated `adr/` directory not yet present |

The traceability direction is:

```text
Stakeholder requirement (StR)
    → System requirement (SyR)
    → Use case (UC) / Feature (F)
    → Function / EARS acceptance criterion (FR)
    → Process + interaction diagrams (AD / SD)
    → Screen ID + Screen Spec (S)
    → Wireframes and acceptance tests
```

**Numbering caution:** the local `F-xx` labels in *Architecture §3 (Functional design)* describe subfunctions and do **not** match the four business features `F-01`–`F-04` in **W0 Table 2**. For feature ownership and `FR-xx-yy` prefixes, always use **W0/W1**. Do not conflate these two numbering schemes.

## 5. Common workflows for editing and building

**Before every change.** Read this README, the [Project Brief](project/project-brief.md), [W0](function-list/w0-scope.md), the owning file from section 3, and its immediate references. Read [W4](function-list/w4-priorities.md) before making any release-scope or build-priority decision.

**Change a function or business rule.** Update the owning W1 EARS criterion or W0 BR, respectively; review W4 and cost/handover impact; trace affected AD/SD, Screen Specs, UI messages and tests. Do not change only a button label if the behaviour being changed is a business rule.

**Add or revise a use case.** Update the Use Case Diagram and W0 inventory, then W1 (when functionality changes), corresponding SD/AD, Activity Index, Usage Flow, Screen List and Screen Transition as needed. A diagram existing by filename does not guarantee it is up to date.

**Add or revise a screen.** Verify that its Screen ID exists in the Screen List and its routes exist in Screen Transition. Write the Screen Spec against W0/W1, relevant AD/SD, Usage Flow and UI Conventions. Match elements and states to the PNGs (including numbered exports). Use the canonical [`S-F01-01` Screen Spec](external-design/screens/s-f01-01/screen-spec-s-f01-01.md) as a *format reference*, not as a source of requirements for other use cases.

**Build a screen in DBIZ3.** Read its Screen Spec → linked FRs (W1) → linked BRs (W0) → AD/SD → UI Conventions → relevant role/route definitions. Implement the described states and server-verified outcomes. Do not infer receipt from dispatch, a successful data load from a successful transaction, or data permissions from visual hiding alone.

**Implement an integration or background job.** Start from W1, the system boundary, architecture and SD/AD. In particular, `UC-17` reads and validates the BMS inventory snapshot; `UC-11` checks thresholds after valid synchronisation. The UI must not treat a dashboard reload as initiating a new BMS synchronisation unless an approved requirement says so.

**Check traceability without changing files.** Return five separate findings lists (use “none” where applicable): **A** user-visible FRs absent from Screen Specs; **B** screen components without linked FRs; **C** actor actions in ADs without a documented screen; **D** Screen Spec navigation targets absent from Screen Transition; **E** orphaned or missing paths among Usage Flow, Screen List, and Screen Transition. Note which documents and revisions were checked.

## 6. Notation, UI and product conventions

- **Language:** the documentation, screen specifications and explanatory notes are in **English**; the **application UI is Vietnamese**. Internal identifiers and statuses remain in English.
- **Date/time:** `dd/MM/yyyy`, `HH:mm` (24-hour) and `dd/MM/yyyy HH:mm` for timestamps; use Vietnamese time (`UTC+07:00`) where applicable. Use the authoritative rules in [UI Conventions §2](external-design/ui-conventions.md).
- **Numbers and inventory:** use `đơn vị` for blood-unit quantities. Per W0 `BR-01`, release 1 handles **red blood cells** and **350 ml per bag**; do not silently extend this to other products or bag sizes.
- **UI dimensions:** standard desktop Figma frame **1440 × 1024 px**. Sidebar, top bar, typography, colours, spacing, message content and accessibility requirements come from [UI Conventions](external-design/ui-conventions.md), not individual AI preferences.
- **UI states:** Default, Empty, Loading, Error and Success are the baseline presentation states. Some screen-specific states or overlays may also be documented; read their Screen Spec. Always distinguish `Empty` (valid query, zero results) from `Error` (could not retrieve or validate data).
- **Role scoping:** Hospital Staff access their own hospital's records; NBC departments see only data permitted to their role. Authorization must be enforced by the system, not only by UI visibility (`BR-06`, `FR-04-03`).
- **Flow diagrams:** Mermaid code is embedded in `.md`. Sequence diagrams use `sequenceDiagram`; activity diagrams in this repository can use `swimlane-beta` syntax. Verify the diagram renderer and the current file instead of substituting another diagram's logic.
- **Business workflow limits:** blood receipt must be explicitly confirmed once (`BR-03`); forecast publication contains only approved items (`BR-07`); allocation optimisation and training the prediction model are **outside release 1** (W0 Table 3).
- **Financial estimates:** W2, W5 and W6 contain the team’s planning/costing model. Do not present projected costs, NPV or IRR as observed financial results.

## 7. Repository state and known documentation gaps

This section is an **audit snapshot of `main` on 22 November 2026**, not an approval that every file is complete or consistent. The source files and W4 priorities always take precedence.

| Area | Files found | State / attention required |
|---|---|---|
| Project | `project/project-brief.md` | Present. It refers to `project/decision-log.md`, which is **not present** in the current tree; do not invent its content. |
| Context | `context/cvca.md`, `context/context-diagram.md` | Present. |
| Use cases | `use-case/use-case-diagram.md`, `SD-UC-01.md` through `SD-UC-17.md` | Present; diagram content still needs normal review and trace checks. |
| Architecture | `architecture/architecture.md` | Present. It refers to an `architecture/adr/` directory that does **not** exist on `main`; decisions appear in Architecture §5. |
| Function List | W0, W1, W2, W4, W5, W6 | All six current filenames exist as mapped in section 2. **W4 effective priorities** govern scope. |
| Activity diagrams | `activity-index.md` and **10 `AD-UC-xx.md` files** | Present. The Activity Index contains stale feature-number references and historical “open” findings; reconcile against current W0/W1 before treating its rows as authoritative. |
| External design | `usage-flow.md`, `screen-list.md`, `screen-transition.md`, `ui-conventions.md` | Present; Screen List declares **13 screens**. |
| Screen Specs | **12 screen folders** with Screen Specs and `img/` | Planned `S-F03-01` folder/spec is **absent**. Existing screens may be drafts and require final review. |
| Screen Spec filenames | `s-f02-02/screen-specs-s-f02-02.md`, `s-f03-03/screen-spec-s-f03-03-2.md` | These two names **differ from** the canonical `screen-spec-s-fxx-nn.md` pattern; resolve references before renaming. |
| Development | No app source code / automated test suite at repository root | This is a **design handover**, not a working deployable application. |

**Other consistency checkpoints:** the current Screen List marks `S-F03-02` as `Must`, while the associated W4 FR-03-02/FR-03-03 effective priorities are `Should`; the discrepancy should be reviewed in the *owning* sources. Do not silently upgrade an FR’s priority to match a screen. The original root README also linked several old paths (for example `function-list/w1-function-list-2.md` and nested `external-design/usage-flow/`); this replacement README uses the actual paths on `main`.

### Screen Spec availability at this snapshot

| Screen | Screen Spec on `main` | Notes |
|---|---|---|
| `S-F04-01` | [Sign in](external-design/screens/s-f04-01/screen-spec-s-f04-01.md) | Shared login |
| `S-F01-01` | [Blood request form](external-design/screens/s-f01-01/screen-spec-s-f01-01.md) | Formatting baseline |
| `S-F01-02` | [Shipment details and receipt](external-design/screens/s-f01-02/screen-spec-s-f01-02.md) | Hospital Staff |
| `S-F01-03` | [Request queue](external-design/screens/s-f01-03/screen-spec-s-f01-03.md) | NBC Dept |
| `S-F01-04` | [Order review and dispatch](external-design/screens/s-f01-04/screen-spec-s-f01-04.md) | NBC Dept |
| `S-F02-01` | [Blood usage record](external-design/screens/s-f02-01/screen-spec-s-f02-01.md) | Hospital Staff |
| `S-F02-02` | [Inventory dashboard](external-design/screens/s-f02-02/screen-specs-s-f02-02.md) | Nonstandard filename; three roles |
| `S-F02-03` | [Near-expiry units](external-design/screens/s-f02-03/screen-spec-s-f02-03.md) | NBC Dept |
| `S-F03-01` | **Not yet present** | Planned in Screen List; do not create an assumed spec |
| `S-F03-02` | [Demand data review](external-design/screens/s-f03-02/screen-spec-s-f03-02.md) | NBC Forecasting Analyst |
| `S-F03-03` | [Verification response](external-design/screens/s-f03-03/screen-spec-s-f03-03-2.md) | Nonstandard filename |
| `S-F03-04` | [Forecast review and approval](external-design/screens/s-f03-04/screen-spec-s-f03-04.md) | NBC Forecasting Analyst |
| `S-F03-05` | [Published forecast](external-design/screens/s-f03-05/screen-spec-s-f03-05.md) | NBC Operations |

## 8. Reading order for people

| Step | Area | Read this | Why |
|---|---|---|---|
| 1 | Project | [Project Brief](project/project-brief.md) | Product context, client, goals and ownership |
| 2 | Stakeholders | [CVCA](context/cvca.md) | Value exchanges and stakeholder relationships |
| 3 | System boundary | [Context Diagram](context/context-diagram.md) | NBSP vs external systems and data flows |
| 4 | Requirements | [Architecture §§1–2](architecture/architecture.md) | `StR` and `SyR` requirements |
| 5 | Use cases | [Use Case Diagram](use-case/use-case-diagram.md) and [SD files](use-case/) | Actors, UCs and message-level interactions |
| 6 | System design | [Architecture §§3–5](architecture/architecture.md) | Functional/physical architecture and decisions |
| 7 | Scope | [W0 Scope](function-list/w0-scope.md) | UC inventory, four business features, exclusions, data groups and BRs |
| 8 | Functional requirements | [W1 Function List](function-list/w1-function-list.md) | `FR` EARS acceptance criteria and work breakdown |
| 9 | Delivery priorities | [W4 Priorities](function-list/w4-priorities.md) | Effective Must/Should/Could |
| 10 | Estimation | [W2 Cost](function-list/w2-cost.md), [W5 Handover](function-list/w5-handover.md) | Work effort and build baseline |
| 11 | Business model | [W6 Cash Flow](function-list/w6-cashflow.md) | Assumptions, scenarios, NPV/IRR/payback |
| 12 | Process design | [Activity Index](activity/activity-index.md) and [ADs](activity/) | Key decisions, branches and business states; check the index caveat in §7 |
| 13 | User journeys | [Usage Flow](external-design/usage-flow.md) | User work by actor and cross-role handoffs |
| 14 | External design | [Screen List](external-design/screen-list.md), [Screen Transition](external-design/screen-transition.md), [UI Conventions](external-design/ui-conventions.md) | Screen inventory, navigation and project-wide design rules |
| 15 | Individual screens | [Screen Specs](external-design/screens/), beginning with [S-F01-01](external-design/screens/s-f01-01/screen-spec-s-f01-01.md) | Data, elements, states, actions and annotated wireframes |

**Before DBIZ3 implementation:** resolve the documented cross-file conflicts and blocking Open Questions in their owning documents, then establish the implementation backlog from W5 + W4. A Markdown file or a PNG being present does **not** mean the related functionality has been approved, built, or tested.
