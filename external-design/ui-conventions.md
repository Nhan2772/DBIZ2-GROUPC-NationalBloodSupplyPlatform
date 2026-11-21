# UI Conventions — NBSP

## 0. Scope, references and general principles

These conventions apply to the **13 screens** in `screen-list.md`, to the flows of the **4 actors** in `usage-flow.md`, and to the UI states that appear in the related use cases. Do not create a new screen just because a message, a status, a confirmation dialog or a background task appears.

| Actor | What the actor does (Usage Flow) | Main screens |
|---|---|---|
| **Hospital Staff** (*Nhân viên bệnh viện*) | Sign in; request blood; view the list of requests and shipments, confirm receipt; record blood usage and the opening balance; log planned demand; respond to data verification; view inventory within their scope | `S-F04-01`, `S-F01-01`, `S-F01-02`, `S-F02-01`, `S-F02-02`, `S-F03-01`, `S-F03-03` |
| **NBC Blood Storage and Distribution Department** (*Bộ phận Lưu trữ & Phân phối máu NBC*, "the Dept") | View the request queue; check stock; approve and dispatch, or not approve with a reason; monitor near-expiry units; receive below-threshold alerts | `S-F01-03`, `S-F01-04`, `S-F02-02`, `S-F02-03` |
| **NBC Forecasting Analyst** (*Chuyên viên phân tích dự báo NBC*) | Review demand data and anomalies; record validation decisions; review draft forecast items and publish a version | `S-F03-02`, `S-F03-04` |
| **NBC Operations** (*Bộ phận Vận hành NBC*) | View the published forecast; check the stock position (read-only, same data scope as the Dept); receive below-threshold alerts | `S-F03-05`, `S-F02-02` |

**Cross-cutting rules:**

1. **One Screen ID is one screen.** It may contain several tabs, tables, modals or states. Do not turn states such as "Gửi thành công", "Thiếu máu" or "Đang vận chuyển" into separate screens if the Screen List does not have them.
2. **Access rights decide which data and actions are visible.** Never show data of another unit outside the authorised scope; the menu and the content must match the signed-in actor (FR-04-03).
3. **Separate business status, displayed label and action feedback.** Example: "Không thể điều phối do thiếu tồn kho" is error feedback, **not** a new final status of the request.
4. **Do not invent business requirements.** Identifiers, the near-expiry window, configurable values and field data not defined in W0, W1 or a Screen Spec must be read from configuration or logged as an open question.
5. **Keep reference codes in English** (`UC-xx`, `FR-xx-xx`, `S-Fxx-xx`, `Pending`, `Dispatched`, `Received`, `Not approved`) for traceability and code; only the **displayed labels** are translated for the user.

---

## 1. Language and wording

### 1.1. General rules

- **100% of user-facing labels, descriptions, messages, buttons, titles and placeholders are in Vietnamese.** Explain a specialist abbreviation at first use if the target users may not know it; system names and codes stay as they are.
- Tone: **clear, short, neutral and accountable**, suited to blood management and distribution. Avoid wording that suggests the system has completed an action before the server confirms it.
- **Action button = verb + object/result**, for example "Gửi yêu cầu cấp máu", "Xem chi tiết lô hàng", "Xác nhận đã nhận máu", "Lưu dữ liệu sử dụng", "Yêu cầu bệnh viện xác minh", "Công bố phiên bản". Never use "OK", "Submit", "Process" or "Done" alone.
- Addressing the user: avoid "bạn"; name the object and the action instead. Example: "Yêu cầu chưa được tạo. Vui lòng kiểm tra các trường được đánh dấu."
- Use the same terms everywhere: **"yêu cầu cấp máu"**, **"lô hàng"**, **"đơn vị máu"**, **"tồn kho"**, **"dữ liệu nhu cầu"**, **"bản dự báo"**, **"xác minh dữ liệu"**; never replace "phê duyệt" (approve) with "đã giao" (delivered).

### 1.2. Status glossary

| Code / status in the system | Displayed label (Vietnamese) | Meaning and constraints |
|---|---|---|
| `Pending` | **Đang chờ xử lý** | The request is waiting for NBC to review and dispatch; it does not mean a shipment has left. |
| `Dispatched` | **Đã xuất kho / đang vận chuyển** | NBC has recorded the dispatch. The screen may show **"Đang vận chuyển"** as an *explanatory label*; this is **not a new database status**. Never show "Đã giao tới bệnh viện" without proof of delivery. |
| `Received` | **Đã xác nhận nhận máu** | Only after the hospital confirms physical receipt and the system has saved it; never set automatically after dispatch. |
| `Not approved` | **Không được phê duyệt** | The Dept closed a `Pending` request with a reason (FR-01-09, BR-02). Final status. The hospital sees the reason in the request list on `S-F01-02`; no separate message is sent. |
| `Pending Verification` | **Đang chờ xác minh** | An anomalous record was sent to the hospital for verification (UC-13, UC-14). |
| `Accepted` / `Excluded` | **Đã chấp nhận** / **Đã loại khỏi dự báo** | Decision on a *demand data record*; not a shipment status. |
| Forecast item `Draft` / `Approved` / `Rejected` | **Dự báo nháp** / **Đã duyệt** / **Đã từ chối** | Status of *each forecast item* (AD-UC-15 v1.0, FR-03-06, BR-07). `Rejected` and `Draft` items are not published. |
| Forecast version `Published` | **Phiên bản đã công bố** | Contains only `Approved` items; NBC Operations reads this version (UC-16). Earlier versions are kept. |
| Urgency `Emergency` / `Urgent` / `Periodic` | **Cấp cứu** / **Khẩn** / **Định kỳ** (proposed labels) | The three values of W0 BR-01, ordered in the queue in this order (BR-05). Labels to confirm with the client. |
| Below-threshold alert | **Tồn kho dưới ngưỡng** | Raised by UC-11 and sent by email; inside NBSP it appears only on `S-F02-02`. No dedicated screen. |

**Receipt confirmation note:** the UI state "Sẵn sàng xác nhận" applies only when the shipment is in a status that can be confirmed under FR-01-07 and the user **has physically received and checked the shipment**. If the request is `Dispatched` but still in transit, show the guidance "Chỉ xác nhận sau khi nhận và kiểm tra lô hàng"; never infer delivery from the ETA.

### 1.3. Standard labels

| Function | Vietnamese labels to use |
|---|---|
| Authentication | Đăng nhập; Đăng xuất; Tài khoản; Mật khẩu |
| Requests and dispatch | Yêu cầu cấp máu; Danh sách yêu cầu; Hàng đợi yêu cầu; Xem yêu cầu; Phê duyệt và điều phối; Không phê duyệt yêu cầu; Lý do không phê duyệt |
| Delivery and receipt | Chi tiết lô hàng; Hành trình lô hàng; Dự kiến đến (ETA); Xác nhận đã nhận máu |
| Inventory | Tổng quan tồn kho; Nhóm máu; Số lượng (đơn vị); Đơn vị máu gần hết hạn; Đồng bộ gần nhất; Dưới ngưỡng; Số dư đầu kỳ; Hạn dùng sớm nhất |
| Clinical data | Ghi nhận sử dụng máu; Nhu cầu theo kế hoạch; Dữ liệu bất thường; Chấp nhận; Loại khỏi dự báo; Yêu cầu xác minh |
| Forecast | Dự báo nháp; Cần xem xét; Bình thường; Duyệt nguyên giá trị; Điều chỉnh và duyệt; Từ chối; Duyệt nhóm Bình thường; Công bố phiên bản; Phiên bản đã công bố |
| Common actions | Tìm kiếm; Lọc; Xóa bộ lọc; Làm mới dữ liệu; Quay lại; Hủy; Lưu thay đổi; Xem chi tiết |

---

## 2. Size, layout and data format

### 2.1. Standard desktop frame for the whole project

| Property | Convention |
|---|---|
| **Figma frame size** | **1440 × 1024 px**: *width = 1440*, *height = 1024* |
| Target device | Desktop browser; no 360 px mobile design in release 1 |
| Canvas | One frame per screen; UI states may be separate frames for review, **but keep the Screen ID** in the frame name |
| Outer margin (proposed) | 16 px |
| Shared sidebar (proposed) | 240 px wide; menu items depend on the user's role |
| Sidebar–content gap (proposed) | 16 px |
| Main area (from the proposed frame) | 1152 px = 1440 − 16 (left margin) − 240 (sidebar) − 16 (gap) − 16 (right margin) |
| Top bar (proposed) | 64–72 px high; title or search and account information |
| Content padding (proposed) | 24 px; 16–24 px between cards |
| Content grid (proposed) | 12 columns in the main area; 16 px gutter; table/card layout per task |
| Scrolling | **Vertical** scrolling is allowed when content exceeds 1024 px; never shrink the font to fit the frame |
| Narrower windows | Outside the standard wireframe; responsive behaviour needs its own decision; no mobile design by default |

**Common layout:** left sidebar (brand and role-based menu) → top bar (search only where the screen has searchable data, account information) → breadcrumb and title → main content (form, table or cards) → action area. The sign-in screen `S-F04-01` **does not need the sidebar** but uses the same fonts, colours, inputs and message conventions.

### 2.2. Data display rules

| Information | Convention | Example |
|---|---|---|
| Date, expiry date | `dd/MM/yyyy` | `17/11/2026` |
| Time | `HH:mm` (24-hour) | `14:30` |
| Date and time, ETA, needed-by | `dd/MM/yyyy HH:mm`, Vietnam time `UTC+07:00` where it needs stating | `18/11/2026 09:30` |
| Blood type | ABO + Rh, medical notation kept | `O+`, `A−`, `AB+` |
| Quantity | Non-negative whole number when **displayed**; input fields follow the business rules (BR-01: whole number ≥ 1, one unit = one 350 ml bag) and carry the unit label `đơn vị` | `4 đơn vị` |
| Blood product | Name from the approved catalogue (release 1: red blood cells only, D-12); never invent a list | `Hồng cầu lắng` |
| Request / shipment / unit ID | Show **the code issued by NBSP** as is, copyable where useful; ID generation rules are not set in this document | `[Mã yêu cầu]`, `[Mã lô]`, `[Mã đơn vị]` |
| Urgency | One of the three values of BR-01 with the labels in §1.2 | `Mức độ khẩn: Khẩn` |
| Inventory and totals | Title stating the scope (hospital or NBC), the figures and the latest sync time when stored | `Cập nhật lần cuối: [Ngày giờ]` |
| Near-expiry units | Show the expiry date and the days remaining; **do not state the warning window** until it is defined (configurable, D-19, OQ-04) | `Còn [N] ngày` |
| Demand and forecast data | State the hospital, period, blood type and unit of every figure; no numbers without context | `Kỳ dự báo: [Khoảng thời gian]` |
| Field with no data | `—` or an empty-state sentence; **never show 0 when there is no real data** | `Chưa có dữ liệu` |
| Currency | Not used on any current screen; design it only for a newly approved requirement | Not applicable |

**Critical medical fields:** unit ID, blood type, product, expiry date, quantity, status and recording history must be easy to read; never rely on colour alone.

---

## 3. Shared UI components

| Component | Project-wide convention |
|---|---|
| **Sidebar / menu** | One shared structure; items shown by role. The current item uses background + bar/marker + text, not colour alone. No notification badge on the menu (alerts reach users by email, OQ-03). |
| **Top bar** | Consistent system and user names; a search box only where there is searchable data. No fake global search. |
| **Breadcrumb / title** | Vietnamese business names; breadcrumb links only to screens in the Screen List. |
| **Primary button** | At most one primary action **per task area**; the label states the result. Proposed height 44–48 px. While sending, show loading/disabled to prevent double submission. |
| **Secondary / destructive button** | Back, cancel and refresh are secondary. Actions with significant business effect (exclude data, not approve a request, reject a forecast item) use clear labels and an appropriate confirmation. |
| **Input / select / date / textarea** | Visible label even when filled; mark required fields; helper text and error message directly under the field. Never use the placeholder as the label. |
| **Data table** | Clear headers, numbers right-aligned where sensible, empty/loading/error states, paging or scrolling for large data (per spec). Every row keeps its identifier. |
| **Filters** | Based on permissions and data source, e.g. hospital, date range, blood type, status. Show only filters that exist in an FR or Screen Spec. Show which filters are active. |
| **Status badge / chip** | Always has **text**, optionally an icon; colour only supports it. Keep the database status and the explanatory UI label apart. |
| **Progress / timeline** | For shipments and forecast cycles where there are confirmed business steps; never mark "Đã giao tới bệnh viện" from `Dispatched` alone. |
| **Confirmation modal** | States what will happen, the object and its code, the main consequence, and offers "Hủy" plus the full action label; never a new Screen ID. The **"Không phê duyệt yêu cầu"** modal on `S-F01-04` is the one listed in the Screen List. |
| **Notification / banner / toast** | Says what happened and what to do next. A toast never replaces a field error; errors that block the task use a banner or inline message. |
| **Disabled state** | Lock an action only for a reason (missing data, no permission, sending, business condition not met) and explain the reason. |
| **Sync information** | Inventory from an external source may show "Đồng bộ gần nhất" when the timestamp exists; never label a snapshot "thời gian thực" (FR-02-05, UC-17). |
| **Server confirmation** | Show "success" only after the system confirms; if the network fails, do not treat the action as done. |

### 3.1. Five states for wireframe review

For **every screen where the risk applies**, check the states below. They are **UI states**: they do not create use cases, Screen IDs or data statuses.

| State | How it is shown | Example |
|---|---|---|
| **1. Default / with data** | Full real information; buttons and filters match the role | Request list, inventory table, demand data list |
| **2. Empty / no data** | No record in the current scope; state the next possible step | "Chưa có lô hàng cần xác nhận"; "Không có bản ghi phù hợp bộ lọc" |
| **3. Loading** | Skeleton or spinner in the data area; disable actions that need the data | Loading shipment details; retrieving the forecast |
| **4. Business in progress / not yet allowed**, or **Error**, depending on the screen | On *receipt confirmation*, use "**Đang vận chuyển**"; this is not an error. On input and dispatch screens, show validation or stock-shortage errors with a specific reason. | "Lô hàng đang vận chuyển. Chỉ xác nhận sau khi nhận và kiểm tra lô." / "Không đủ đơn vị máu hợp lệ để điều phối." |
| **5. Success** | Server confirmation + business result + next step | "Đã ghi nhận sử dụng máu"; "Đã xác nhận nhận máu"; "Đã công bố phiên bản dự báo" |

**Note:** technical and validation errors must still be designed wherever there is an error branch, even though state 4 of the receipt-confirmation wireframe is used for "Đang vận chuyển". UI states never change the data-saving rules in W1.

### 3.2. Conventions per screen in the Screen List

| Screen ID — Vietnamese UI title | Actor | Required content / behaviour | Trace |
|---|---|---|---|
| `S-F04-01` — **Đăng nhập** | All 4 actors | Account and password; authentication error; after sign-in, go to the right scope (or back to the screen an email link asked for) | UC-06, UC-12 · FR-04-01, FR-04-02 |
| `S-F01-01` — **Yêu cầu cấp máu** | Hospital Staff | Blood product, blood type, quantity, urgency, needed-by date and time, contact name and phone, optional reason (BR-01, BR-10); per-field validation; keep entered values on error | UC-01 · FR-01-01, FR-01-06 |
| `S-F01-02` — **Chi tiết lô hàng & xác nhận nhận máu** | Hospital Staff | List of the hospital's requests with their status (with the reason when Không được phê duyệt); shipment details (units, expiry dates, ETA) once `Dispatched`; confirm only after physical check; block early or repeated confirmation | UC-02, UC-03 · FR-01-04, FR-01-05, FR-01-07 |
| `S-F01-03` — **Hàng đợi yêu cầu** | Dept | Pending requests in scope, ordered by urgency then submission time (BR-05); link to open a request | UC-07 · FR-01-02 |
| `S-F01-04` — **Xem xét & điều phối đơn máu** | Dept | Request details, available stock, FEFO pre-selected units and ETA per spec; stock-shortage branch; **"Không phê duyệt yêu cầu"** modal with a reason from the list (FR-01-09) | UC-08 · FR-01-03, FR-01-08, FR-01-09 |
| `S-F02-01` — **Ghi nhận sử dụng máu** | Hospital Staff | Tab "Ghi nhận sử dụng": quantity used or discarded and reason; no inventory change if the data is invalid or exceeds stock. Tab "Số dư đầu kỳ": entered once, per blood type the quantity and the earliest expiry date at a cut-off date, stored as one lot; later changes by the NBSP administrator | UC-04 · FR-02-01, FR-02-06, FR-02-07 |
| `S-F02-02` — **Tổng quan tồn kho** | Hospital Staff, Dept, NBC Operations (read-only, same scope as the Dept) | Only data within the permitted scope; list and totals; latest sync time when available; for NBC roles, a banner and a "Dưới ngưỡng" badge on the affected blood-type tiles (UC-11) | UC-09 · FR-02-02 (FR-02-04 display) |
| `S-F02-03` — **Đơn vị máu gần hết hạn** | Dept | Expiry date, days remaining, list by the configured criterion, earliest expiry first | UC-10 · FR-02-03 |
| `S-F03-01` — **Ghi nhận nhu cầu máu theo kế hoạch** | Hospital Staff | Demand tied to scheduled surgery or periodic transfusion; period label and quantity | UC-05 · FR-03-01 |
| `S-F03-02` — **Rà soát dữ liệu nhu cầu** | Forecasting Analyst | Filters for hospital, date range and blood type; anomaly flag + reason; Chấp nhận / Loại / Yêu cầu xác minh | UC-13 · FR-03-02, FR-03-03 |
| `S-F03-03` — **Phản hồi yêu cầu xác minh** | Hospital Staff | Flagged record and analyst note; confirm, correct or supplement (UC-14); change history kept | UC-14 · FR-03-04 |
| `S-F03-04` — **Xem xét & duyệt dự báo** | Forecasting Analyst | Two groups Cần xem xét / Bình thường; a decision **per item** (approve as is, adjust and approve with a reason, reject with a reason); approve the Normal group; publish a version containing only approved items | UC-15 · FR-03-06 |
| `S-F03-05` — **Dự báo đã duyệt** | NBC Operations | Only the published forecast, with forecast period and data scope; read access by role | UC-16 · FR-03-07 |

**No screen** for `UC-11` (below-threshold alert), `UC-17` (automatic sync), `FR-03-05` (automatic draft forecast), `FR-03-08` (close validation period), `FR-04-03` (authorisation) or `FR-04-04` (audit). Show the relevant feedback on the related screen, by email, or as a background process.

---

## 4. Message standards

### 4.1. How to write a message

- **Success:** `[What was completed] + [Result] + [Next action, if any]`.
- **Data error:** `[What is invalid] + [Field or record concerned] + [How to fix it]`; keep the entered data where the business rule allows.
- **Not yet allowed:** `[Current status] + [Why it cannot be done] + [What to do next]`.
- **Unknown result (connection):** do not claim success or failure before the server answers; ask the user to reload the current status before trying again.
- **Warning:** give context (blood type, inventory scope, time) and a path to the related information when a valid target screen exists.

### 4.2. Vietnamese message library (proposed)

The messages below are **UI copy**. They add no behaviour or business branch beyond the current FRs and Usage Flow.

| Context | Displayed message (Vietnamese) | Where |
|---|---|---|
| Sign-in failed | **"Không thể đăng nhập. Vui lòng kiểm tra thông tin tài khoản và thử lại."** | `S-F04-01`, next to the form / banner |
| No permission | **"Tài khoản không có quyền truy cập nội dung này."** | Requested screen, without showing forbidden data |
| Blood request sent | **"Đã gửi yêu cầu cấp máu. Yêu cầu đang chờ NBC xử lý."** | `S-F01-01`, success |
| Invalid request | **"Chưa tạo yêu cầu. Vui lòng sửa các trường được đánh dấu và gửi lại."** | `S-F01-01`, inline + banner |
| No shipment yet | **"Chưa có thông tin lô hàng được điều phối cho yêu cầu này. Vui lòng kiểm tra lại sau."** | `S-F01-02`, empty (request `Pending`) |
| Request not approved | **"Yêu cầu không được phê duyệt. Lý do: [Lý do]. Nếu vẫn cần máu, vui lòng gửi yêu cầu mới."** | `S-F01-02`, request detail (`Not approved`) |
| Shipment in transit | **"Lô hàng đang vận chuyển. Chỉ xác nhận sau khi bệnh viện đã nhận và kiểm tra lô."** | `S-F01-02`, status banner |
| Confirmation too early | **"Chưa thể xác nhận nhận máu vì lô hàng chưa ở trạng thái đã xuất kho/điều phối."** | `S-F01-02`, error FR-01-07 |
| Repeated confirmation | **"Lô hàng này đã được xác nhận nhận máu. Không ghi tăng tồn kho lần nữa."** | `S-F01-02`, error FR-01-07 |
| Receipt confirmed | **"Đã xác nhận nhận máu. Các đơn vị máu được ghi nhận vào tồn kho bệnh viện."** | `S-F01-02`, success |
| Not enough eligible stock | **"Không đủ đơn vị máu hợp lệ để điều phối. Đơn chưa được chuyển sang Đã xuất kho."** | `S-F01-04`, error FR-01-08 |
| Request not approved (Dept side) | **"Đã ghi nhận quyết định không phê duyệt cùng lý do."** | `S-F01-04`, success / modal |
| Invalid usage record | **"Chưa lưu dữ liệu sử dụng. Kiểm tra các trường bắt buộc và số lượng còn trong kho."** | `S-F02-01`, inline + banner |
| Usage saved | **"Đã ghi nhận lượng máu sử dụng và cập nhật tồn kho bệnh viện."** | `S-F02-01`, success |
| Opening balance saved | **"Đã lưu số dư đầu kỳ làm tồn kho khởi đầu của bệnh viện."** | `S-F02-01`, tab Số dư đầu kỳ, success |
| No near-expiry units | **"Không tìm thấy đơn vị máu gần hết hạn theo phạm vi đang chọn."** | `S-F02-03`, empty (shorter wording is S-F02-03 OQ-13) |
| Below threshold | **"Tồn kho nhóm máu [Nhóm máu] đang thấp hơn ngưỡng quy định. Xem tổng quan tồn kho."** | UC-11 email; banner on `S-F02-02` |
| Inventory may be outdated | **"Dữ liệu tồn kho được đồng bộ gần nhất lúc [Ngày giờ]. Vui lòng kiểm tra trước khi quyết định."** | `S-F02-02`, when a timestamp exists |
| Anomalous demand data | **"Bản ghi này có dấu hiệu bất thường: [Lý do]. Vui lòng chọn hướng xử lý."** | `S-F03-02`, next to the record |
| Verification requested | **"Đã ghi nhận yêu cầu xác minh. Bản ghi đang chờ phản hồi từ bệnh viện."** | `S-F03-02`, success |
| Hospital responded | **"Đã gửi thông tin xác minh. Hệ thống đã ghi nhận phản hồi để chuyên viên xem xét."** | `S-F03-03`, success |
| Verification overdue | **"Yêu cầu xác minh đã quá hạn. Vui lòng xem lại bản ghi và quyết định hướng xử lý theo quy định."** | `S-F03-02`, warning |
| Forecast item approved | **"Đã duyệt mục dự báo."** | `S-F03-04`, action status |
| Forecast item rejected | **"Đã ghi nhận quyết định từ chối mục dự báo cùng lý do. Mục này không được công bố."** | `S-F03-04`, action status |
| Forecast version published | **"Đã công bố phiên bản dự báo. Phiên bản chỉ gồm các mục đã duyệt và sẵn sàng cho bộ phận vận hành xem."** | `S-F03-04`, success |
| No matching data | **"Không có dữ liệu phù hợp với bộ lọc hiện tại. Hãy thay đổi bộ lọc hoặc thử lại."** | Tables and lists, empty |
| Unknown result (connection lost) | **"Chưa xác định được kết quả thao tác. Hãy tải lại trạng thái hiện tại trước khi thử lại."** | Screens that write data, banner |

**Result rule:** never call an action "successful" when the user presses the button. Wait for the system's answer or a verified status.

---

## 5. Accessibility and usability

- **Contrast:** normal text at least **4.5:1**; icons and component borders at least **3:1** against their background. Use colour pairs that have been measured; never rely on green/red alone.
- **Target size:** at least **24 × 24 CSS px**; use **40–44 px** for buttons and main interactive targets on desktop. Suggested primary button height **44–48 px**.
- **Keyboard:** Tab order follows the reading order; visible focus; Enter/Space activate the right control; Escape closes a modal where appropriate without losing data unexpectedly.
- **Forms:** labels independent of placeholders; required fields and errors marked; error descriptions linked to their fields; the user can correct without losing entered data (where the FR requires it).
- **Dynamic messages:** announce loading, success and error in a way assistive technology can read; never rely only on a toast that disappears.
- **Data tables:** clear column headers, units attached to values, unit IDs, expiry dates and statuses never truncated; table title, filter information and empty-state message.
- **Typography:** a font that supports Vietnamese; do not shrink long data to unreadable sizes; body text about **14–16 px**. No icon-only buttons for high-risk actions (dispatch, receipt confirmation, forecast approval).
- **Standard screen:** check at **1440 × 1024 px** with the real frame content; check scrolling and overflow rather than cutting important business information.

---

## 6. Design tokens and visual conventions — proposed for Figma

The values below use a **green/white/grey palette** close to the "HuyếtMạch" wireframe the team used, replacing the blue palette of the old Flow 1 version. **These are proposed tokens, to be confirmed before the design system is frozen (OQ-08).** Tokens do not decide business flows.

| Token | Proposed value | Use |
|---|---|---|
| `color/primary` | `#14633F` | Primary action, current menu item, focus ring |
| `color/primary-dark` | `#0B5135` | Emphasis and headings on light backgrounds |
| `color/background` | `#EEF1F0` | Desktop page background |
| `color/surface` | `#FFFFFF` | Cards, forms, modals |
| `color/surface-subtle` | `#F8FAF9` | Sidebar, light information boxes |
| `color/text-primary` | `#172A24` | Titles and main text |
| `color/text-secondary` | `#566A60` | Captions, secondary labels. Changed from `#62756B`, which is only about 4.3:1 on `color/background`; `#566A60` is the value already used and measured in the S-F02-02 and S-F02-03 specs |
| `color/border` | `#DCE5DE` | Dividers and borders with no message meaning |
| `color/border-strong` | *to define* | Input and chip borders (proposed in the S-F01-01 spec) |
| `color/success-bg` | `#E5F4E9` | Success state background |
| `color/warning` | `#9A3412` | Warning text on a measured light background |
| `color/warning-bg` | `#FDF3EC` | Warning banner and chip background (used in S-F02-03, 6.7:1 with `color/warning`) |
| `color/error` | `#8A1C2D` | Error text on a measured light background |
| `color/error-bg` | `#FBEFF1` | Error banner background (used in S-F02-03, 8.2:1 with `color/error`) |
| `color/disabled-bg` | *to define* | Disabled fields and the "sending" banner (proposed in the S-F01-01 spec) |
| `font/family` | `Inter`, fallback `Arial, sans-serif` | Font with Vietnamese support |
| `font/page-title` | `28 px`, bold | Screen name |
| `font/section-title` | `20 px`, bold | Card or group title |
| `font/body` | `16 px`, regular | Body text |
| `font/table-label` | `14 px`, medium | Labels, tables, forms |
| `font/helper` | `12–13 px` | Helper text, never for core content |
| `spacing/base` | `8 px`, with a `4 px` half-step | Spacing in multiples |
| `radius/card` | `16–20 px` | Cards and panels |
| `radius/control` | `8–12 px` | Buttons, inputs, selects |
| `height/button-primary` | `44–48 px` | Primary action |

**Token rules:** use them consistently on all 13 screens; status colours only support a text label; do not flatten a mockup into a single image layer if it still needs editing in Figma; a colour that is not a token is either added here or replaced by an existing token.

---

## 7. Links between screens, actors and use cases

- **Hospital — Flow 1:** `S-F04-01` → `S-F01-01` → *(NBC dispatches or does not approve: a step of another actor)* → `S-F01-02` (request list; UC-02 and UC-03 on the same screen). There is no separate sign-in screen per actor.
- **Hospital — Flows 2 and 3:** from the shared sign-in to `S-F02-01` / `S-F03-01` (Flow 2) or `S-F03-03` (Flow 3, also from the link in the verification email); `S-F02-02` is available by permission (UC-09).
- **Dept — Flow 1:** `S-F04-01` → `S-F01-03` → optionally the inventory on `S-F02-02` → `S-F01-04` (approve and dispatch, or not approve with a reason). A stock shortage does not create a new page; it is explained on the dispatch screen.
- **Dept — Flow 2:** `S-F04-01` → `S-F02-02` → optionally `S-F02-03`. The UC-11 alert arrives by email and is shown on `S-F02-02`; it is not a screen.
- **Analyst — Flow 1:** `S-F04-01` → `S-F03-02`; a verification request goes to the hospital by email and is answered on `S-F03-03` (UC-14). Do not assume that UC-13 creates a forecast immediately.
- **Analyst — Flow 2:** when a draft is ready (email), `S-F04-01` (if not signed in) → `S-F03-04`; item decisions, the Normal-group approval and publishing all happen on this screen.
- **NBC Operations — Flows 1 and 2:** `S-F04-01` → `S-F03-05` (published forecast) or `S-F02-02` (inventory, read-only). No threshold configuration screen.

**Traceability:** screen names and IDs follow `screen-list.md`; journeys follow `usage-flow.md` (current version v2.3); the original conventions came from `NBSP_UI_Conventions_Hospital_Flow1(2).md`. Every screen spec refers to this document instead of copying it into its own version.

---

## 8. Open questions for the team

| ID | Finding | Impact | Where to decide / status |
|---|---|---|---|
| **OQ-01** | The Screen List had the "not approve request" modal (FR-01-09) while the usage flow said release 1 had no not-approved branch in UC-08. | Whether `S-F01-04` has the action and the modal. | **Decided 21/11/2026:** Not approved exists (W0 BR-02, W1 FR-01-09, AD-UC-08 v0.5); `usage-flow.md` updated. |
| **OQ-02** | The usage flow said NBC Operations also uses UC-09 (`S-F02-02`) while the Screen List named only Hospital Staff and the Dept. | Menu rights and data scope per actor. | **Decided 21/11/2026:** NBC Operations sees `S-F02-02` read-only with the same scope as the Dept; Screen List and SD-UC-09 updated. |
| **OQ-03** | The UC-11 alert channel was described as an in-system notification while the Screen List said the channel was to confirm. | Banner/notification position and deep links. | **Decided 21/11/2026:** email to the Dept and NBC Operations; inside NBSP only a banner and a "Dưới ngưỡng" badge on `S-F02-02`; no menu badge. |
| **OQ-04** | The near-expiry window of UC-10 (number of days, per product) is not defined; it is configurable (W0 Table 3, D-19). Urgency values are now defined in BR-01. | The near-expiry filter text and sample values cannot be fixed. | Open: business owner / W1; do not invent the window. Urgency labels in §1.2 are proposed and need client confirmation. |
| **OQ-05** | Five screen specs exist (`S-F01-01`, `S-F02-02`, `S-F02-03`, `S-F03-02`, `S-F03-04`); eight are still planned paths in the Screen List (`screens/s-fxx-nn/screen-spec-s-fxx-nn.md`). | Field- and interaction-level details of those eight screens cannot be checked yet. | Check every spec against this document before handover to development, starting with `S-F01-02` and `S-F01-04`. |
| **OQ-06** | `Dispatched` does not mean "delivered to the hospital"; physical receipt is the hospital's action in UC-03. | The receipt wireframe must not show delivery success from the ETA alone. | Decide a delivery-proof source only if a separate progress step is wanted. |
| **OQ-07** | The usage flow says thresholds are configured at deployment, with no configuration screen; the Context Diagram needed a fix. | Avoid adding a screen or rights to manage thresholds outside the current scope. | **Done:** Context Diagram v4.1 has no "configure thresholds"; W0 Table 3 records threshold configuration as Won't-have. |
| **OQ-08** | Colours, fonts and some component sizes are **proposed** from the HuyếtMạch wireframe; there is no approved design system. `color/border-strong` and `color/disabled-bg` have no value yet. | UI consistency once all screens are integrated. | The team approves the tokens once in Figma / this document, then everyone uses them. |

---

## 9. Checklist before design handover / pushing to GitHub

- [ ] All UI copy on the 13 screens is in Vietnamese (except codes, proper names or technical labels that must stay).
- [ ] Main frames are **1440 px wide × 1024 px high**; the sign-in screen uses the same desktop ratio.
- [ ] Screen ID, business name and actor match `screen-list.md`; any mismatch is logged as an open question.
- [ ] No separate screen for success, error, modal, UC-11 or UC-17 unless approved.
- [ ] Forms have labels, validation rules, specific errors and loading/disabled states.
- [ ] System status and displayed label are never confused; nothing moves to `Received` automatically.
- [ ] Every data-writing action reports success only after server confirmation; no repeated processing when the network is uncertain.
- [ ] All screens share the palette, typography, spacing and button names; every colour used is a token.
- [ ] Key branches are checked against the matching AD, SD and FR, especially UC-03, UC-04, UC-08, UC-13 and UC-15.
- [ ] Contrast, keyboard navigation, focus, table readability, error messages and role-based access are checked.

### Sources

1. `screen-list.md` — Screen List, 13 screens, draft.
2. `usage-flow-total (1).md` — merged usage flow of the 4 actors, v2.2 of 07/11/2026 (current version: `usage-flow.md` v2.3, 20/11/2026).
3. `NBSP_UI_Conventions_Hospital_Flow1(2).md` — earlier UI conventions, Hospital Staff Flow 1 only.
4. HuyếtMạch illustrations and wireframes from the workshop — used only to **propose** colours and layout; they do not replace W0 or W1.
5. Screen specs `S-F01-01`, `S-F02-02`, `S-F02-03` (tokens and measured contrast); W0 business rules BR-01, BR-02, BR-05, BR-07, BR-10.
