# ADR-006: Send notifications in-app and by email through an external email service

> DBIZ 2 – Group C. NBSP architecture decision.

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-11-19 (architecture v1) |

**Context.** NBSP must reach users who are not looking at the app: below-threshold alerts for the
Dept and NBC Operations (UC-11), verification requests to hospitals (UC-13, UC-14), the answers back
to the analyst, and the "draft forecast ready" notice (FR-03-05). Running a mail server is outside
the team's scope.

**Decision.** All notifications go through one Notification adapter. They are shown in the app and
sent by email through an external email service.

**Consequences.** The email service is an external entity on the Context Diagram. Below-threshold
alerts go by email to the Dept and NBC Operations; inside NBSP they appear only as a banner and a
"Dưới ngưỡng" badge on S-F02-02, with no menu badge ([ui-conventions](../../external-design/ui-conventions.md) OQ-03).
Hospitals are not notified when a request is dispatched (D-11) and get no low-stock alerts (D-18);
they are notified of verification requests. FR-02-04 still says "in-system notification" only and
is to be updated. Email work items are INT rows in W1, and the email subscription is a running cost
in W6.
