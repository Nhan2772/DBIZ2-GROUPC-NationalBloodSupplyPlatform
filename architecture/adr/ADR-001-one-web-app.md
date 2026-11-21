# ADR-001: One web app with a Hospital workspace and an NBC workspace

> DBIZ 2 – Group C. NBSP architecture decision.

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-11-19 (architecture v1) |

**Context.** NBSP has four actors in two organisations: Hospital Staff in 180+ receiving hospitals,
and the NBC Blood Storage and Distribution Dept., NBC Forecasting Analyst and NBC Operations at NBC.
Both sides sign in (UC-06, UC-12), and some screens are shared: the inventory dashboard S-F02-02
serves Hospital Staff, the Dept and NBC Operations with a different data scope for each.

**Decision.** Build one responsive web app for every actor, with a Hospital workspace and an NBC
workspace. The workspace and the menu follow the user's role and organisation. No native mobile
app in this release.

**Consequences.** One sign-in screen S-F04-01 serves all four actors. Every screen applies the
role's data scope (FR-04-03), so a shared screen such as S-F02-02 shows each actor only its own
scope. Release 1 wireframes are drawn for desktop browsers at 1440px; narrower windows need their
own decision ([ui-conventions](../../external-design/ui-conventions.md) §2.1). The native app is
to be recorded as Won't-have in W0 Table 3.
