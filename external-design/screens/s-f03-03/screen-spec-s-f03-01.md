# Screen Spec: S-F03-01 Blood demand log

| Field | Value |
|---|---|
| Screen ID | S-F03-01 |
| Screen name | Blood demand log (Vietnamese UI title: "Nhu cầu máu theo kế hoạch") |
| Actor | Hospital Staff |
| Priority | Could (`screen-list.md`; W4 effective priority of FR-03-01: Could, release R3, D-25) |
| Feature | F-03 Clinical Demand & Forecasting |
| Use cases / FRs | UC-05 · FR-03-01 (as in `screen-list.md`) |
| Activity diagrams | AD-UC-05 (v0.1) |
| Sequence diagrams | SD-UC-05 |
| Usage flow | Hospital Staff Flow 2 "Record blood usage and planned blood demand" (`usage-flow.md` v2.3): S-F04-01 → **S-F03-01** ⇄ S-F02-01 |
| Mockup images | `img/s-f03-01-default.png`, `img/s-f03-01-empty.png`, `img/s-f03-01-loading.png`, `img/s-f03-01-error.png`, `img/s-f03-01-success.png`; numbered: the same five names ending in `-numbered.png` (section 2) |
| Frame size | 1440 px wide (desktop, UI conventions v2.0 §2.1). Height grows with the content: 1117 px (default), 1024 px (empty, success), 1189 px (loading), 1311 px (error); the page scrolls vertically |
| Figma frame | Not yet in Figma. Draft frames on the design canvas https://claude.ai/artifact/BF77VmyRjL3vdqqTiHCMYU (frames "S-F03-01 · Nhu cầu máu theo kế hoạch · 1 to 5") |
| Version | v1.0, 21/11/2026 |
| Status | Draft |

## 1. Purpose

**Shown when:** Hospital Staff know in advance that the hospital will need blood, for a scheduled surgery or for patients on periodic transfusion, and log that plan so NBC can use it as input for the blood-demand forecast (UC-05). They choose the plan type, enter the date, blood type, blood product and units (for a periodic plan also the cycle and the number of patients), then save. NBSP validates the plan and stores it as forecasting input; if a required field is missing or invalid, nothing is saved and the reason is shown (FR-03-01).

**The user leaves this screen when:** the plan is saved and the user presses "Ghi nhận sử dụng máu" (to S-F02-01), opens another menu item, or signs out. "Nhập kế hoạch khác" stays on this screen with an empty form.

**Entry points** (`screen-list.md`): menu item "Nhu cầu máu theo kế hoạch"; success state of S-F02-01 ("Nhập nhu cầu máu theo kế hoạch").

## 2. Mockup

![S-F03-01 default](img/s-f03-01-default.png)

**Layout (all states):** sidebar on the left (brand, role group, menu for Hospital Staff); top bar (hospital name, role, Sign out); breadcrumb, title and subtitle; main card on the left of the content area (plan type, then the plan details for the chosen type, or the plan summary in the success state); on the right, the card "Kế hoạch sắp tới" and the card "Sau khi lưu kế hoạch".

**Numbered images:** each number is the # in the element inventory (section 3). One number covers a field's label, control and helper text. Element 8 (error banner) appears only in the error state and element 9 (saving banner) only in the loading state. Elements 13 to 17 follow the plan type: the scheduled-surgery fields are on the default and loading images, the periodic-transfusion fields (with 18 and 19) on the error image. Elements 22 and 23 appear only in the error state; they are numbered once, on the start date field. Element 24 appears only in the empty state, in place of the fields. Elements 27 to 31 appear only in the success state, where they replace elements 10 to 24 in the main card.

| State | Numbered image | Numbers shown |
|---|---|---|
| Default | ![](img/s-f03-01-default-numbered.png) | 1–7, 10–17, 20–21, 25–26 |
| Empty | ![](img/s-f03-01-empty-numbered.png) | 1–7, 10–12, 20–21, 24–26 |
| Loading | ![](img/s-f03-01-loading-numbered.png) | 1–7, 9–17, 20–21, 25–26 |
| Error | ![](img/s-f03-01-error-numbered.png) | 1–8, 10–23, 25–26 |
| Success | ![](img/s-f03-01-success-numbered.png) | 1–7, 25–31 |

| State | Image | What it shows |
|---|---|---|
| Default (with data) | `img/s-f03-01-default.png` | Scheduled surgery: 26/11/2026, 4 đơn vị, A+, Hồng cầu lắng, a note; Save enabled |
| Empty | `img/s-f03-01-empty.png` | No plan type chosen; the details area asks the user to choose a type |
| Loading | `img/s-f03-01-loading.png` | Saving banner, fields disabled, Save with spinner |
| Error | `img/s-f03-01-error.png` | Periodic transfusion with a start date in the past, no cycle and 0 units (FR-03-01, IF clause): banner plus a message under each invalid field; values kept. The uncertain-result error is described in section 4 only |
| Success | `img/s-f03-01-success.png` | Success banner, plan summary, status "Đã lưu"; the new plan first in "Kế hoạch sắp tới" |

All colours, fonts and corners come from the design tokens in the UI conventions v2.0, section 6 ([ui-conventions.md](../ui-conventions.md#6-design-tokens)), except the colours listed in open question 9.

## 3. Element inventory

Vietnamese labels are written as they appear in the mockup.

### 3.1 Page frame (all states)

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | Brand | Logo and text | static: "NBSP", "Chuỗi cung ứng máu" | Yes | none |
| 2 | Menu | Navigation | static; role group "NHÂN VIÊN BỆNH VIỆN" and the items allowed for Hospital Staff (FR-04-03): "Yêu cầu cấp máu", "Chi tiết lô hàng", "Ghi nhận sử dụng máu", "Nhu cầu máu theo kế hoạch" (current, marked by background, left bar and bold text), "Phản hồi xác minh" (count badge of Open verification requests, D-20), "Tổng quan tồn kho" | Yes | badge hidden when the count is 0 |
| 3 | Account info | Text | `hospital.name` ("[Tên bệnh viện]") and the role "Nhân viên bệnh viện", from the session | Yes | none |
| 4 | Sign out | Secondary button | label "Đăng xuất" | Yes | none |
| 5 | Breadcrumb | Text | static: "Nhu cầu máu theo kế hoạch" | Yes | none |
| 6 | Title | Header | static: "Nhu cầu máu theo kế hoạch" | Yes | none |
| 7 | Subtitle | Text | static: "Nhập lịch phẫu thuật hoặc kế hoạch truyền máu định kỳ để NBC dùng làm đầu vào dự báo." | Yes | none |

### 3.2 Plan form (default, empty, loading and error states)

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 8 | Error banner | Banner (`role="alert"`) | static: "Chưa lưu kế hoạch. Vui lòng sửa các trường được đánh dấu và lưu lại." (proposed copy, open question 10) | No | shown only after a rejected save, at the top of the form card |
| 9 | Saving banner | Banner (`role="status"`) | static: "Đang lưu kế hoạch nhu cầu máu. Vui lòng chờ hệ thống xác nhận." with a spinner | No | shown only in the loading state, at the top of the form card |
| 10 | Section title | Header | static: "Loại kế hoạch" | Yes | none |
| 11 | Plan type | Single-choice cards (2) | label "Loại kế hoạch *"; `plan.type`: Scheduled surgery "Phẫu thuật theo lịch" (Một ngày phẫu thuật cần máu), Periodic transfusion "Truyền máu định kỳ" (Nhóm bệnh nhân truyền máu theo chu kỳ); the selected card has a filled radio, a thicker border and a light background. Empty state hint: "Chọn loại kế hoạch; các trường chi tiết hiện theo loại đã chọn." | Yes | exactly one value |
| 12 | Details title | Header | depends on `plan.type`: "Chi tiết ca phẫu thuật" (surgery), "Chi tiết truyền máu định kỳ" (periodic), "Chi tiết kế hoạch" (none chosen); after a divider | Yes | none |
| 13 | Date | Date input with calendar icon | surgery: label "Ngày phẫu thuật *", periodic: label "Ngày bắt đầu *"; `plan.date`; placeholder "dd/MM/yyyy"; hint "Định dạng dd/MM/yyyy, từ hôm nay trở đi." | Yes | valid date dd/MM/yyyy; today or later (open question 6) |
| 14 | Blood type | Single-choice chips (8, two rows of 4) | label "Nhóm máu *"; `plan.blood_type`: A+, A−, B+, B−, AB+, AB−, O+, O−; the selected chip has a tick, a thicker border and a light background; hint "Chọn một nhóm máu theo ký hiệu ABO + Rh." | Yes | exactly one of the 8 valid types (BR-01) |
| 15 | Blood product | Select | label "Chế phẩm máu *"; `plan.blood_product`, list of the enabled products (release 1: red blood cells only, BR-01); placeholder "Chọn chế phẩm máu"; sample "Hồng cầu lắng"; hint "Danh sách theo danh mục chế phẩm đã duyệt." | Yes | one value of the list |
| 16 | Units | Number input with unit | surgery: label "Số đơn vị dự kiến *", periodic: label "Số đơn vị mỗi kỳ *" (open question 2); `plan.units`; placeholder "Nhập số đơn vị"; unit "đơn vị" inside the field; hint "Số nguyên từ 1 trở lên." | Yes | whole number of 1 or more; maximum [TBD] |
| 17 | Note | Multi-line text | label "Ghi chú (không bắt buộc)"; `plan.note`; placeholder "Thông tin thêm cho NBC" | No | maximum length [TBD]; field not in SD-UC-05 (open question 4) |
| 18 | Cycle | Select (periodic only) | label "Chu kỳ *"; `plan.cycle`, list of allowed cycles [TBD, open question 3]; placeholder "Chọn chu kỳ" | Yes for periodic | one value of the list |
| 19 | Number of patients | Number input with unit (periodic only) | label "Số bệnh nhân *"; `plan.patient_count`; unit "bệnh nhân"; hint "Số nguyên từ 1 trở lên." | Yes for periodic | whole number of 1 or more |
| 20 | Save | Primary button | label "Lưu kế hoạch nhu cầu"; in the loading state a spinner and "Đang lưu…" | Yes | disabled while the plan is being saved |
| 21 | Clear form | Secondary button | label "Xóa biểu mẫu" | No | disabled while the plan is being saved |
| 22 | Field error messages | Inline text with error icon | one message under each invalid field (texts in section 4, Error) | No | shown only after a rejected save |
| 23 | Invalid field marker | Border | red border on each invalid field, together with element 22 | No | shown only after a rejected save |
| 24 | Details placeholder | Empty-state panel (dashed border) | static: "Chưa chọn loại kế hoạch" and "Chọn "Phẫu thuật theo lịch" hoặc "Truyền máu định kỳ" để nhập ngày, nhóm máu, chế phẩm và số đơn vị." | No | shown only while no plan type is chosen, in place of elements 13 to 19 |

### 3.3 Right column (all states)

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 25 | Upcoming plans card | Card, read-only list | title "Kế hoạch sắp tới"; the hospital's saved plans whose date is today or later [how many: open question 7], soonest first: "Phẫu thuật · `blood_type` · `blood_product`" or "Định kỳ · `blood_type` · `patient_count` bệnh nhân", the date ("Từ `date` · `cycle`" for periodic) and the units ("`units` đơn vị" or "`units` đơn vị/kỳ"). In the success state the new plan is first, on a light green background, with "· Mới". Empty state: "Chưa có kế hoạch nhu cầu nào được lưu." | Yes | read-only; only the signed-in hospital's plans (BR-06) |
| 26 | After-save card | Card, static | title "Sau khi lưu kế hoạch" and three status chips, each with a marker, a word and one line: "Đã lưu" (Kế hoạch vào dữ liệu đầu vào cho kỳ dự báo hiện tại.), "Đang chờ xác minh" (NBC có thể yêu cầu xác minh nếu thấy số liệu bất thường; trả lời tại Phản hồi xác minh.), "Đã chấp nhận" (Chuyên viên dự báo NBC chấp nhận dữ liệu cho bản dự báo.) | Yes | none |

### 3.4 Plan summary (success state only)

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 27 | Success banner | Banner (`role="status"`) with tick icon | static: "Đã lưu kế hoạch nhu cầu máu. Kế hoạch được dùng làm đầu vào cho dự báo của NBC." (proposed copy, open question 10) | No | shown only after the server confirms |
| 28 | Plan summary | Title "Tóm tắt kế hoạch" and a list of label–value rows | "Mã kế hoạch" `plan.id`; "Loại kế hoạch" `plan.type`; "Ngày phẫu thuật" or "Ngày bắt đầu" `plan.date`; for periodic also "Chu kỳ" `plan.cycle` and "Số bệnh nhân" `plan.patient_count`; "Nhóm máu" `plan.blood_type`; "Chế phẩm máu" `plan.blood_product`; "Số đơn vị dự kiến" / "Số đơn vị mỗi kỳ" `plan.units` + " đơn vị"; "Ghi chú" `plan.note` ("—" when empty); "Lưu lúc" `plan.created_at` (dd/MM/yyyy HH:mm) | No | read-only |
| 29 | Status chip | Chip, in the row "Trạng thái" | `plan.status` = Saved, label "Đã lưu" (tick, text and colour) | No | read-only |
| 30 | Log another plan | Primary button | label "Nhập kế hoạch khác" (with the question "Còn dữ liệu cần nhập?") | No | none |
| 31 | Record usage | Secondary button with arrow icon | label "Ghi nhận sử dụng máu" | No | none |

## 4. States

| State | What the user sees | Trigger |
|---|---|---|
| Default (with data) | Plan type "Phẫu thuật theo lịch" chosen; details filled (example: 26/11/2026, 4 đơn vị, A+, Hồng cầu lắng, "Phẫu thuật thay khớp háng, 2 ca liên tiếp buổi sáng."); hints under the fields; Save and Clear form enabled; 3 upcoming plans on the right | The user has chosen a type and entered all required values |
| Empty (no data) | No plan type chosen; the details area shows the placeholder (element 24) instead of the fields; Save enabled (pressing it shows the error "Chọn loại kế hoạch."); upcoming card "Chưa có kế hoạch nhu cầu nào được lưu." | Screen opened from the menu, or after "Nhập kế hoạch khác" or "Xóa biểu mẫu" |
| Loading | Saving banner (element 9) at the top of the form card; plan type cards, fields and chips disabled (grey background) and keep their values; Save shows a spinner and "Đang lưu…"; Clear form disabled | The user pressed Save and the server has not answered |
| Error | Error banner (element 8) at the top of the form card. Under each invalid field, a red border, an error icon and the message that replaces the hint: "Chọn loại kế hoạch.", "Nhập ngày theo dd/MM/yyyy.", "Ngày phẫu thuật không được trước hôm nay (`today`)." / "Ngày bắt đầu không được trước hôm nay (`today`).", "Chọn nhóm máu.", "Chọn chế phẩm máu.", "Nhập số đơn vị là số nguyên từ 1 trở lên.", "Chọn chu kỳ truyền máu.", "Nhập số bệnh nhân là số nguyên từ 1 trở lên.". Valid fields keep their hint and normal border. No plan is saved and every value already entered is kept, including invalid ones such as "0" (FR-03-01, IF clause). The example image is a periodic plan with start date 15/11/2026, no cycle and 0 units. **Uncertain result** (no image): banner "Chưa xác định được kết quả thao tác. Hãy tải lại trạng thái hiện tại trước khi thử lại."; the form keeps its values and nothing is shown as saved | Validation fails; or the connection is lost before the server answers |
| Success / confirmation | The form is replaced by the success banner, the plan summary with the status chip "Đã lưu", and the buttons "Nhập kế hoạch khác" and "Ghi nhận sử dụng máu". The new plan appears first in "Kế hoạch sắp tới", marked "Mới". The after-save card stays on the right | The server stored the plan as forecasting input (FR-03-01) |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Plan type card | click | Selects that type; shows the details fields of the type (surgery: 13–17; periodic: 13–19); values already typed in the common fields (date, blood type, product, units, note) are kept; clears the plan type error | stays |
| 2 | Blood type chip | click | Selects that type and shows the tick; any other selected type is cleared; clears the blood type error | stays |
| 3 | Date, Product, Units, Note, Cycle, Number of patients | enter or choose | Stores the value in the form; clears the error message and red border of that field | stays |
| 4 | Save | click, or Enter in the form | Shows the loading state and sends the plan. If valid: stores it as forecasting input (FR-03-01) and shows the success state. If not: saves nothing and shows the error state | stays (loading, then success or error state) |
| 5 | Clear form | click | Empties the plan type, every field and every message; shows the empty state | stays |
| 6 | Nhập kế hoạch khác | click (success state) | Shows the empty form | stays |
| 7 | Ghi nhận sử dụng máu | click (success state) | Opens the blood usage record ("More to record?" = yes, Flow 2) | S-F02-01 |
| 8 | Menu item "Yêu cầu cấp máu" | click | Opens the blood request form | S-F01-01 |
| 9 | Menu item "Chi tiết lô hàng" | click | Opens the shipment screen | S-F01-02 |
| 10 | Menu item "Ghi nhận sử dụng máu" | click | Opens the blood usage record | S-F02-01 |
| 11 | Menu item "Nhu cầu máu theo kế hoạch" | click | Current screen; opens the empty form if the success state is shown | stays |
| 12 | Menu item "Phản hồi xác minh" | click | Opens the verification response | S-F03-03 |
| 13 | Menu item "Tổng quan tồn kho" | click | Opens the inventory dashboard | S-F02-02 |
| 14 | Sign out | click | Ends the session | S-F04-01 |

Leaving the screen with unsaved values discards them; no confirmation dialog is shown (no modal for this screen in `screen-list.md`). The upcoming plans in element 25 are read-only: no FR covers editing or cancelling a saved plan (open question 5).

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | A plan is saved only if the plan type and every required field of that type are valid (surgery: date, blood type, product, units; periodic: also cycle and number of patients); otherwise nothing is saved. | FR-03-01, SD-UC-05, AD-UC-05 |
| SR-002 | After a rejected save every value already entered stays in the form. | FR-03-01 (IF clause), W1 L3 FR-03-01-FE |
| SR-003 | The screen shows "success" only after the server confirms the plan; while it waits, the fields and buttons are disabled so the plan cannot be saved twice. If the result is unknown, the screen does not say that the plan was saved or rejected. | UI conventions v2.0 §3, §4.1; AD-UC-05 reading note (confirmation after the save) |
| SR-004 | A saved plan is only forecasting input; the screen does not say that NBC has accepted it or that a forecast was made. NBC may flag it later (UC-13), and the hospital answers on S-F03-03. | FR-03-01, FR-03-02, FR-03-03 |
| SR-005 | Hospital Staff log and see plans only for their own hospital; the hospital comes from the session and is not a field of the form. | BR-06, FR-04-03 |
| SR-006 | State is never shown by colour alone: a selected type has a filled radio and a border, a selected blood type has a tick, an error has an icon and text, a status chip has a marker and a word. | UI conventions v2.0 §5 |

## 7. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| FR-03-01 | Provides the plan form for scheduled surgery and periodic transfusion and the Save action; shows the saved plan; shows the reason for each invalid field, saves nothing and keeps the values so the user can correct and resubmit |
| FR-04-03 | Shows only the menu items and the hospital data allowed for the signed-in Hospital Staff |

## 8. Responsive and accessibility notes

- The mockups are drawn at 1440 px wide only, as the UI conventions v2.0 require. A narrow window and mobile are out of scope for this release (UI conventions §2.1); if they are added later, the two-column fields stack and the right column moves under the form.
- Every field has a visible label that stays when a value is entered; placeholders never replace labels. Required fields are marked with *, and the Note label says "(không bắt buộc)".
- When the plan type changes, the new fields are announced (`aria-live="polite"` on the details area) and focus stays on the type card.
- Error messages sit directly under their field and are linked to it; the error banner announces the failure (`role="alert"`); the saving and success banners use `role="status"`.
- Keyboard: Tab follows the reading order (plan type, then the details row by row, then Save and Clear); arrow keys move between the two type cards and between blood type chips; a focus outline is visible; Enter on Save saves the plan.
- Body text contrast at least 4.5:1; borders of inputs, chips and cards at least 3:1. Disabled fields in the loading state keep their values readable.
- Body text 14–16 px; helper and error text 13 px.

## 9. Mockup corrections

Differences between the five images and this spec. Fix them in Figma when the frames are rebuilt there.

| # | Image | Problem | Fix |
|---|---|---|---|
| 1 | All | The frames exist only on the design canvas, not in the team's Figma file | Rebuild them in Figma as frames "S-F03-01 · Nhu cầu máu theo kế hoạch · 1 to 5", then export again |
| 2 | Default, Loading | Only the scheduled-surgery fields are drawn; the periodic fields appear only on the error image | Add a default image of a valid periodic plan if the team wants one |
| 3 | Default, Error | The periodic plan in "Kế hoạch sắp tới" shows "[Chu kỳ]" because the cycle values are not decided | Replace with a real value once open question 3 is answered |

## 10. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | Which FR covers the invalid-plan branch? `usage-flow.md` cites FR-03-08 (proposed), `screen-list.md` says "FR number to confirm", W1 puts it in the IF clause of FR-03-01. | No | Closed in this spec: FR-03-01 (W1). Update `usage-flow.md` and `screen-list.md`. |
| 2 | [NEEDS CLARIFICATION: for a periodic plan, are the units per cycle for the whole group, or per patient per cycle? SD-UC-05 says only "units".] | Yes | Open. Default: total units per cycle ("Số đơn vị mỗi kỳ"). |
| 3 | [NEEDS CLARIFICATION: which cycle values are allowed (weekly, every 2 weeks, monthly, a number of days)? AD-UC-05 test uses "14 days".] | Yes | Open. The mockup shows a select with placeholder "Chọn chu kỳ". |
| 4 | [NEEDS CLARIFICATION: the Note field is not in FR-03-01 or SD-UC-05. Keep it (useful when NBC verifies the data) or remove it?] | No | Open. Default: keep, optional. |
| 5 | [NEEDS CLARIFICATION: can a saved plan be edited or cancelled? AD-UC-05 lists only the state Saved and asks for the full state list.] | No | Open. Default: no; plans in element 25 are read-only. |
| 6 | [NEEDS CLARIFICATION: may the date be in the past (a plan logged late)? Is there a maximum number of units? (AD-UC-05 Q5)] | No | Open. Default: today or later; no maximum. |
| 7 | [NEEDS CLARIFICATION: is the list "Kế hoạch sắp tới" part of the use case? AD-UC-05 left out SD step 8 "Show updated plan list (optional)" (Q3). How many plans does it show?] | No | Open. Default: shown, 3 soonest plans, read-only. |
| 8 | [NEEDS CLARIFICATION: does a periodic plan need an end date or a number of cycles? Neither is in SD-UC-05, so the mockup has none and the plan is open-ended.] | No | Open. |
| 9 | Colours in the mockup that are proposed and not yet in the UI conventions v2.0 §6: border-strong `#AFC0B5`, warning-bg `#FDEDE4` (chip "Đang chờ xác minh"), error-bg `#FBEAED`, disabled/loading grey `#EEF1EF`, text-secondary `#5A6D63`. | No | Open. Same list as S-F02-01. |
| 10 | The UI conventions §4.2 have no copy for S-F03-01. Add the error, saving and success texts used here (elements 8, 9, 27). | No | Open. |

---

## Completion checklist

- [x] The mockup images are separate files, named with the Screen ID, at the frame width.
- [x] The numbered images exist, and their numbers match the # column of the element inventory.
- [x] Every visible element in the mockup appears in the element inventory.
- [ ] Every colour in the mockup is a design token from the UI conventions. *(five proposed colours, see question 9)*
- [x] Every input element has a validation rule or an explicit "none".
- [x] All five states are filled in.
- [x] Every navigation target is a Screen ID that exists in the Screen List.
- [x] Every element that displays data names the field it displays.
- [x] Every linked FR exists in the Function List, and every action of this screen in AD-UC-05 is covered by an interaction row. *(select plan type → 1; enter details → 2–3; validate, save, confirm → 4 and Success; show error and re-enter → Error state)*
