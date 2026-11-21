# Screen Specification:S-F02-02 Inventory dashboard 

> DBIZ2 — Group C · NBSP · Draft v0.1 · 21/11/2026
> Structure follows the Screen Specification template described in the master prompt §7. The reference file `screen-spec-s-f01-01.md` was **not** supplied with this batch, so section order and table formats follow the prompt; align headings with S-F01-01 once it is available (see OQ-17).
> UI copy is Vietnamese (UI Conventions §1). Codes (`UC-xx`, `FR-xx-xx`, `S-Fxx-xx`) stay in English.

---

## 1. Screen metadata

| Field | Value |
|---|---|
| Screen ID | `S-F02-02` |
| Screen name | Inventory dashboard — UI title **Tổng quan tồn kho** |
| Actor | Hospital Staff · NBC Blood Storage and Distribution Department · NBC Operations (Screen List v2; see OQ-02) |
| Priority | **Should** (Screen List). Function List gives FR-02-02 initial priority **Must** — conflict, see OQ-01 |
| Feature | F-02 |
| Linked UC / FR | UC-09 View Inventory Dashboard · FR-02-02 View role-scoped inventory dashboard |
| Related (not served by this screen) | UC-17 / FR-02-05 Synchronise NBC inventory (source of the sync timestamp); UC-11 / FR-02-04 Below-threshold alert (entry point only); FR-04-03 Role-based authorisation (cross-cutting) |
| Sequence / activity diagrams | SD UC-09 (Mid-term Report, "UC-09: View inventory dashboard"): `alt` Hospital Staff → identify hospital role and access scope → query own hospital inventory + NBC available stock; `alt` NBC Staff → identify NBC role → query NBC + each hospital's inventory; filter by hospital or blood type re-queries; access denied → message. No activity diagram for UC-09 was supplied. |
| Usage flow | Hospital Staff: menu "Inventory" or "[usage saved] View inventory" from S-F02-01. NBC Dept Flow 1: S-F01-03 → "Check stock" → S-F02-02 → "Back to queue". NBC Dept Flow 2: S-F04-01 → S-F02-02 → S-F02-03. NBC Operations Flow 2: S-F04-01 → S-F02-02. Below-threshold alert (UC-11) → S-F02-02 (screen-transition-2.md §1, §2, §4) |
| Mockup images | `img/s-f02-02-default.png` · `img/s-f02-02-empty.png` · `img/s-f02-02-loading.png` · `img/s-f02-02-error.png` · `img/s-f02-02-success.png` · role variant `img/s-f02-02-default-hospital-staff.png` |
| Frame dimensions | Width 1440 px (all states). Height content-dependent: Default 1307, Empty 1024, Loading 1364, Error 1081, Success 1431, Hospital variant 1114 px. PNGs exported at 2× (2880 px wide). |
| Figma | File **NBSP — S-F02-02 & S-F02-03 (5 UI states)** — https://www.figma.com/design/3OinFn4dX034IiohmTAYJ3 · Frames: `S-F02-02 — Default` (node 4-2), `S-F02-02 — Empty` (3-2), `S-F02-02 — Loading` (5-2), `S-F02-02 — Error` (3-131), `S-F02-02 — Success` (4-306), `S-F02-02 — Default (Hospital Staff scope)` (5-271) |
| Version | v0.1 |
| Status | Draft — for team review. Blocking questions: OQ-02, OQ-05 |

---

## 2. Purpose

**Why the screen exists.** A user checks blood stock. The system identifies the user's role and shows only the inventory scope that role may see, by blood type, together with the latest NBC synchronisation timestamp (FR-02-02, UC-09).

**When it is shown.** When an authorised user opens the dashboard from the menu, after sign-in, from S-F02-01's success state, from S-F01-03 ("Check stock"), or from a below-threshold alert (Screen List entry points; screen-transition-2.md).

**How the user enters.**

| Entry point | Actor | Source |
|---|---|---|
| Home after sign in (S-F04-01) | NBC Dept, NBC Operations | Screen List; transition §2, §4 |
| Menu "Tổng quan tồn kho" | All three actors | Screen List; transition §1, §2, §4 |
| S-F02-01 success state → "View inventory" | Hospital Staff | Screen List; transition §1 |
| S-F01-03 → "Check stock" | NBC Dept | transition §2 |
| Below-threshold alert (UC-11) | NBC Dept, NBC Operations | Screen List; transition §2, §4 |

**Where the user goes next.** S-F02-03 (Dept), S-F01-03 (Dept, "Back to queue"), S-F03-05 (Operations, via menu), any other menu item permitted for the role, or Sign out.

**Read-only.** The screen does not create, change or confirm any blood transaction. Opening it does not start a synchronisation with the Blood Management Software (Mid-term Report Assumptions: NBC inventory is synchronised periodically; FR-02-05 is a scheduled job).

---

## 3. Mockup

### 3.1 Shared layout (all states)

`Sidebar (240 px) | Main area (1152 px)` inside a 16 px outer margin, per UI Conventions §2.1.

1. **Sidebar** — NBSP brand block, role card ("Vai trò"), role-based menu with the current item highlighted (background + left bar + bold text, not colour alone), footer label.
2. **Topbar** (68 px) — organisation/unit of the signed-in account, avatar + user name, role chip, "Đăng xuất".
3. **Page header** — breadcrumb "Kho máu › Tổng quan tồn kho", H1 "Tổng quan tồn kho", subtitle, review tag (state name + "Dữ liệu minh họa"), header actions (Dept only).
4. **Scope card** and **Sync card** side by side.
5. **Filter card** — "Bệnh viện / kho" (NBC roles only), "Nhóm máu", "Áp dụng bộ lọc", "Xóa bộ lọc", active-filter line.
6. **Status region** (state-dependent banner) — empty in Default.
7. **Data region** — NBC section (tiles by blood type) and hospital section (table by hospital × blood type); replaced by empty/error/skeleton content depending on state.

The review tag ("Wireframe S-F02-02 · Trạng thái … · Dữ liệu minh họa, không phải số liệu vận hành") is a wireframe annotation and **is not part of the production UI**.

All sample values (hospital names "Bệnh viện Đa khoa A … F", quantities, user names, timestamps) are **illustrative**.

### 3.2 State images

| State | Image | What visibly differs from Default |
|---|---|---|
| 1 Default | ![S-F02-02 Default](../img/s-f02-02-default.png) | Baseline: all blood types, no filter, NBC tiles + 6-hospital table, sync timestamp. |
| 2 Empty | ![S-F02-02 Empty](../img/s-f02-02-empty.png) | Filter = one hospital with no inventory records; data region replaced by empty illustration, message and "Xóa bộ lọc". Sync card still shown. |
| 3 Loading | ![S-F02-02 Loading](../img/s-f02-02-loading.png) | Filters and buttons disabled; "Đang tải…" on apply button; loading banner (spinner + text); skeleton tiles, skeleton table, skeleton sync timestamp. |
| 4 Error | ![S-F02-02 Error](../img/s-f02-02-error.png) | Red bordered banner with ⚠ icon, title, "Đây không phải tồn kho bằng 0", "Thử lại"; sync value "— Chưa tải được"; dashed placeholders instead of numbers. |
| 5 Success | ![S-F02-02 Success](../img/s-f02-02-success.png) | After applying filter Nhóm máu = O−: green status banner (load time, filter, sync time, "không phát sinh giao dịch"), active-filter chip, one NBC tile, table reduced to O− column. |
| Role variant | ![S-F02-02 Hospital Staff](../img/s-f02-02-default-hospital-staff.png) | Hospital Staff menu; scope "Bệnh viện Đa khoa A và tồn kho NBC sẵn sàng phân phối"; only "Nhóm máu" filter; sections "Tồn kho của Bệnh viện Đa khoa A" and "Tồn kho NBC sẵn sàng phân phối"; no hospital table; no Dept header actions. |

NBC Operations is not drawn as a separate variant: its data scope is not defined in any business source (OQ-02). Until confirmed it is assumed to use the NBC Dept layout without the Dept-only header actions, with the Operations menu (Dự báo đã duyệt, Tổng quan tồn kho).

---

## 4. Element inventory

Field names are not defined in W0/W1; every data source below is a **logical** source with field name **TBD** (OQ-14).

| # | Element | Type | Content / Data source | Required | Validation |
|---|---|---|---|---|---|
| E01 | NBSP brand block | Logo + text | Static: "NBSP", "Nền tảng chuỗi cung ứng máu quốc gia" | Yes | — |
| E02 | Role card | Info box | "Vai trò" + role label from session (FR-04-01/02 session role). Dept: "Bộ phận Lưu trữ & Phân phối máu NBC"; Hospital: "Nhân viên bệnh viện"; Operations: TBD label | Yes | Must match authenticated role |
| E03 | Sidebar menu | Navigation list | Items allowed for the role (FR-04-03). Dept: Hàng đợi yêu cầu (S-F01-03), Tổng quan tồn kho (S-F02-02), Đơn vị máu gần hết hạn (S-F02-03). Hospital: Yêu cầu cấp máu (S-F01-01), Lô hàng (S-F01-02), Ghi nhận sử dụng máu (S-F02-01), Tổng quan tồn kho, Nhu cầu máu theo kế hoạch (S-F03-01), Yêu cầu xác minh (S-F03-03). Operations: Dự báo đã duyệt (S-F03-05), Tổng quan tồn kho | Yes | Only items the role may open; current item has `aria-current="page"` |
| E04 | Organisation / unit | Text | Session: organisation (NBC or hospital) and unit — field TBD | Yes | — |
| E05 | User name + avatar initials | Text + avatar | Session user display name — field TBD | Yes | — |
| E06 | Role chip | Chip | Same value as E02 | Yes | — |
| E07 | "Đăng xuất" | Secondary button | Static | Yes | — |
| E08 | Breadcrumb | Text | "Kho máu › Tổng quan tồn kho" ("Kho máu" is a group label, not a link) | Yes | — |
| E09 | Page title | H1 | "Tổng quan tồn kho" | Yes | — |
| E10 | Subtitle | Text | NBC roles: "Tồn kho theo nhóm máu của kho NBC và từng bệnh viện, trong phạm vi được phân quyền cho tài khoản." Hospital: "Tồn kho theo nhóm máu của bệnh viện và tồn kho NBC sẵn sàng phân phối, trong phạm vi được phân quyền cho tài khoản." | Yes | — |
| E11 | "Quay lại hàng đợi yêu cầu" | Secondary button | Static — **NBC Dept only** | Dept: Yes | Hidden for Hospital Staff and Operations |
| E12 | "Xem đơn vị máu gần hết hạn" | Secondary (outline) button | Static — **NBC Dept only** | Dept: Yes | Hidden for Hospital Staff and Operations (UC-10 actor is the Dept only) |
| E13 | Scope card | Info card | Label "Phạm vi dữ liệu được phép xem"; value derived from role + organisation: NBC roles "Kho NBC và tồn kho của từng bệnh viện"; Hospital "[Tên bệnh viện] và tồn kho NBC sẵn sàng phân phối" + "Tồn kho của bệnh viện khác không hiển thị với tài khoản bệnh viện." | Yes | Text must reflect the scope actually queried |
| E14 | Sync card — label | Text | "Đồng bộ gần nhất — kho NBC" | Yes | — |
| E15 | Sync card — timestamp | Text (large) | Latest NBC snapshot source timestamp stored by FR-02-05 — field TBD. Format `dd/MM/yyyy HH:mm` + "(UTC+07:00)" | Yes (FR-02-02) | Never blank in Default/Empty/Success. Loading: skeleton. Error: "— Chưa tải được". If no snapshot exists yet: "—" + "Chưa có lần đồng bộ" (OQ-07) |
| E16 | Sync card — helper | Text | "Dữ liệu kho NBC được đồng bộ định kỳ từ Blood Management Software, không phải thời gian thực. Vui lòng kiểm tra trước khi quyết định. Tồn kho bệnh viện do NBSP ghi nhận." (UI Conventions §3, §4.2) | Yes | Must not say "thời gian thực" |
| E17 | "Bệnh viện / kho" filter | Select (label above) | Options: "Tất cả (Kho NBC và các bệnh viện)", "Kho NBC", each hospital in NBC scope (list source TBD). **NBC roles only** | NBC: Yes | Options limited to authorised scope. Not rendered for Hospital Staff (would expose other hospitals' names) |
| E18 | "Nhóm máu" filter | Select (label above) | "Tất cả nhóm máu", O+, O−, A+, A−, B+, B−, AB+, AB− (UI Conventions §2.2 notation) | Yes | Single value; allowed list = 8 ABO/Rh types (confirm, OQ-05) |
| E19 | "Áp dụng bộ lọc" | Primary button (44 px) | Static | Yes | Disabled + "Đang tải…" while loading (prevents duplicate queries) |
| E20 | "Xóa bộ lọc" | Secondary button | Static | Yes | Disabled while loading |
| E21 | Active-filter line + chips | Text + removable chips | "Bộ lọc đang áp dụng:" + one chip per applied filter, e.g. "Nhóm máu: O− ×"; "Không có" when none | Yes | Chip "×" removes that filter and re-queries |
| E22 | NBC section title | H2 + meta | NBC roles: "Kho NBC — số lượng theo nhóm máu"; Hospital: "Tồn kho NBC sẵn sàng phân phối". Meta "Đơn vị: đơn vị máu · Theo lần đồng bộ [E15]" | Yes | — |
| E23 | NBC total | Text | Sum of visible NBC tiles, "Tổng[ (filter)] [n] đơn vị" — derived on screen (OQ-13) | No | Non-negative integer |
| E24 | Blood-type tile (× 8, or filtered) | Card | Blood-type badge (text, e.g. "O+") + quantity + "đơn vị". Source: NBC snapshot quantity per blood type (NBC roles: NBC inventory; Hospital: NBC **available distribution** stock) — fields TBD | Yes | Integer ≥ 0. "0" only when a record exists with zero stock; "—" when no record (UI Conventions §2.2) |
| E25 | Own-hospital section (Hospital Staff only) | H2 + meta + total + 8 tiles | "Tồn kho của [Tên bệnh viện]" · "Dữ liệu do NBSP ghi nhận theo giao dịch nhận và sử dụng máu" · quantities from NBSP hospital inventory (UC-03, UC-04) | Hospital: Yes | Only the signed-in user's hospital (FR-04-01 hospital-scoped session) |
| E26 | Hospital section title (NBC roles) | H2 + meta | "Tồn kho bệnh viện theo nhóm máu" · "[n] bệnh viện trong phạm vi vận hành của NBC · Dữ liệu do NBSP ghi nhận · Đơn vị: đơn vị máu" | NBC: Yes | n = hospitals in scope |
| E27 | Hospital inventory table | Data table | Columns: Bệnh viện, one column per visible blood type (right-aligned), Tổng. Rows: each hospital in NBC scope. Source: NBSP hospital inventory aggregated by hospital × blood type (FR-02-02 BE) | NBC: Yes | Not rendered for Hospital Staff. Header row has a caption for assistive tech. Hospitals with no records show "—" and the note "· Chưa có dữ liệu tồn kho" |
| E28 | Table "Tổng" column | Derived column | Row sum of visible blood-type cells; "—" if the row has no records | No | Excludes "—" cells (OQ-13) |
| E29 | Zero/no-data legend | Text | "0 = đã có dữ liệu, tồn kho bằng 0 · — = chưa có dữ liệu tồn kho" | Yes | — |
| E30 | Empty-state block | Illustration + H3 + text + button | Icon (box); "Chưa có dữ liệu tồn kho trong phạm vi đang chọn"; "[Phạm vi] chưa có bản ghi tồn kho nào trong NBSP. Đây là trạng thái chưa có dữ liệu, không phải tồn kho bằng 0."; "Xóa bộ lọc" | Empty state | — |
| E31 | Loading banner | Status banner (`role="status"`, polite) | Spinner + "Đang tải dữ liệu tồn kho đã đồng bộ…" + "Hệ thống đang đọc dữ liệu tồn kho đã lưu trong phạm vi được phép. Mở trang không khởi chạy một lần đồng bộ mới với Blood Management Software." | Loading state | Timeout duration **TBD** (OQ-09) |
| E32 | Skeletons | Placeholder shapes | Tiles, table rows, sync timestamp | Loading state | Same layout as loaded data |
| E33 | Error banner | Alert banner (`role="alert"`), 2 px border `#8A1C2D`, ⚠ icon | "Không tải được dữ liệu tồn kho" · "Hệ thống chưa nhận được dữ liệu tồn kho, vì vậy số liệu bên dưới chưa được hiển thị. **Đây không phải tồn kho bằng 0.** Vui lòng thử lại." | Error state | Icon + text + border (not colour alone) |
| E34 | "Thử lại" | Primary button in banner | Static | Error state | Re-issues the dashboard query only |
| E35 | Error placeholders | Dashed boxes | "Dữ liệu chưa được hiển thị do lỗi tải dữ liệu. Chọn Thử lại ở thông báo phía trên." | Error state | No numbers, no "0" |
| E36 | Success status banner | Status banner (`role="status"`, polite), left accent bar + ✓ icon | "Đã tải dữ liệu tồn kho theo bộ lọc lúc [HH:mm] — [bộ lọc]" · "Số liệu kho NBC theo lần đồng bộ [E15]; tồn kho bệnh viện theo dữ liệu NBSP. Đây là kết quả hiển thị dữ liệu, không phát sinh giao dịch nhập/xuất máu." | Success state | Load time = client time of response; not a transaction confirmation |
| E37 | Access-denied message (not drawn) | Inline message | "Tài khoản không có quyền truy cập nội dung này." (UI Conventions §4.2) — no data rendered | When role has no permission | Required by SD UC-09 "access denied" branch |
| E38 | Review tag (wireframe only) | Dashed label | State name + "Dữ liệu minh họa" | No — not in production | — |

**Deliberately not on this screen** (see §7.9 and OQ-11): Ghi chú, Nhiệt độ bảo quản, Báo sự cố cho NBC, Tải biên bản, Tạo yêu cầu mới, real-time indicator, "Đồng bộ ngay" button, below-threshold banner (OQ-06).

---

## 5. Five UI states

| State | What the user sees | Trigger |
|---|---|---|
| **1 Default** (`s-f02-02-default.png`) | Full shell; scope card; sync card with latest NBC sync timestamp; filters at "Tất cả"; NBC tiles for 8 blood types with total; hospital table (6 illustrative hospitals, one without records shown with "—"); 0 vs — legend. | User opens the dashboard and the role-scoped query returns data (FR-02-02). |
| **2 Empty** (`s-f02-02-empty.png`) | Same shell, scope card, sync card and filters (with the applied filter chip). Data region replaced by empty block "Chưa có dữ liệu tồn kho trong phạm vi đang chọn" and "Xóa bộ lọc". Explicitly says it is **not** zero stock. | Query succeeds but returns no inventory record for the authorised scope + filters (e.g. a hospital that has no NBSP inventory yet). |
| **3 Loading** (`s-f02-02-loading.png`) | Same layout. Filters, "Áp dụng bộ lọc" ("Đang tải…") and "Xóa bộ lọc" disabled. Loading banner states the page reads stored data and does not start a sync. Skeleton tiles, skeleton table, skeleton sync timestamp. Navigation stays usable. | Page open, filter applied, chip removed, or "Thử lại" — until the response arrives. Maximum wait **TBD** (OQ-09). |
| **4 Error** (`s-f02-02-error.png`) | ⚠ bordered banner "Không tải được dữ liệu tồn kho" + "Đây không phải tồn kho bằng 0" + "Thử lại". Sync value "— Chưa tải được". Numbers replaced by dashed placeholders; no stale numbers shown as fresh. | Dashboard data retrieval fails (server/network error). **Design assumption:** a failed NBC synchronisation is *not* an error state of this screen — FR-02-05 keeps the previous snapshot, so the dashboard shows that snapshot with its older timestamp (OQ-07). |
| **5 Success** (`s-f02-02-success.png`) | Green status banner confirming data was loaded for the applied filter, with load time and sync time and an explicit "không phát sinh giao dịch nhập/xuất máu". Filter chip "Nhóm máu: O−"; NBC tile and table reduced to O−; scope and sync card unchanged. | A re-query completes successfully after a user action (apply filter, remove chip, clear filters, "Thử lại"). SD UC-09: "If the staff applies a filter, the system queries the matching records again and shows the updated results." The banner is a read confirmation, not a transaction confirmation. |

---

## 6. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| I01 | Page | Open (menu, sign-in home, alert link, S-F02-01 "View inventory", S-F01-03 "Check stock") | Check role permission (FR-04-03); if denied show E37 and no data. Otherwise Loading → query stored inventory in role scope + latest NBC sync timestamp → Default / Empty / Error | S-F02-02 |
| I02 | E18 Nhóm máu / E17 Bệnh viện / kho | Change selection | Value changes only; no query until applied | — |
| I03 | E19 Áp dụng bộ lọc | Click / Enter | Loading → re-query with filters inside the authorised scope → Success (data), Empty (no records) or Error | — |
| I04 | E21 filter chip "×" | Click | Remove that filter → same as I03 | — |
| I05 | E20 Xóa bộ lọc (filter card or empty block) | Click | Reset all filters to "Tất cả" → same as I03 | — |
| I06 | E34 Thử lại | Click | Loading → repeat the last query. Does **not** trigger an NBC synchronisation | — |
| I07 | E12 Xem đơn vị máu gần hết hạn (Dept) | Click | Navigate | **S-F02-03** |
| I08 | E11 Quay lại hàng đợi yêu cầu (Dept) | Click | Navigate | **S-F01-03** |
| I09 | E03 Menu items | Click | Navigate (only role-permitted items rendered) | Dept: S-F01-03, S-F02-03 · Hospital: S-F01-01, S-F01-02, S-F02-01, S-F03-01, S-F03-03 · Operations: S-F03-05 |
| I10 | E07 Đăng xuất | Click | End session | **S-F04-01** |
| I11 | E24 tile / E27 row | Click | **No action.** No drill-down screen exists in the Screen List | TBD — not designed (OQ-15) |

All targets exist in `screen-list-2.md`.

---

## 7. Screen-level rules

| # | Rule | Source |
|---|---|---|
| R1 | **Role-based access.** Only Hospital Staff, NBC Blood Storage and Distribution Department and NBC Operations may open the screen; others see E37 with no data. | Screen List S-F02-02; FR-02-02; FR-04-03; SD UC-09 (access denied) |
| R2 | **Hospital Staff visibility.** Shows only (a) the user's own hospital inventory and (b) NBC available distribution stock. **Other hospitals' inventory, names and counts are never rendered or returned.** The hospital filter is not shown. | UC-09 (Mid-term Report); SD UC-09 Hospital branch; FR-04-01 |
| R3 | **NBC Dept visibility.** NBC inventory and each hospital's inventory within NBC operational scope. | UC-09; SD UC-09 NBC branch |
| R4 | **NBC Operations visibility.** Read-only; scope as authorised for the role. Assumed equal to R3 until confirmed (OQ-02). Dept-only actions E11/E12 hidden. | Screen List v2; screen-transition §4; Use Case Diagram "(read-only)" |
| R5 | **Read-only.** No create/update/delete, no transaction. Success state = data loaded. | FR-02-02 (EQ); prompt §4 State 5 |
| R6 | **Data freshness.** NBC figures are the latest periodically synchronised snapshot; the latest NBC sync timestamp is always shown (E15). Never labelled "thời gian thực". Opening/refreshing the page does not trigger a sync. Hospital figures come from NBSP transactions. | FR-02-02; FR-02-05; Mid-term Report Assumptions; UI Conventions §3 |
| R7 | **Display by blood type** using ABO+Rh notation; quantities as non-negative integers with unit "đơn vị". | UC-09; UI Conventions §2.2 |
| R8 | **Zero vs no data.** "0" only when a record exists with zero stock; "—" / "Chưa có dữ liệu" when no record. An error never shows "0". | UI Conventions §2.2; prompt §4 State 4 |
| R9 | **Filtering.** Hospital (NBC roles) and blood type; applied filters shown as chips; filter options never exceed the authorised scope; server enforces scope regardless of UI. | FR-02-02 FE work item; SD UC-09; UI Conventions §3 |
| R10 | **Sorting.** Not specified by any source. Display order: blood types O+, O−, A+, A−, B+, B−, AB+, AB−; hospitals in the order returned (design assumption, OQ-16). | — (assumption) |
| R11 | **Error handling.** Retrieval failure → E33/E34; stale data is not presented as freshly loaded; no incident workflow. | Prompt §4 State 4; FR-02-05 (sync failure keeps previous snapshot) |
| R12 | **State communication.** Loading/success use polite live regions; error uses `role="alert"`; every status uses icon + text, not colour alone. | UI Conventions §5 |

---

## 8. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| FR-02-02 | Identifies the role (session), shows only the permitted inventory scope by blood type, shows the latest NBC synchronisation timestamp, provides hospital and blood-type filters (FE work item), consumes the aggregated authorised inventory (BE work item). |
| FR-04-03 | Renders only role-permitted menu items, filters and sections; access-denied message when not permitted. Enforcement is server-side. |
| FR-02-05 | Consumer only: displays the snapshot timestamp written by the scheduled sync. No sync control on screen. |
| FR-02-04 / UC-11 | Entry point only (alert link opens this screen). In-screen alert banner not designed — OQ-06. |

---

## 9. Responsive and accessibility notes

- Target: desktop 1440 px; vertical scroll when content exceeds 1024 px; mobile out of scope (UI Conventions §2.1).
- Every filter has a visible label above the control; no placeholder-as-label.
- Keyboard: Tab order sidebar → topbar → header actions → filters → apply → clear → chips → data. Enter/Space activate buttons; chips are buttons with accessible name "Xóa bộ lọc Nhóm máu: O−".
- Visible focus ring (2 px `#14633F` + 2 px offset) on all interactive elements.
- Contrast: body text `#172A24` on white/`#EEF1F0`; secondary text `#566A60` (5.8:1 on white, 5.1:1 on page background `#EEF1F0`). The proposed token `#62756B` passes on white (4.9:1) but drops to ≈4.3:1 on the page background where subtitles sit, so a slightly darker value is used — confirm, OQ-12; error `#8A1C2D`; primary `#14633F`.
- Status never by colour alone: active menu item has bar + bold + `aria-current`; banners have icon + heading text; "—" has a written legend.
- Font sizes: body 15–16 px, table 15 px, labels 14 px, helper ≥ 12–13 px; H1 28 px; H2 20 px.
- Tables: `<caption>`, header cells `<th scope="col">`, numbers right-aligned with tabular figures, unit in section meta.
- Touch/click targets ≥ 44 px for buttons and menu items.

---

## 10. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| OQ-01 | Priority conflict: Screen List = **Should**, Function List FR-02-02 initial priority = **Must**. Which is final (W4 effective priority)? | No | Open — Screen List / W4 owner |
| OQ-02 | NBC Operations: Screen List v2 and screen-transition §4 include Operations (decided 2026-11-06), but the Use Case Diagram has no Operations–UC-09 association and the Mid-term UC list names only "NBC Staff, Hospitals"; UI Conventions OQ-02 still lists it as unresolved. What exact data scope does Operations see (same as Dept? NBC only?) and is any filter restricted? | **Yes** (for Operations variant) | Open — update UC Diagram + UC-09 text |
| OQ-03 | Hospital/kho filter semantics are a design assumption: options "Tất cả", "Kho NBC", each hospital; filters apply on "Áp dụng bộ lọc", not instantly. Confirm. | No | Assumption |
| OQ-04 | Should Hospital Staff have any scope filter (e.g. "Bệnh viện của tôi / NBC")? Designed with blood-type filter only. | No | Assumption |
| OQ-05 | Data shown per blood type is quantity only. Is a component/product split (chế phẩm), reserved vs available, or per-location breakdown required? How is "NBC available distribution stock" (Hospital view) defined vs "NBC inventory" (Dept view)? | **Yes** (BE query) | Open — W1 FR-02-02 owner |
| OQ-06 | Below-threshold alert: Screen List says "in-app badge and banner on S-F02-02" (D-16, D-20); FR-02-04 FE says "notification centre and alert detail"; UI Conventions OQ-03 says channel unconfirmed. Should S-F02-02 show an alert banner / per-blood-type "Tồn kho dưới ngưỡng" indicator? Not drawn. | No | Open — UI impact: one extra banner row above filters + optional badge on tiles |
| OQ-07 | NBC sync failure: FR-02-05 keeps the previous snapshot and logs the failure; no user-facing behaviour defined. Should the dashboard warn when the snapshot is older than X hours / when the last sync failed? What if no snapshot exists yet? Designed: show timestamp as-is; "—" + "Chưa có lần đồng bộ" if none. | No | Assumption — UI impact: warning banner variant |
| OQ-08 | Partial failure (NBC snapshot readable but hospital data fails, or vice versa): one banner for the whole page or per section? Designed: whole-page error. | No | Assumption |
| OQ-09 | Maximum loading time / timeout before showing the Error state is not specified anywhere. | No | **TBD** |
| OQ-10 | Access-denied UI (SD UC-09) not drawn as a frame; uses UI Conventions message. Confirm whether a dedicated frame is required. | No | Open |
| OQ-11 | Team-raised items checked against Screen List, UC-09, FR-02-02, SD UC-09: **Ghi chú** — no source; **Nhiệt độ bảo quản** — not a UC-09 field; **Báo sự cố** — no use case/workflow/actor; **Tải biên bản** — no report/export function in W1; **Tạo yêu cầu mới** — belongs to S-F01-01 (UC-01, Hospital), not an inventory function. None implemented. | No | Not implemented — sources to update if wanted: W0 UC list, W1 Function List, Screen List |
| OQ-12 | Colour tokens are "Đề xuất" (UI Conventions §6, OQ-08). `text-secondary` used as `#566A60` instead of proposed `#62756B`, which is ≈4.3:1 on the `#EEF1F0` page background (below 4.5:1). Confirm palette. | No | Assumption |
| OQ-13 | "Tổng" (NBC total, row total) is a derived on-screen sum, not stated in FR-02-02. Keep or remove? | No | Assumption |
| OQ-14 | Data field names (blood type, quantity, sync timestamp, hospital id/name) are not defined in W0/W1. | No (for wireframe) | **TBD** — data model |
| OQ-15 | No drill-down from a tile/row to unit-level detail exists in the Screen List. Confirm none is needed. | No | Open |
| OQ-16 | Ordering of hospitals in the table (alphabetical? by total?) and paging when many hospitals. | No | **TBD** |
| OQ-17 | Reference files `screen-spec-s-f01-01.md`, the NBSP login screenshot and the "Wireframe in five states" image were not supplied in this batch. Structure follows prompt §7; visual style follows UI Conventions §6 tokens. Re-check against them when available. | No | Open |

---

## Completion checklist

- [x] All five mockups exist (`img/s-f02-02-{default,empty,loading,error,success}.png`) + Hospital Staff role variant.
- [x] All five states documented (§5).
- [x] Every visible UI element is in the element inventory (E01–E38).
- [x] Every data element has a source (logical source given; field names TBD — OQ-14).
- [x] Every interaction has a defined response (I01–I11).
- [x] Every navigation target exists in the Screen List (S-F01-01, S-F01-02, S-F01-03, S-F02-01, S-F02-03, S-F03-01, S-F03-03, S-F03-05, S-F04-01).
- [x] FR-02-02 supported: role scope, blood-type display, sync timestamp, hospital/blood-type filters.
- [x] Role permissions respected: Hospital Staff sees no other hospital's data; Dept-only actions hidden for other roles.
- [x] Open questions recorded (OQ-01 – OQ-17). Blocking: OQ-02, OQ-05.
- [x] Figma frames created and named per convention.
