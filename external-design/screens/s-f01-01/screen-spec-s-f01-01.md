# Screen Spec: S-F01-01 Blood request form

| Field | Value |
|---|---|
| Screen ID | S-F01-01 |
| Screen name | Blood request form (Vietnamese UI title: "Yêu cầu cấp máu") |
| Actor | Hospital Staff |
| Priority | Must |
| Feature | F-01 Blood Ordering & Fulfillment |
| Use cases / FRs | UC-01 · FR-01-01, FR-01-06 (as in `screen-list.md`) |
| Activity diagrams | AD-UC-01 |
| Usage flow | Hospital Staff Flow 1 "Order blood and Receive" (`usage-flow.md` v2.3): S-F04-01 → **S-F01-01** → S-F01-02 |
| Mockup images | `img/s-f01-01-default.png`, `img/s-f01-01-empty.png`, `img/s-f01-01-loading.png`, `img/s-f01-01-error.png`, `img/s-f01-01-success.png`; numbered: the same five names ending in `-numbered.png` (section 2) |
| Frame size | 1440 px wide (desktop, UI conventions v2.0 §2.1). Height grows with the content: 1134 px (default, empty), 1203 px (loading, error), 1024 px (success); the page scrolls vertically |
| Figma frame | https://www.figma.com/design/oR1xULg2oLdGezDpqdwhdz (frames "S-F01-01 · Yêu cầu cấp máu · 1 to 5") |
| Version | v1.1, 21/11/2026 (changes at the end of this file) |
| Status | Draft |

## 1. Purpose

**Shown when:** Hospital Staff need blood. They enter the blood type, quantity, blood product, urgency, the date and time by which the blood is needed, an optional reason and a contact person, then submit the request to NBC (UC-01). It is the first screen after sign-in for Hospital Staff (`screen-transition.md`, section 1).

**The user leaves this screen when:** the request is submitted and the user presses "Xem chi tiết lô hàng" (to S-F01-02), opens another menu item, or signs out. "Gửi yêu cầu khác" stays on this screen with an empty form.

## 2. Mockup

![S-F01-01 default](img/s-f01-01-default.png)

**Layout (all states):** sidebar on the left (brand, role group, menu for Hospital Staff); top bar (hospital name, role, Sign out); breadcrumb, title and subtitle; main card on the left of the content area (the form, or the request summary in the success state); "Sau khi gửi yêu cầu" card on the right.

**Numbered images:** each number is the # in the element inventory (section 3). Element 9 (error banner) appears only in the error state and element 10 (sending banner) only in the loading state. Elements 29 and 30 appear only in the error state; on the error image they are numbered once, on the Quantity field. Elements 31 to 35 appear only in the success state, where they replace elements 11 to 28 in the main card.

| State | Numbered image | Numbers shown |
|---|---|---|
| Default | ![](img/s-f01-01-default-numbered.png) | 1–8, 11–28 |
| Empty | ![](img/s-f01-01-empty-numbered.png) | 1–8, 11–28 |
| Loading | ![](img/s-f01-01-loading-numbered.png) | 1–8, 10–28 |
| Error | ![](img/s-f01-01-error-numbered.png) | 1–9, 11–14, 16, 18, 20, 22–25, 27–30 |
| Success | ![](img/s-f01-01-success-numbered.png) | 1–8, 31–35 |

| State | Image | What it shows |
|---|---|---|
| Default (with data) | `img/s-f01-01-default.png` | All fields filled, O− selected, Submit enabled |
| Empty | `img/s-f01-01-empty.png` | No value, placeholders and hints under the fields |
| Loading | `img/s-f01-01-loading.png` | Sending banner, fields disabled, Submit with spinner |
| Error | `img/s-f01-01-error.png` | Invalid fields (FR-01-06): banner plus a message under each invalid field; values kept. The uncertain-result error is described in section 4 only |
| Success | `img/s-f01-01-success.png` | Success banner, request summary, status "Đang chờ xử lý" |

All colours, fonts and corners come from the design tokens in the UI conventions v2.0, section 6 ([ui-conventions.md](../ui-conventions.md#6-design-tokens)), except the colours listed in open question 7.

## 3. Element inventory

Vietnamese labels are written as they appear in the mockup.

### 3.1 Page frame (all states)

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | Brand | Logo and text | static: "NBSP", "Chuỗi cung ứng máu" | Yes | none |
| 2 | Menu | Navigation | static; role group "NHÂN VIÊN BỆNH VIỆN" and the items allowed for Hospital Staff (FR-04-03): "Yêu cầu cấp máu" (current, marked by background, left bar and bold text), "Chi tiết lô hàng", "Ghi nhận sử dụng máu", "Nhu cầu máu theo kế hoạch", "Phản hồi xác minh", "Tổng quan tồn kho" | Yes | none |
| 3 | Account info | Text | `hospital.name` ("[Tên bệnh viện]") and the role "Nhân viên bệnh viện", from the session | Yes | none |
| 4 | Sign out | Secondary button | label "Đăng xuất" | Yes | none |
| 5 | Breadcrumb | Text | static: "Yêu cầu cấp máu" | Yes | none |
| 6 | Title | Header | static: "Yêu cầu cấp máu" | Yes | none |
| 7 | Subtitle | Text | static: "Nhập thông tin máu cần cấp và người liên hệ để gửi yêu cầu tới NBC." | Yes | none |
| 8 | Next-steps card | Card | static. Title "Sau khi gửi yêu cầu" and three status chips, each with a marker, a word and one line: "Đang chờ xử lý" (NBC xem xét yêu cầu, kiểm tra tồn kho và điều phối.), "Đang vận chuyển" (NBC đã chọn đơn vị máu và ETA. Bệnh viện theo dõi tại Chi tiết lô hàng.), "Đã xác nhận nhận máu" (Bệnh viện xác nhận sau khi đã nhận và kiểm tra lô hàng.) | Yes | none |

### 3.2 Request form (default, empty, loading and error states)

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 9 | Error banner | Banner (`role="alert"`) | static: "Chưa tạo yêu cầu. Vui lòng sửa các trường được đánh dấu và gửi lại." | No | shown only after a failed validation, at the top of the form card |
| 10 | Sending banner | Banner (`role="status"`) | static: "Đang gửi yêu cầu cấp máu. Vui lòng chờ hệ thống xác nhận." with a spinner | No | shown only in the loading state, at the top of the form card |
| 11 | Section title | Header | static: "Thông tin máu cần cấp" | Yes | none |
| 12 | Blood type | Single-choice chips (8, two rows of 4) | label "Nhóm máu *"; `request.blood_type`: A+, A−, B+, B−, AB+, AB−, O+, O−; the selected chip has a tick, a thicker border and a light background | Yes | exactly one of the 8 valid types (BR-01) |
| 13 | Blood type hint | Helper text | static: "Chọn một nhóm máu theo ký hiệu ABO + Rh." | Yes | replaced by the error message when invalid |
| 14 | Quantity | Number input with unit | label "Số lượng *"; `request.quantity`; placeholder "Nhập số lượng"; unit "đơn vị" inside the field | Yes | whole number of 1 or more (BR-01) |
| 15 | Quantity hint | Helper text | static: "Số nguyên từ 1 trở lên." | Yes | replaced by the error message when invalid |
| 16 | Blood product | Select | label "Chế phẩm máu *"; `request.blood_product`, list from the approved product catalogue; placeholder "Chọn chế phẩm máu"; sample value "Hồng cầu lắng" | Yes | one value of the list |
| 17 | Blood product hint | Helper text | static: "Danh sách theo danh mục chế phẩm đã duyệt." | Yes | replaced by the error message when invalid |
| 18 | Urgency | Select | label "Mức độ khẩn *"; `request.urgency`, list from the system; placeholder "Chọn mức độ khẩn"; the mockup shows "[Giá trị mức độ khẩn]" | Yes | one value of the list (BR-01) |
| 19 | Urgency hint | Helper text | static: "Danh sách giá trị lấy từ hệ thống." | Yes | replaced by the error message when invalid |
| 20 | Needed by | Date and time input with calendar icon | label "Cần trước ngày *"; `request.needed_by`; placeholder "dd/MM/yyyy HH:mm" | Yes | valid date and time in the format dd/MM/yyyy HH:mm, Vietnam time (UTC+07:00); not in the past |
| 21 | Needed by hint | Helper text | static: "Định dạng dd/MM/yyyy HH:mm, giờ Việt Nam." | Yes | replaced by the error message when invalid |
| 22 | Reason | Multi-line text | label "Lý do (không bắt buộc)"; `request.reason`; placeholder "Nêu lý do cần máu" | No | maximum length [TBD] |
| 23 | Section title | Header | static: "Thông tin liên hệ" (after a divider) | Yes | none |
| 24 | Contact name | Text input | label "Họ tên người liên hệ *"; `request.contact_name`; placeholder "Nhập họ tên" | Yes | not empty |
| 25 | Contact phone | Text input | label "Số điện thoại *"; `request.contact_phone`; placeholder "Nhập số điện thoại" | Yes | not empty; format [TBD] |
| 26 | Contact phone hint | Helper text | static: "Số NBC có thể gọi khi cần xác nhận yêu cầu." | Yes | replaced by the error message when invalid |
| 27 | Submit request | Primary button | label "Gửi yêu cầu cấp máu"; in the loading state a spinner and "Đang gửi yêu cầu…" | Yes | disabled while the request is being sent |
| 28 | Clear form | Secondary button | label "Xóa biểu mẫu" | No | disabled while the request is being sent |
| 29 | Field error messages | Inline text with error icon | one message under each invalid field (texts in section 4, Error) | No | shown only after a failed validation |
| 30 | Invalid field marker | Border | red border on each invalid field, together with element 29 | No | shown only after a failed validation |

### 3.3 Request summary (success state only)

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 31 | Success banner | Banner (`role="status"`) with tick icon | static: "Đã gửi yêu cầu cấp máu. Yêu cầu đang chờ NBC xử lý." | No | shown only after the server confirms |
| 32 | Request summary | Title "Tóm tắt yêu cầu" and a list of label–value rows | "Mã yêu cầu" `request.id`; "Nhóm máu" `request.blood_type`; "Số lượng" `request.quantity` + " đơn vị"; "Chế phẩm máu" `request.blood_product`; "Mức độ khẩn" `request.urgency`; "Cần trước ngày" `request.needed_by`; "Lý do" `request.reason` ("—" when empty); "Người liên hệ" `request.contact_name` · `request.contact_phone`; "Gửi lúc" `request.created_at` (dd/MM/yyyy HH:mm) | No | read-only |
| 33 | Status chip | Chip, in the row "Trạng thái" | `request.status` = Pending, label "Đang chờ xử lý" (marker, text and colour) | No | read-only |
| 34 | View shipment | Primary button | label "Xem chi tiết lô hàng" | No | none |
| 35 | Send another request | Secondary button | label "Gửi yêu cầu khác" | No | none |

## 4. States

| State | What the user sees | Trigger |
|---|---|---|
| Default (with data) | Form filled with a valid request (example: O−, 6 đơn vị, Hồng cầu lắng, 19/11/2026 12:00, contact Nguyễn Văn A, 0912 345 678); hints under the fields; Submit request and Clear form enabled | The user has entered all required values |
| Empty (no data) | No blood type selected and no text; placeholders in every field; hints under the fields; Submit request enabled (pressing it shows the error state) | Screen opened for a new request, or after "Gửi yêu cầu khác" or "Xóa biểu mẫu" |
| Loading | Sending banner (element 10) at the top of the form card; every field and chip disabled (grey background) and keeps its value; Submit request shows a spinner and "Đang gửi yêu cầu…"; Clear form disabled | The user pressed Submit request and the server has not answered |
| Error | Error banner (element 9) at the top of the form card. Under each invalid field, a red border, an error icon and the message that replaces the hint: "Chọn nhóm máu.", "Nhập số đơn vị là số nguyên từ 1 trở lên.", "Chọn chế phẩm máu.", "Chọn mức độ khẩn.", "Nhập thời hạn theo dd/MM/yyyy HH:mm.", "Nhập họ tên người liên hệ.", "Nhập số điện thoại liên hệ.". Valid fields keep their hint and normal border. No request is created and every value already entered is kept, including invalid ones such as "0" (FR-01-06). **Uncertain result** (no image): banner "Chưa xác định được kết quả thao tác. Hãy tải lại trạng thái hiện tại trước khi thử lại."; the form keeps its values and nothing is shown as sent | Validation fails; or the connection is lost before the server answers |
| Success / confirmation | The form is replaced by the success banner, the request summary with the status chip "Đang chờ xử lý", and the buttons "Xem chi tiết lô hàng" and "Gửi yêu cầu khác". The next-steps card stays on the right | The server created the request with status Pending (FR-01-01) |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Blood type chip | click | Selects that type and shows the tick; any other selected type is cleared; clears the blood type error | stays |
| 2 | Quantity, Blood product, Urgency, Needed by, Reason, Contact name, Contact phone | enter or choose | Stores the value in the form; clears the error message and red border of that field | stays |
| 3 | Submit request | click, or Enter in the form | Checks the values (FR-01-06). If valid: shows the loading state, creates the request as Pending and puts it in the NBC queue (FR-01-01), then shows the success state. If not valid: shows the error state | stays (loading, then success or error state) |
| 4 | Clear form | click | Empties every field, the blood type and every message; shows the empty state | stays |
| 5 | View shipment | click (success state) | Opens the new request on the shipment screen. While the request is Pending, S-F01-02 shows its empty state "Chưa có thông tin lô hàng được điều phối cho yêu cầu này." | S-F01-02 |
| 6 | Gửi yêu cầu khác | click (success state) | Shows the empty form | stays |
| 7 | Menu item "Yêu cầu cấp máu" | click | Current screen; opens the empty form if the success state is shown | stays |
| 8 | Menu item "Chi tiết lô hàng" | click | Opens the shipment screen | S-F01-02 |
| 9 | Menu item "Ghi nhận sử dụng máu" | click | Opens the blood usage record | S-F02-01 |
| 10 | Menu item "Nhu cầu máu theo kế hoạch" | click | Opens the demand log | S-F03-01 |
| 11 | Menu item "Phản hồi xác minh" | click | Opens the verification response | S-F03-03 |
| 12 | Menu item "Tổng quan tồn kho" | click | Opens the inventory dashboard | S-F02-02 |
| 13 | Sign out | click | Ends the session | S-F04-01 |

Leaving the screen with unsaved values discards them; no confirmation dialog is shown (no modal for this screen in `screen-list.md`).

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | A request is created only if blood type, quantity, blood product, urgency, needed-by date and contact are all valid; otherwise nothing is created. | FR-01-06, BR-01 |
| SR-002 | After a failed validation every value already entered stays in the form. | FR-01-06 |
| SR-003 | The screen shows "success" only after the server confirms the new request; while it waits, the fields and buttons are disabled so the request cannot be sent twice. If the result is unknown, the screen does not say that the request was sent or failed. | UI conventions v2.0 §3, §4.1 |
| SR-004 | The new request has status Pending ("Đang chờ xử lý"); the screen does not show it as Dispatched or Received. | FR-01-01 |
| SR-005 | Hospital Staff see and create requests only for their own hospital; the hospital comes from the session and is not a field of the form. | BR-06, FR-04-03 |
| SR-006 | State is never shown by colour alone: a selected blood type has a tick and a border, an error has an icon and text, a status has a marker and a word. | UI conventions v2.0 §5 |

## 7. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| FR-01-01 | Provides the form and the Submit action, and shows the new Pending request in the success state |
| FR-01-06 | Shows a reason for each invalid field and keeps the entered values so the user can correct and resubmit |
| FR-04-03 | Shows only the menu items and the hospital data allowed for the signed-in Hospital Staff |

## 8. Responsive and accessibility notes

- The mockups are drawn at 1440 px wide only, as the UI conventions v2.0 require. A narrow window and mobile are out of scope for this release (UI conventions §2.1); if they are added later, the two-column fields stack.
- Every field has a visible label that stays when a value is entered; placeholders never replace labels. Required fields are marked with *, and the Reason label says "(không bắt buộc)".
- Error messages sit directly under their field and are linked to it; the error banner announces the failure (`role="alert"`); the sending and success banners use `role="status"`.
- Keyboard: Tab follows the reading order (blood type chips, then the fields row by row, then Submit and Clear); a focus outline is visible; arrow keys move between blood type chips; Enter on Submit request sends the form.
- Body text contrast at least 4.5:1; borders of inputs and chips at least 3:1. Disabled fields in the loading state keep their values readable.
- Body text 14–16 px; helper and error text 13 px.

## 9. Mockup corrections

Differences between the five images and this spec. Fix them in Figma before exporting the final images.

| # | Image | Problem | Fix |
|---|---|---|---|
| 1 | Error | The selected chip O− has a border and background but no tick (SR-006) | Add the tick, as in the default and loading images |
| 2 | Empty, Error | The Reason label is "Lý do" without "(không bắt buộc)" | Use "Lý do (không bắt buộc)" as in the default and loading images |
| 3 | All | The menu item "Nhu cầu máu theo kế hoạch" is cut to "kế hoạc" | Let the label wrap on two lines, or reduce the item padding |
| 4 | Loading | "Xóa biểu mẫu" looks almost the same as when it is enabled | Use the same disabled style as the Submit button |

## 10. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: Blood product, Needed by, Reason, Contact name and Contact phone are not in FR-01-01 or FR-01-06 (only blood type, quantity, urgency), nor in the SD-UC-01 payload. Update W1 and SD-UC-01, or remove the fields.] | Yes | Open |
| 2 | [NEEDS CLARIFICATION: which urgency values exist?] | Yes | Open. The mockup shows "[Giá trị mức độ khẩn]". |
| 3 | [NEEDS CLARIFICATION: which blood products are in the catalogue?] | Yes | Open. The mockup shows "Hồng cầu lắng" as a sample. |
| 4 | [NEEDS CLARIFICATION: may Needed by be in the past, and must it fit the urgency?] | No | Open. Default: not in the past. |
| 5 | [NEEDS CLARIFICATION: phone number format and maximum length of Reason?] | No | Open. |
| 6 | [NEEDS CLARIFICATION: is Reason required for the most urgent level?] | No | Open. Default: optional. |
| 7 | Four colours in the mockup are proposed and not yet in the UI conventions v2.0 §6: border-strong (input and chip borders), warning-bg (chip "Đang chờ xử lý"), error-bg (error banner) and the grey background of the sending banner and disabled fields. | No | Open. Add them to the tokens or replace them with existing tokens. |
| 8 | Which element numbers should the numbered images show? This spec numbers 35 elements (v1.0 had 21). | No | Closed: five numbered images made from section 3 (section 2). |

---

## Completion checklist

- [x] The mockup images are separate files, named with the Screen ID, at the frame width. *(five state images; corrections in section 9 still to apply)*
- [x] The numbered images exist, and their numbers match the # column of the element inventory. *(five images; redo them after the corrections in section 9)*
- [x] Every visible element in the mockup appears in the element inventory.
- [ ] Every colour in the mockup is a design token from the UI conventions. *(four proposed colours, see question 7)*
- [x] Every input element has a validation rule or an explicit "none".
- [x] All five states are filled in.
- [x] Every navigation target is a Screen ID that exists in the Screen List.
- [x] Every element that displays data names the field it displays.
- [ ] Every linked FR exists in the Function List, and every action of this screen in AD-UC-01 is covered by an interaction row. *(AD-UC-01 not checked)*

## Changes in v1.1 (21/11/2026)

- Image names made consistent (lowercase, one name per state) and matched to the five exported images; frame heights recorded.
- Element inventory rebuilt from the images: added brand, breadcrumb, subtitle, section titles, helper texts, field error messages and markers, summary rows with their labels; split into page frame, form and summary; renumbered 1 to 35.
- Success state corrected: the summary replaces the whole form, not only the Clear form button.
- States: added the example data, the disabled look of the loading state and the behaviour of valid fields in the error state.
- Interactions: added the current menu item, Enter to submit, and what S-F01-02 shows while the request is Pending.
- Section 8: removed the 360 px minimum width, which the UI conventions v2.0 put out of scope.
- New section 9 lists four corrections needed in the mockups; open question 7 extended, open question 8 added.
