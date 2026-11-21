# Screen Spec: S-F01-01 Blood request form

| Field | Value |
|---|---|
| Screen ID | S-F01-01 |
| Screen name | Blood request form (Vietnamese UI title: "Yêu cầu cấp máu") |
| Actor | Hospital Staff |
| Priority | Must |
| Feature | F-01 Blood Ordering & Fulfillment |
| Activity diagrams | AD-UC-01 |
| Mockup images | `img/S-F01-01.png` (default), `img/S-F01-01-numbered.png`, four state images (section 2) |
| Frame width | 1440px (desktop, as in the project UI conventions v2.0) |
| Figma frame | https://www.figma.com/design/oR1xULg2oLdGezDpqdwhdz (frames "S-F01-01 · Yêu cầu cấp máu · 1 to 5") |
| Status | Draft |

## 1. Purpose

**Shown when:** Hospital Staff need blood and enter the blood type, quantity, blood product, urgency, the date by which the blood is needed, an optional reason and a contact person, then submit the request to NBC.

**The user leaves this screen when:** the request is submitted (success state, then "Xem chi tiết lô hàng" or "Gửi yêu cầu khác"), or the user opens another item of the menu, or signs out.

## 2. Mockup

![S-F01-01](img/S-F01-01.png)

**Numbered:** each number is the # of the element inventory in section 3. Elements 5 (Error banner) and 17 (Sending banner) appear only in the error and loading states, so they are numbered on those images. Elements 18 to 21 appear only in the success state.

| Default, numbered | Error, numbered |
|---|---|
| ![S-F01-01 numbered](img/S-F01-01-numbered.png) | ![S-F01-01 error numbered](img/S-F01-01-error-numbered.png) |

| State | Image |
|---|---|
| Default (with data) | `img/S-F01-01.png` |
| Empty | `img/S-F01-01-empty.png` |
| Loading | `img/S-F01-01-loading.png` |
| Error | `img/S-F01-01-error.png` (invalid fields, FR-01-06; the uncertain-result error is described in section 4 only) |
| Success | `img/S-F01-01-success.png` |

All colours, fonts and corners come from the design tokens in
[ui-conventions.md](../ui-conventions.md#6-design-tokens).

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | Menu | Navigation | static; items allowed for Hospital Staff (FR-04-03) | Yes | none |
| 2 | Account info | Text | `hospital.name`, role "Nhân viên bệnh viện" from the session | Yes | none |
| 3 | Sign out | Secondary button | static | Yes | none |
| 4 | Title | Header | static: "Yêu cầu cấp máu" | Yes | |
| 5 | Error banner | Banner | static: "Chưa tạo yêu cầu. Vui lòng sửa các trường được đánh dấu và gửi lại." | No | shown only after a failed validation |
| 6 | Blood type | Single-choice chips (8) | `request.blood_type`: A+, A−, B+, B−, AB+, AB−, O+, O− | Yes | exactly one of the 8 valid types (BR-01) |
| 7 | Quantity | Number input | `request.quantity`, unit "đơn vị" | Yes | whole number of 1 or more (BR-01) |
| 8 | Blood product | Select | `request.blood_product`, list from the approved product catalogue | Yes | one value of the list |
| 9 | Urgency | Select | `request.urgency`, list from the system | Yes | one value of the list (BR-01) |
| 10 | Needed by | Date and time input | `request.needed_by`, format dd/MM/yyyy HH:mm, Vietnam time | Yes | valid date and time; not in the past |
| 11 | Reason | Multi-line text | `request.reason` | No | maximum length [TBD] |
| 12 | Contact name | Text input | `request.contact_name` | Yes | not empty |
| 13 | Contact phone | Text input | `request.contact_phone` | Yes | not empty; format [TBD] |
| 14 | Submit request | Primary button | label "Gửi yêu cầu cấp máu" | Yes | disabled while the request is being sent |
| 15 | Clear form | Secondary button | label "Xóa biểu mẫu" | No | disabled while the request is being sent |
| 16 | Next-steps card | Card | static: Đang chờ xử lý, Đang vận chuyển, Đã xác nhận nhận máu with one line each | Yes | none |
| 17 | Sending banner | Banner | static: "Đang gửi yêu cầu cấp máu. Vui lòng chờ hệ thống xác nhận." | No | shown only in the loading state |
| 18 | Success banner | Banner | static: "Đã gửi yêu cầu cấp máu. Yêu cầu đang chờ NBC xử lý." | No | shown only after the server confirms |
| 19 | Request summary | List of rows | `request.id`, `request.blood_type`, `request.quantity`, `request.blood_product`, `request.urgency`, `request.needed_by`, `request.reason`, `request.contact_name` and `request.contact_phone`, `request.created_at` | No | read-only |
| 20 | Status chip | Chip | `request.status` = Pending, label "Đang chờ xử lý" (marker, text and colour) | No | |
| 21 | View shipment | Primary button | label "Xem chi tiết lô hàng" | No | |

In the success state the Clear form button is replaced by "Gửi yêu cầu khác" (secondary button).

## 4. States

| State | What the user sees | Trigger |
|---|---|---|
| Default (with data) | Form filled with a valid request; Submit request enabled | Screen opened and the user has entered all required values |
| Empty (no data) | Form with no value selected and no text; hints under the fields; Submit request enabled | Screen opened for a new request, or after "Gửi yêu cầu khác" or "Xóa biểu mẫu" |
| Loading | Sending banner; all fields disabled; Submit request shows a spinner and "Đang gửi yêu cầu…"; Clear form disabled | The user pressed Submit request and the server has not answered |
| Error | Error banner and a message under each invalid field: "Chọn nhóm máu.", "Nhập số đơn vị là số nguyên từ 1 trở lên.", "Chọn chế phẩm máu.", "Chọn mức độ khẩn.", "Nhập thời hạn theo dd/MM/yyyy HH:mm.", "Nhập họ tên người liên hệ.", "Nhập số điện thoại liên hệ."; no request is created, values already entered are kept (FR-01-06). Uncertain result: "Chưa xác định được kết quả thao tác. Hãy tải lại trạng thái hiện tại trước khi thử lại." | Validation fails, or the connection is lost before the server answers |
| Success / confirmation | Success banner, request summary with status chip "Đang chờ xử lý", buttons "Xem chi tiết lô hàng" and "Gửi yêu cầu khác" | The server created the request with status Pending (FR-01-01) |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Blood type chip | click | Selects that type; any other selected type is cleared | stays |
| 2 | Quantity, Blood product, Urgency, Needed by, Reason, Contact name, Contact phone | enter or choose | Stores the value in the form; clears the message of that field | stays |
| 3 | Submit request | click | Checks the values (FR-01-06); if valid, creates the request as Pending and puts it in the NBC queue (FR-01-01); else shows the error state | stays (success or error state) |
| 4 | Clear form | click | Empties every field and message | stays |
| 5 | View shipment | click (success state) | Opens the request in the shipment list | S-F01-02 |
| 6 | Gửi yêu cầu khác | click (success state) | Shows an empty form | stays |
| 7 | Menu item "Chi tiết lô hàng" | click | Opens the shipment screen | S-F01-02 |
| 8 | Menu item "Ghi nhận sử dụng máu" | click | Opens the blood usage record | S-F02-01 |
| 9 | Menu item "Nhu cầu máu theo kế hoạch" | click | Opens the demand log | S-F03-01 |
| 10 | Menu item "Phản hồi xác minh" | click | Opens the verification response | S-F03-03 |
| 11 | Menu item "Tổng quan tồn kho" | click | Opens the inventory dashboard | S-F02-02 |
| 12 | Sign out | click | Ends the session | S-F04-01 |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | A request is created only if blood type, quantity, blood product, urgency, needed-by date and contact are all valid; otherwise nothing is created. | FR-01-06, BR-01 |
| SR-002 | After a failed validation every value already entered stays in the form. | FR-01-06 |
| SR-003 | The screen shows "success" only after the server confirms the new request; while it waits, the fields and buttons are disabled so the request cannot be sent twice. | ui-conventions §3 |
| SR-004 | The new request has status Pending; the screen does not show it as Dispatched or Received. | FR-01-01 |
| SR-005 | Hospital Staff see and create requests only for their own hospital. | BR-06, FR-04-03 |
| SR-006 | State is never shown by colour alone: a selected blood type has a tick and a border, an error has an icon and text, a status has a marker and a word. | ui-conventions §5 |

## 7. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| FR-01-01 | Provides the form and the Submit action, and shows the new Pending request in the success state |
| FR-01-06 | Shows a reason for each invalid field and keeps the entered values so the user can correct and resubmit |
| FR-04-03 | Shows only the menu items and the hospital data allowed for the signed-in Hospital Staff |

## 8. Responsive and accessibility notes

- Frame width 1440px; the smallest supported width is 360px. The mockups are drawn at 1440px only. The two-column fields stack on a narrow window.
- Every field has a visible label; required fields are marked with *, and the Reason label says "(không bắt buộc)".
- Error messages sit directly under their field and the banner announces the failure (`role="alert"`); the sending and success banners use `role="status"`.
- Keyboard: Tab follows the reading order, a focus outline is visible, Enter on Submit request sends the form.
- Body text contrast at least 4.5:1; borders of inputs and chips at least 3:1.

## 9. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: Blood product, Needed by, Reason, Contact name and Contact phone are not in FR-01-01 or FR-01-06 (only blood type, quantity, urgency), nor in the SD-UC-01 payload. Update W1 and SD-UC-01, or remove the fields.] | Yes | Open |
| 2 | [NEEDS CLARIFICATION: which urgency values exist?] | Yes | Open. The mockup shows "[Giá trị mức độ khẩn]". |
| 3 | [NEEDS CLARIFICATION: which blood products are in the catalogue?] | Yes | Open. The mockup shows "Hồng cầu lắng" as a sample. |
| 4 | [NEEDS CLARIFICATION: may Needed by be in the past, and must it fit the urgency?] | No | Open. Default: not in the past. |
| 5 | [NEEDS CLARIFICATION: phone number format and maximum length of Reason?] | No | Open. |
| 6 | [NEEDS CLARIFICATION: is Reason required for the most urgent level?] | No | Open. Default: optional. |
| 7 | Three colours in the mockup (border-strong, warning-bg, error-bg) are proposed and not yet in the project UI conventions. | No | Open |

---

## Completion checklist

- [ ] The mockup is a separate cropped image file, named with the Screen ID, at the frame width. *(export from Figma)*
- [ ] The numbered image exists, and its numbers match the # column of the element inventory. *(to make)*
- [x] Every visible element in the mockup appears in the element inventory.
- [ ] Every colour in the mockup is a design token from ui-conventions.md. *(three proposed colours, see question 7)*
- [x] Every input element has a validation rule or an explicit "none".
- [x] All five states are filled in.
- [x] Every navigation target is a Screen ID that exists in the Screen List.
- [x] Every element that displays data names the field it displays.
- [ ] Every linked FR exists in the Function List, and every action of this screen in AD-UC-01 is covered by an interaction row. *(AD-UC-01 not checked)*
