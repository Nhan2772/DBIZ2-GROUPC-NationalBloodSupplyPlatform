# Quy ước giao diện người dùng — NBSP

> **DBIZ2 — Nhóm C · Bản đề xuất v2.0 · 17/11/2026**  
> **Phạm vi:** Toàn bộ National Blood Supply Chain Platform (NBSP), gồm Bệnh viện, Bộ phận Lưu trữ & Phân phối máu NBC, Chuyên viên phân tích dự báo NBC và Bộ phận Vận hành/Dự báo NBC.  
> **Thiết bị mục tiêu:** **Máy tính (desktop) — khung thiết kế chuẩn 1440 × 1024 px (rộng × cao)**.  
> **Ngôn ngữ giao diện:** **Tiếng Việt**; giữ nguyên mã nghiệp vụ, tên hệ thống và thuật ngữ kỹ thuật cần thiết.  
> **Trạng thái tài liệu:** Quy ước dùng chung để thiết kế wireframe và triển khai; các mục ghi **Đề xuất** hoặc **Cần chốt** chưa phải yêu cầu chức năng mới.

## 0. Phạm vi, nguồn tham chiếu và nguyên tắc chung

Quy ước áp dụng cho **13 màn hình** trong `screen-list.md`, các luồng của **4 nhóm người dùng** trong `usage-flow-total.md` và những trạng thái giao diện xuất hiện trong các use case liên quan. Không tự tạo màn hình mới chỉ vì xuất hiện thông báo, trạng thái, cửa sổ xác nhận hoặc một tác vụ nền.

| Nhóm người dùng | Phạm vi thao tác theo Usage Flow | Màn hình tiêu biểu |
|---|---|---|
| **Nhân viên bệnh viện** (*Hospital Staff*) | Đăng nhập; yêu cầu cấp máu; xem lô hàng/xác nhận đã nhận; ghi nhận sử dụng máu; nhập nhu cầu theo kế hoạch; phản hồi xác minh dữ liệu; xem tồn kho theo quyền | `S-F04-01`, `S-F01-01`, `S-F01-02`, `S-F02-01`, `S-F02-02`, `S-F03-01`, `S-F03-03` |
| **Bộ phận Lưu trữ & Phân phối máu NBC** (*NBC Blood Storage & Distribution*) | Xem hàng đợi yêu cầu; kiểm tra kho; phê duyệt/điều phối; theo dõi máu gần hết hạn; nhận cảnh báo dưới ngưỡng | `S-F01-03`, `S-F01-04`, `S-F02-02`, `S-F02-03` |
| **Chuyên viên phân tích dự báo NBC** (*NBC Forecasting Analyst*) | Kiểm tra dữ liệu nhu cầu và bất thường; ghi quyết định xác thực; xem/duyệt bản dự báo nháp | `S-F03-02`, `S-F03-04` |
| **Bộ phận Vận hành NBC** (*NBC Operations*) | Xem dự báo đã duyệt; kiểm tra vị thế tồn kho và tiếp nhận cảnh báo dưới ngưỡng | `S-F03-05`, `S-F02-02` (theo Usage Flow; **cần cập nhật Screen List**) |

**Quy tắc xuyên suốt:**

1. **Một Screen ID là một màn hình**, có thể chứa nhiều tab, bảng, modal hoặc trạng thái. Không biến các trạng thái như “Gửi thành công”, “Thiếu máu”, “Đang vận chuyển” thành màn hình riêng nếu Screen List không có.
2. **Quyền truy cập quyết định dữ liệu và thao tác được thấy.** Không hiển thị dữ liệu của đơn vị khác ngoài phạm vi được phân quyền; menu và nội dung phải tương ứng actor đã đăng nhập (FR-04-03).
3. **Phân biệt trạng thái nghiệp vụ, trạng thái hiển thị và phản hồi thao tác.** Ví dụ: “Không thể điều phối do thiếu tồn kho” là phản hồi lỗi, **không** mặc nhiên là trạng thái cuối mới của đơn.
4. **Không tự suy diễn yêu cầu nghiệp vụ.** Loại mức độ khẩn, mã định danh, thời hạn gần hết hạn, cơ chế giao thông báo và dữ liệu trường chưa được W0/W1/Screen Spec xác định cần được cấu hình hoặc đưa vào danh sách câu hỏi cần chốt.
5. **Giữ mã tham chiếu bằng tiếng Anh** (`UC-xx`, `FR-xx-xx`, `S-Fxx-xx`, `Pending`, `Dispatched`, `Received`) để thống nhất traceability và code; chỉ dịch **nhãn hiển thị** cho người dùng.

---

## 1. Ngôn ngữ và cách diễn đạt

### 1.1. Quy định chung

- **100% nhãn, mô tả, thông báo, nút bấm, tiêu đề và placeholder hướng tới người dùng bằng tiếng Việt.** Những từ viết tắt chuyên ngành phải được giải thích ở lần dùng đầu nếu người dùng mục tiêu chưa quen; tên/mã hệ thống giữ nguyên.
- Văn phong **rõ ràng, ngắn gọn, trung tính, có trách nhiệm**, phù hợp quy trình quản lý và cấp phát máu. Tránh từ ngữ gây hiểu lầm rằng hệ thống đã hoàn tất một nghiệp vụ khi máy chủ chưa xác nhận.
- **Nút tác vụ = động từ + đối tượng/kết quả**: “Gửi yêu cầu cấp máu”, “Xem chi tiết lô hàng”, “Xác nhận đã nhận máu”, “Lưu dữ liệu sử dụng”, “Yêu cầu bệnh viện xác minh”, “Duyệt dự báo”. Tránh dùng một mình “OK”, “Submit”, “Process”, “Done”.
- Cách xưng hô trong thông báo: hạn chế “bạn”; ưu tiên nêu đối tượng và hành động. Ví dụ: “Yêu cầu chưa được tạo. Vui lòng kiểm tra các trường được đánh dấu.”
- Nhất quán thuật ngữ **“yêu cầu cấp máu”**, **“lô hàng”**, **“đơn vị máu”**, **“tồn kho”**, **“dữ liệu nhu cầu”**, **“bản dự báo”**, **“xác minh dữ liệu”**; không tùy tiện thay thế “phê duyệt” bằng “đã giao”.

### 1.2. Từ điển thuật ngữ trạng thái

| Mã/trạng thái dùng trong hệ thống | Nhãn hiển thị đề xuất | Ý nghĩa và ràng buộc |
|---|---|---|
| `Pending` | **Đang chờ xử lý** | Yêu cầu đang chờ NBC xem xét/điều phối; không khẳng định lô hàng đã được xuất. |
| `Dispatched` | **Đã xuất kho / đang vận chuyển** | NBC đã ghi nhận điều phối. Trên màn hình có thể hiển thị **“Đang vận chuyển”** như *nhãn diễn giải*, **không tạo một status DB mới**. Không ghi “Đã giao tới bệnh viện” nếu chưa có bằng chứng giao đến. |
| `Received` | **Đã xác nhận nhận máu** | Chỉ sau khi bệnh viện xác nhận thực tế và hệ thống ghi nhận thành công; không tự chuyển sau khi dispatch. |
| `Pending Verification` | **Đang chờ xác minh** | Dữ liệu bất thường được gửi yêu cầu xác minh tới bệnh viện (UC-13/UC-14). |
| `Accepted` / `Excluded` | **Đã chấp nhận** / **Đã loại khỏi dự báo** | Kết quả quyết định với *bản ghi dữ liệu nhu cầu*; không phải trạng thái lô hàng. |
| Draft / Approved forecast | **Dự báo nháp** / **Dự báo đã duyệt** | Phân biệt bản đang xét duyệt với bản NBC Operations có thể đọc. |
| Cảnh báo dưới ngưỡng | **Tồn kho dưới ngưỡng** | Thông báo từ UC-11; không mở một màn hình chuyên biệt nếu chưa được xác định trong Screen List. |

**Chú ý riêng cho Receipt Confirmation:** Trạng thái giao diện “Sẵn sàng xác nhận” chỉ áp dụng khi shipment ở trạng thái phù hợp để xác nhận theo FR-01-07 và người dùng **đã nhận/đối chiếu lô hàng ngoài thực tế**. Nếu mới `Dispatched` nhưng vẫn đang vận chuyển, hiển thị hướng dẫn “Chỉ xác nhận sau khi nhận và kiểm tra lô hàng”; không suy luận trạng thái giao thành công từ ETA.

### 1.3. Ví dụ nhãn chuẩn

| Chức năng | Nhãn tiếng Việt nên dùng |
|---|---|
| Xác thực | Đăng nhập; Đăng xuất; Tài khoản; Mật khẩu |
| Yêu cầu & điều phối | Yêu cầu cấp máu; Hàng đợi yêu cầu; Xem yêu cầu; Phê duyệt và điều phối; Nhập lý do không phê duyệt |
| Giao nhận | Chi tiết lô hàng; Hành trình lô hàng; Dự kiến đến (ETA); Xác nhận đã nhận máu |
| Kho máu | Tổng quan tồn kho; Nhóm máu; Số lượng (đơn vị); Đơn vị máu gần hết hạn; Đồng bộ gần nhất |
| Dữ liệu lâm sàng | Ghi nhận sử dụng máu; Nhu cầu theo kế hoạch; Dữ liệu bất thường; Chấp nhận; Loại khỏi dự báo; Yêu cầu xác minh |
| Dự báo | Dự báo nháp; Xem xét dự báo; Duyệt dự báo; Từ chối dự báo; Dự báo đã được duyệt |
| Tác vụ chung | Tìm kiếm; Lọc; Xóa bộ lọc; Làm mới dữ liệu; Quay lại; Hủy; Lưu thay đổi; Xem chi tiết |

---

## 2. Kích thước, bố cục và định dạng dữ liệu

### 2.1. Khung desktop chuẩn cho toàn dự án

| Thuộc tính | Quy ước |
|---|---|
| **Kích thước frame Figma** | **1440 × 1024 px**: *width = 1440*, *height = 1024* |
| Thiết bị mục tiêu | Trình duyệt trên máy tính; không thiết kế mobile 360 px trong phạm vi buổi thực hành này |
| Canvas | Mỗi màn hình là một frame riêng; các UI state cũng có thể là frame riêng để review, **nhưng giữ nguyên Screen ID** trong tên |
| Lề bao ngoài (đề xuất) | 16 px |
| Sidebar chung (đề xuất) | Rộng 240 px; menu tương ứng quyền người dùng |
| Khoảng cách sidebar–nội dung (đề xuất) | 16 px |
| Vùng chính (tính theo khung đề xuất) | 1152 px = 1440 − 16 (lề trái) − 240 (sidebar) − 16 (khoảng cách) − 16 (lề phải) |
| Thanh đầu trang (đề xuất) | Cao 64–72 px; tiêu đề/tìm kiếm và thông tin người dùng |
| Padding nội dung (đề xuất) | 24 px; khoảng cách giữa card 16–24 px |
| Lưới nội dung (đề xuất) | 12 cột trong vùng chính; gutter 16 px; bố cục bảng/card tùy tác vụ |
| Cuộn | Cho phép cuộn **theo chiều dọc** nếu nội dung vượt 1024 px; không giảm font đến mức khó đọc để ép vừa frame |
| Ứng xử khi thu hẹp cửa sổ | Ngoài phạm vi wireframe chuẩn; triển khai responsive cần quyết định riêng, không mặc định thiết kế mobile |

**Bố cục xuyên suốt:** sidebar trái (thương hiệu/menu theo vai trò) → topbar (tìm kiếm phù hợp màn hình, thông tin tài khoản) → breadcrumb/tiêu đề → nội dung chính (form/bảng/card) → khu vực hành động. Màn hình đăng nhập `S-F04-01` **không bắt buộc dùng sidebar** nhưng phải dùng cùng font, màu, trường nhập và quy ước thông báo.

### 2.2. Quy ước hiển thị dữ liệu

| Loại thông tin | Quy ước | Ví dụ hiển thị |
|---|---|---|
| Ngày, hạn dùng | `dd/MM/yyyy` | `17/11/2026` |
| Giờ | `HH:mm` (24 giờ) | `14:30` |
| Ngày giờ, ETA | `dd/MM/yyyy HH:mm`, giờ Việt Nam `UTC+07:00` khi cần làm rõ | `18/11/2026 09:30` |
| Nhóm máu | ABO + Rh, giữ nguyên ký hiệu y khoa | `O+`, `A−`, `AB+` |
| Số lượng | Số nguyên không âm khi **hiển thị**; trường nhập số lượng theo ràng buộc nghiệp vụ, có nhãn `đơn vị` | `4 đơn vị` |
| Chế phẩm máu | Tên chuyên môn theo danh mục được duyệt, không tự phát minh danh sách | `Hồng cầu lắng` (minh họa) |
| Mã yêu cầu/lô/đơn vị | Hiển thị **nguyên mã NBSP đã cấp**, cho phép sao chép khi cần; không đặt quy tắc sinh mã trong UI Convention | `[Mã yêu cầu]`, `[Mã lô]`, `[Mã đơn vị]` |
| Tính khẩn cấp | Danh sách giá trị lấy từ hệ thống/Use Case đã duyệt | `Mức độ khẩn: [Giá trị]` |
| Tồn kho và tổng hợp | Có tiêu đề phạm vi bệnh viện/NBC, số liệu và thời điểm đồng bộ gần nhất nếu đã được lưu | `Cập nhật lần cuối: [Ngày giờ]` |
| Máu gần hết hạn | Hiển thị ngày hết hạn và số ngày còn lại; **không chốt số ngày cảnh báo** khi yêu cầu chưa xác định | `Còn [N] ngày` |
| Dữ liệu nhu cầu/dự báo | Ghi rõ bệnh viện, khoảng thời gian, nhóm máu và đơn vị của từng chỉ số; tránh viết số không có bối cảnh | `Kỳ dự báo: [Khoảng thời gian]` |
| Trường chưa có dữ liệu | `—` hoặc câu trạng thái trống, **không hiển thị số 0 nếu chưa có dữ liệu thực tế** | `Chưa có dữ liệu` |
| Tiền tệ | Không mặc định xuất hiện trong các màn hình Screen List hiện tại; chỉ thiết kế nếu có yêu cầu mới được duyệt | Không áp dụng mặc định |

**Trường y tế trọng yếu:** Mã đơn vị, nhóm máu, chế phẩm, hạn dùng, số lượng, trạng thái và lịch sử ghi nhận cần ưu tiên độ rõ ràng; không chỉ dựa vào màu để phân biệt.

---

## 3. Thành phần giao diện dùng chung

| Thành phần | Quy ước áp dụng toàn dự án |
|---|---|
| **Sidebar/menu** | Một cấu trúc nhận diện chung; mục được phép hiển thị theo vai trò. Trạng thái đang chọn có nền + viền/dấu chỉ thị + chữ, không chỉ đổi màu. |
| **Topbar** | Nhất quán tên hệ thống/người dùng; chỉ đưa ô tìm kiếm nếu có dữ liệu có thể tìm. Không dùng global search giả khi chưa có chức năng. |
| **Breadcrumb/tiêu đề** | Dùng tên nghiệp vụ tiếng Việt; breadcrumb trỏ tới trang có thực trong Screen List. |
| **Primary button** | Tối đa một hành động chính trong **mỗi vùng tác vụ**; nhãn nêu rõ kết quả. Chiều cao đề xuất 44–48 px. Khi gửi dữ liệu, có loading/disabled chống nhấn lặp. |
| **Secondary/destructive button** | Quay lại, hủy, làm mới là secondary. Hành động có hệ quả nghiệp vụ đáng kể (loại dữ liệu, không phê duyệt, từ chối dự báo) phải dùng nhãn rõ và xác nhận phù hợp. |
| **Input/select/date/textarea** | Có nhãn hiện hữu kể cả khi đã nhập giá trị; nêu bắt buộc khi nghiệp vụ yêu cầu; có helper text và lỗi ngay dưới trường. Không dùng placeholder thay label. |
| **Bảng dữ liệu** | Header rõ nghĩa, căn số sang phải khi hợp lý, có trạng thái trống/loading/lỗi, phân trang hoặc cuộn khi dữ liệu lớn (tùy spec). Mỗi hàng giữ được mã định danh. |
| **Bộ lọc** | Theo quyền và nguồn dữ liệu: ví dụ bệnh viện, khoảng ngày, nhóm máu, trạng thái. Chỉ hiển thị bộ lọc thực sự có trong FR hoặc Screen Spec. Có chỉ thị bộ lọc đang áp dụng. |
| **Badge/chip trạng thái** | Luôn có **text** và có thể kèm icon; màu chỉ hỗ trợ. Phân biệt status DB với nhãn diễn giải giao diện. |
| **Progress/timeline** | Phù hợp lô hàng và chu kỳ dự báo nếu có bước nghiệp vụ xác nhận; không tự đánh dấu “Đã giao tới bệnh viện” chỉ từ `Dispatched`. |
| **Modal xác nhận** | Chứa việc sẽ làm, đối tượng/mã liên quan, hệ quả chính và nút “Hủy” + hành động đầy đủ; không tạo Screen ID mới. Modal **không phê duyệt đơn** đã nêu tại `S-F01-04` là modal đã có trong Screen List. |
| **Notification/banner/toast** | Thông báo cho biết việc gì xảy ra và bước tiếp theo. Toast không thay thế lỗi trường nhập; lỗi cản nghiệp vụ dùng banner/inline. |
| **Trạng thái disabled** | Chỉ khóa hành động khi có căn cứ (chưa đủ dữ liệu, không có quyền, đang gửi, chưa đạt điều kiện nghiệp vụ) và giải thích rõ lý do. |
| **Thông tin đồng bộ** | Tồn kho dựa vào nguồn ngoài có thể hiển thị “Đồng bộ gần nhất” khi dữ liệu tồn tại; không gắn nhãn “thời gian thực” nếu chỉ là snapshot (FR-02-05/UC-17). |
| **Xác nhận nghiệp vụ từ server** | Chỉ hiển thị “thành công” khi hệ thống xác nhận; nếu mạng gián đoạn, không tự coi thao tác đã hoàn tất. |

### 3.1. Năm trạng thái dùng để review wireframe

Đối với **mỗi màn hình có rủi ro tương ứng**, nhóm nên kiểm tra các trạng thái dưới đây. Đây là **UI states**, không tự động tạo use case, Screen ID hoặc trạng thái dữ liệu:

| State | Cách thể hiện | Minh họa |
|---|---|---|
| **1. Default / Có dữ liệu** | Thông tin thực tế đầy đủ; nút và bộ lọc phù hợp quyền | Danh sách lô hàng, bảng tồn kho, danh sách dữ liệu nhu cầu |
| **2. Empty / Chưa có dữ liệu** | Không có bản ghi trong phạm vi đang xem; nêu bước tiếp theo khả dụng | “Chưa có lô hàng cần xác nhận”; “Không có bản ghi phù hợp bộ lọc” |
| **3. Loading / Đang tải** | Skeleton/spinner ở vùng dữ liệu; vô hiệu hóa hành động chưa đủ dữ liệu | Đang tải chi tiết lô hàng; đang truy xuất forecast |
| **4. Business in progress / Chưa đủ điều kiện**, hoặc **Error / Lỗi** tùy màn hình | Với *Receipt Confirmation*, dùng “**Đang vận chuyển**”, không coi đây là lỗi. Với màn hình nhập liệu/điều phối, thể hiện lỗi validation/thiếu tồn kho bằng lý do cụ thể. | “Lô hàng đang vận chuyển. Chỉ xác nhận sau khi nhận và kiểm tra lô.” / “Không đủ đơn vị máu hợp lệ để điều phối.” |
| **5. Success / Hoàn tất** | Xác nhận từ server + kết quả nghiệp vụ + đường đi tiếp theo | “Đã ghi nhận sử dụng máu”; “Đã xác nhận nhận máu”; “Đã duyệt dự báo” |

**Phân biệt:** Lỗi kỹ thuật/validation vẫn phải được thiết kế khi có nhánh lỗi, dù state 4 của wireframe Receipt Confirmation được dành cho “Đang vận chuyển”. Các trạng thái UI không làm thay đổi chính sách lưu dữ liệu trong W1.

### 3.2. Quy ước trên từng màn hình trong Screen List

| Screen ID — tên UI tiếng Việt | Actor | Thành phần/đặc điểm cần có | Trace |
|---|---|---|---|
| `S-F04-01` — **Đăng nhập** | Cả 4 nhóm | Tài khoản/mật khẩu; lỗi xác thực; chuyển đến đúng phạm vi sau đăng nhập | UC-06, UC-12 · FR-04-01, FR-04-02 |
| `S-F01-01` — **Yêu cầu cấp máu** | Bệnh viện | Nhóm máu, số lượng, mức độ khẩn; kiểm tra theo trường và giữ dữ liệu đã nhập khi lỗi | UC-01 · FR-01-01, FR-01-06 |
| `S-F01-02` — **Chi tiết lô hàng & xác nhận nhận máu** | Bệnh viện | Trạng thái, các đơn vị máu, hạn dùng, ETA; xác nhận sau khi kiểm tra thực tế; ngăn xác nhận sớm/trùng | UC-02/03 · FR-01-04, FR-01-05, FR-01-07 |
| `S-F01-03` — **Hàng đợi yêu cầu** | Bộ phận phân phối NBC | Danh sách yêu cầu đang chờ, thông tin ưu tiên, liên kết mở yêu cầu | UC-07 · FR-01-02 |
| `S-F01-04` — **Xem xét & điều phối đơn máu** | Bộ phận phân phối NBC | Thông tin đơn, khả dụng tồn kho, chọn đơn vị/ETA theo spec; nhánh không đủ tồn và modal ghi lý do không phê duyệt **nếu được chốt trong release** | UC-08 · FR-01-03, FR-01-08, FR-01-09 |
| `S-F02-01` — **Ghi nhận sử dụng máu** | Bệnh viện | Nhập số lượng đã sử dụng/loại bỏ và lý do; không thay đổi tồn kho nếu dữ liệu sai/quá mức | UC-04 · FR-02-01, FR-02-06 |
| `S-F02-02` — **Tổng quan tồn kho** | Bệnh viện, Bộ phận phân phối NBC; Operations **cần đồng bộ Screen List** | Chỉ dữ liệu trong phạm vi được phép; danh sách/tổng hợp, đồng bộ gần nhất nếu có | UC-09 · FR-02-02 |
| `S-F02-03` — **Đơn vị máu gần hết hạn** | Bộ phận phân phối NBC | Hạn dùng, số ngày còn lại và danh sách theo tiêu chí đã cấu hình | UC-10 · FR-02-03 |
| `S-F03-01` — **Ghi nhận nhu cầu máu theo kế hoạch** | Bệnh viện | Nhu cầu gắn phẫu thuật/truyền máu định kỳ; nhãn thời kỳ và số lượng | UC-05 · FR-03-01 |
| `S-F03-02` — **Rà soát dữ liệu nhu cầu** | Chuyên viên phân tích | Lọc bệnh viện/khoảng thời gian/nhóm máu; cờ bất thường + lý do; Chấp nhận/Loại/Yêu cầu xác minh | UC-13 · FR-03-02, FR-03-03 |
| `S-F03-03` — **Phản hồi yêu cầu xác minh** | Bệnh viện | Xem bản ghi bị gắn cờ và ghi chú; xác nhận/sửa/bổ sung theo UC-14; giữ dấu vết thay đổi | UC-14 · FR-03-04 |
| `S-F03-04` — **Xem xét & duyệt dự báo** | Chuyên viên phân tích | Bản nháp, các chỉ số liên quan, nút Duyệt/Từ chối theo spec, phản hồi quyết định | UC-15 · FR-03-06 |
| `S-F03-05` — **Dự báo đã duyệt** | NBC Operations | Chỉ bản dự báo được duyệt, có kỳ dự báo và phạm vi dữ liệu; quyền đọc theo vai trò | UC-16 · FR-03-07 |

**Không thêm màn hình** cho `UC-11` (cảnh báo tồn kho dưới ngưỡng), `UC-17` (đồng bộ tự động), `FR-03-05` (tạo dự báo nháp tự động), `FR-03-08` (đóng kỳ validation), `FR-04-03` (phân quyền) hay `FR-04-04` (audit). Thể hiện phản hồi phù hợp trên màn hình liên quan hoặc dưới dạng tiến trình nền.

---

## 4. Quy chuẩn thông báo

### 4.1. Cấu trúc viết thông báo

- **Thành công:** `[Việc đã hoàn tất] + [Kết quả] + [Hành động tiếp theo (nếu có)]`.
- **Lỗi dữ liệu:** `[Điều chưa hợp lệ] + [Trường/bản ghi liên quan] + [Cách sửa]`; giữ dữ liệu đã nhập nếu yêu cầu nghiệp vụ cho phép.
- **Chưa đủ điều kiện:** `[Trạng thái hiện tại] + [Vì sao chưa làm được] + [Cần làm gì tiếp]`.
- **Không rõ kết quả do kết nối:** không khẳng định thành công/thất bại khi chưa nhận phản hồi máy chủ; yêu cầu kiểm tra lại trạng thái trước khi thao tác lần nữa.
- **Cảnh báo:** cung cấp ngữ cảnh (loại máu/vùng tồn kho/thời điểm) và đường đi đến thông tin liên quan khi đã có màn hình đích hợp lệ.

### 4.2. Thư viện thông báo tiếng Việt đề xuất

Các mẫu dưới đây là **UI copy**, không tự bổ sung hành vi/nhánh nghiệp vụ ngoài các FR và Usage Flow hiện hành.

| Ngữ cảnh | Nội dung hiển thị đề xuất | Đặt tại |
|---|---|---|
| Đăng nhập thất bại | **“Không thể đăng nhập. Vui lòng kiểm tra thông tin tài khoản và thử lại.”** | `S-F04-01`, cạnh form/banner |
| Không đủ quyền | **“Tài khoản không có quyền truy cập nội dung này.”** | Màn hình được yêu cầu, không lộ dữ liệu cấm |
| Gửi yêu cầu máu thành công | **“Đã gửi yêu cầu cấp máu. Yêu cầu đang chờ NBC xử lý.”** | `S-F01-01`, success |
| Yêu cầu không hợp lệ | **“Chưa tạo yêu cầu. Vui lòng sửa các trường được đánh dấu và gửi lại.”** | `S-F01-01`, inline + banner |
| Chưa có shipment | **“Chưa có thông tin lô hàng được điều phối cho yêu cầu này. Vui lòng kiểm tra lại sau.”** | `S-F01-02`, empty |
| Lô đang vận chuyển | **“Lô hàng đang vận chuyển. Chỉ xác nhận sau khi bệnh viện đã nhận và kiểm tra lô.”** | `S-F01-02`, banner trạng thái |
| Xác nhận quá sớm | **“Chưa thể xác nhận nhận máu vì lô hàng chưa ở trạng thái đã xuất kho/điều phối.”** | `S-F01-02`, lỗi FR-01-07 |
| Xác nhận trùng | **“Lô hàng này đã được xác nhận nhận máu. Không ghi tăng tồn kho lần nữa.”** | `S-F01-02`, lỗi FR-01-07 |
| Xác nhận nhận máu thành công | **“Đã xác nhận nhận máu. Các đơn vị máu được ghi nhận vào tồn kho bệnh viện.”** | `S-F01-02`, success |
| Thiếu tồn kho hợp lệ | **“Không đủ đơn vị máu hợp lệ để điều phối. Đơn chưa được chuyển sang Đã xuất kho.”** | `S-F01-04`, lỗi FR-01-08 |
| Không phê duyệt đơn (nếu được duyệt trong phạm vi release) | **“Đã ghi nhận quyết định không phê duyệt cùng lý do.”** | `S-F01-04`, success / modal |
| Ghi sử dụng máu không hợp lệ | **“Chưa lưu dữ liệu sử dụng. Kiểm tra các trường bắt buộc và số lượng còn trong kho.”** | `S-F02-01`, inline + banner |
| Lưu sử dụng máu thành công | **“Đã ghi nhận lượng máu sử dụng và cập nhật tồn kho bệnh viện.”** | `S-F02-01`, success |
| Không có đơn vị gần hết hạn | **“Không tìm thấy đơn vị máu gần hết hạn theo phạm vi đang chọn.”** | `S-F02-03`, empty |
| Tồn kho dưới ngưỡng | **“Tồn kho nhóm máu [Nhóm máu] đang thấp hơn ngưỡng quy định. Xem tổng quan tồn kho.”** | UC-11, kênh cần xác nhận |
| Dữ liệu tồn kho có thể đã cũ | **“Dữ liệu tồn kho được đồng bộ gần nhất lúc [Ngày giờ]. Vui lòng kiểm tra trước khi quyết định.”** | `S-F02-02`, khi có timestamp |
| Dữ liệu nhu cầu bất thường | **“Bản ghi này có dấu hiệu bất thường: [Lý do]. Vui lòng chọn hướng xử lý.”** | `S-F03-02`, cạnh bản ghi |
| Yêu cầu xác minh đã ghi nhận | **“Đã ghi nhận yêu cầu xác minh. Bản ghi đang chờ phản hồi từ bệnh viện.”** | `S-F03-02`, success |
| Bệnh viện đã gửi phản hồi | **“Đã gửi thông tin xác minh. Hệ thống đã ghi nhận phản hồi để chuyên viên xem xét.”** | `S-F03-03`, success |
| Bản ghi xác minh quá hạn | **“Yêu cầu xác minh đã quá hạn. Vui lòng xem lại bản ghi và quyết định hướng xử lý theo quy định.”** | `S-F03-02`, warning |
| Dự báo được duyệt | **“Đã duyệt bản dự báo. Bản được duyệt sẵn sàng cho bộ phận vận hành xem.”** | `S-F03-04`, success |
| Dự báo bị từ chối | **“Đã ghi nhận quyết định từ chối bản dự báo. Bản này không được công bố.”** | `S-F03-04`, status/success thao tác |
| Không tìm thấy dữ liệu | **“Không có dữ liệu phù hợp với bộ lọc hiện tại. Hãy thay đổi bộ lọc hoặc thử lại.”** | Các bảng/danh sách, empty |
| Không rõ kết quả do mất kết nối | **“Chưa xác định được kết quả thao tác. Hãy tải lại trạng thái hiện tại trước khi thử lại.”** | Các màn hình ghi dữ liệu, banner |

**Nguyên tắc thông báo kết quả:** Không gọi một thao tác là “thành công” ngay khi người dùng nhấn nút. Phải chờ phản hồi từ hệ thống hoặc trạng thái đã được xác minh.

---

## 5. Khả năng truy cập và tính dễ sử dụng

- **Độ tương phản:** chữ thường tối thiểu **4,5:1**; icon/đường viền thành phần cần phân biệt tối thiểu **3:1** trên nền liên quan. Dùng cặp màu thực tế đã kiểm tra; không chỉ dựa vào màu xanh/đỏ để diễn giải.
- **Vùng nhấn:** tối thiểu **24 × 24 CSS px**; nên sử dụng **40–44 px** cho nút và mục tiêu tương tác chính trên desktop để thao tác chuột thuận lợi. Chiều cao nút chính gợi ý **44–48 px**.
- **Bàn phím:** thứ tự Tab theo luồng đọc; focus dễ nhìn; Enter/Space kích hoạt đúng control; Escape đóng modal khi phù hợp và không làm mất dữ liệu ngoài ý muốn.
- **Biểu mẫu:** nhãn độc lập với placeholder, chỉ rõ bắt buộc và lỗi; kết nối mô tả lỗi với trường tương ứng; có thể sửa lại mà không mất dữ liệu đã nhập (khi FR yêu cầu).
- **Thông báo động:** cập nhật loading, thành công hoặc lỗi theo cách công nghệ hỗ trợ có thể đọc được; không phụ thuộc duy nhất vào toast tự biến mất.
- **Bảng dữ liệu:** tiêu đề cột rõ ràng, đơn vị gắn với giá trị, không cắt mất mã đơn vị/hạn dùng/trạng thái; có tiêu đề bảng, thông tin lọc và thông điệp trạng thái trống.
- **Hình thức:** font có hỗ trợ tiếng Việt; không ép chữ quá nhỏ khi dữ liệu dài; giữ cỡ chữ thân bài khoảng **14–16 px**. Không dùng icon đơn lẻ cho nghiệp vụ rủi ro cao (xuất kho, xác nhận nhận máu, phê duyệt forecast).
- **Màn hình chuẩn:** kiểm tra tại **1440 × 1024 px** và xem nội dung trên vùng thực tế của frame; kiểm tra trạng thái cuộn/overflow thay vì tối giản thông tin nghiệp vụ quan trọng.

---

## 6. Bộ mã thiết kế và quy ước trực quan — đề xuất thống nhất trong Figma

Các giá trị sau chuyển sang **bảng màu xanh lá/trắng/xám** tương đồng với wireframe “HuyếtMạch” mà nhóm đã sử dụng, thay cho bộ màu xanh lam trong bản Flow 1 cũ. **Đây là token thiết kế đề xuất, cần chốt trước khi đóng băng design system.** Token không quyết định luồng nghiệp vụ.

| Token | Giá trị đề xuất | Ứng dụng |
|---|---|---|
| `color/primary` | `#14633F` | Hành động chính, menu đang chọn |
| `color/primary-dark` | `#0B5135` | Vùng nhấn mạnh/heading trên nền nhạt |
| `color/background` | `#EEF1F0` | Nền trang desktop |
| `color/surface` | `#FFFFFF` | Card, form, modal |
| `color/surface-subtle` | `#F8FAF9` | Sidebar, ô thông tin nhạt |
| `color/text-primary` | `#172A24` | Tiêu đề/nội dung chính |
| `color/text-secondary` | `#62756B` | Chú thích, nhãn phụ (cần kiểm tra tương phản với nền thực tế) |
| `color/border` | `#DCE5DE` | Đường chia, viền không mang ý nghĩa thông báo |
| `color/success-bg` | `#E5F4E9` | Nền trạng thái thành công |
| `color/warning` | `#9A3412` | Chữ/cảnh báo trên nền sáng đã kiểm tra |
| `color/error` | `#8A1C2D` | Chữ/lỗi trên nền sáng đã kiểm tra |
| `font/family` | `Inter`, dự phòng `Arial, sans-serif` | Font hỗ trợ tiếng Việt |
| `font/page-title` | `28 px`, đậm | Tên màn hình |
| `font/section-title` | `20 px`, đậm | Tên card/nhóm thông tin |
| `font/body` | `16 px`, thường | Nội dung cơ bản |
| `font/table-label` | `14 px`, vừa | Nhãn/bảng/form |
| `font/helper` | `12–13 px` | Chú thích phụ, không dùng thay nội dung cốt lõi |
| `spacing/base` | `8 px`, bổ sung bước `4 px` | Khoảng cách theo bội số |
| `radius/card` | `16–20 px` | Card/panel |
| `radius/control` | `8–12 px` | Nút, input, select |
| `height/button-primary` | `44–48 px` | Primary action |

**Quy tắc dùng bộ mã thiết kế:** dùng nhất quán trên tất cả 13 screen; màu trạng thái chỉ là trợ giúp, luôn có nhãn; không lấy ảnh mockup làm một lớp duy nhất nếu cần sửa tiếp trong Figma.

---

## 7. Kiểm tra liên kết giữa màn hình, actor và use case

- **Hospital — Flow 1:** `S-F04-01` → `S-F01-01` → *(NBC dispatches; bước nghiệp vụ của actor khác)* → `S-F01-02` (UC-02 và UC-03 cùng màn hình). Không có màn hình sign-in **riêng** cho mỗi actor.
- **Hospital — Flow 2/3/5:** từ đăng nhập chung tới `S-F02-01` / `S-F03-01` / `S-F03-03`; có thể truy cập `S-F02-02` theo quyền (UC-09).
- **NBC phân phối — Flow 1:** `S-F04-01` → `S-F01-03` → tùy nhu cầu xem tồn kho `S-F02-02` → `S-F01-04`. Việc thiếu máu không tạo trang mới; giải thích tại trang điều phối.
- **NBC phân phối — Flow 2:** `S-F04-01` → `S-F02-02` → tùy nhu cầu `S-F02-03`. Cảnh báo UC-11 là trigger/notification, không phải screen.
- **Analyst — Flow 1:** `S-F04-01` → `S-F03-02`; gửi yêu cầu xác minh cho Hospital và phản hồi tại `S-F03-03` qua UC-14. Không tự cho rằng UC-13 lập tức tạo forecast.
- **Analyst — Flow 2:** khi có dự báo nháp, `S-F04-01` (nếu chưa đăng nhập) → `S-F03-04`; quyết định duyệt/từ chối thực hiện trong màn hình này.
- **NBC Operations — Flow 1/2:** `S-F04-01` → `S-F03-05` (dự báo đã duyệt) hoặc `S-F02-02` (tồn kho theo quyền). Không tạo màn hình cấu hình threshold.

**Truy vết yêu cầu:** các tên/mã màn hình giữ theo `screen-list.md`; tuyến hành trình theo `usage-flow-total (1).md` v2.2; quy ước gốc từ `NBSP_UI_Conventions_Hospital_Flow1(2).md`. Mỗi screen spec nên trỏ tới tài liệu này thay vì sao chép và tạo phiên bản quy ước riêng.

---

## 8. Các điểm chưa thống nhất cần nhóm xác nhận

| Mã | Phát hiện sau khi đối chiếu | Tác động | Cần chốt ở đâu |
|---|---|---|---|
| **OQ-01** | `screen-list.md` cho phép modal không phê duyệt đơn (FR-01-09), nhưng `usage-flow-total.md` v2.2 ghi **release 1 không có nhánh không phê duyệt** trong UC-08. | Có/không có action và modal tại `S-F01-04`. | Chốt W0/W1, AD-UC-08 và Screen List đồng bộ; **không tự bật chức năng**. |
| **OQ-02** | `usage-flow-total.md` ghi NBC Operations cũng dùng UC-09 (`S-F02-02`), nhưng Screen List hiện chỉ liệt kê Hospital Staff và bộ phận phân phối NBC. | Quyền menu và phạm vi dữ liệu theo actor. | Bổ sung actor trong Screen List sau khi nhóm xác nhận. |
| **OQ-03** | Kênh nhận cảnh báo UC-11 được mô tả là notification/in-system nhưng Screen List ghi **cần xác nhận kênh**. | Vị trí banner/notification và khả năng deep-link. | Thống nhất phương thức thông báo và đầu mối nhận. |
| **OQ-04** | `UC-10` chưa xác định cụ thể số ngày tính là “gần hết hạn”; mức độ khẩn và một số trường có giá trị cấu hình chưa được cung cấp. | Không thể khóa bộ lọc/nhãn và ví dụ giá trị trong UI. | Chốt cùng owner nghiệp vụ/W1; không tự tạo ngưỡng. |
| **OQ-05** | Các Screen Spec tương ứng mới là đường dẫn dự kiến trong Screen List; không có nội dung chi tiết của từng spec trong bộ ba file hiện tại. | Một số trường/tác vụ ở cấp micro-interaction chưa thể xác thực. | Đối chiếu từng `screens/screen-spec-*.md` trước khi handoff lập trình. |
| **OQ-06** | Từ `Dispatched` không suy ra sự kiện “Đã giao tới bệnh viện”; xác nhận thực nhận là hành động của Hospital trong UC-03. | Wireframe Receipt Confirmation không được tự hiển thị giao thành công chỉ từ ETA. | Chốt nguồn xác minh giao tới nếu muốn bổ sung một bước tiến độ riêng. |
| **OQ-07** | `usage-flow-total.md` nêu threshold được cấu hình khi triển khai, không tạo màn hình cấu hình; Context Diagram cần sửa. | Tránh thêm màn hình hoặc quyền quản lý ngưỡng không có trong phạm vi hiện tại. | Đồng bộ Context Diagram và assumptions của W0. |
| **OQ-08** | Bộ token màu, font và một số kích thước thành phần là **đề xuất triển khai** dựa trên wireframe HuyếtMạch; chưa có bộ design system đã duyệt đính kèm. | Tính thống nhất UI sau khi toàn nhóm tích hợp. | Nhóm phê duyệt một lần trong Figma/UI Conventions rồi dùng chung. |

---

## 9. Checklist trước khi bàn giao thiết kế/đẩy lên GitHub

- [ ] UI copy tất cả 13 màn hình đều bằng tiếng Việt (trừ mã/tên riêng hoặc nhãn kỹ thuật cần giữ nguyên).
- [ ] Frame chính là **1440 px rộng × 1024 px cao**; màn hình đăng nhập có cùng tỉ lệ desktop.
- [ ] Screen ID, tên nghiệp vụ và actor khớp `screen-list.md`; các bất nhất được ghi trong Open Questions.
- [ ] Không phát sinh màn hình riêng cho success/error/modal/UC-11/UC-17 nếu chưa được duyệt.
- [ ] Form có label, quy tắc kiểm tra, lỗi cụ thể và trạng thái loading/disabled.
- [ ] Trạng thái hệ thống và trạng thái hiển thị không bị đánh đồng; không tự chuyển sang `Received`.
- [ ] Mọi hành động ghi dữ liệu chỉ báo thành công khi server xác nhận; tránh xử lý lặp khi mạng không chắc chắn.
- [ ] Tất cả màn hình dùng chung palette, typography, spacing và tên nút.
- [ ] Kiểm tra đối chiếu các nhánh quan trọng với AD/SD/FR tương ứng, đặc biệt UC-03, UC-04, UC-08, UC-13, UC-15.
- [ ] Kiểm tra tương phản, điều hướng bàn phím, focus, khả năng đọc bảng, thông báo lỗi và quyền truy cập theo actor.

### Nguồn dùng để biên soạn

1. `screen-list.md` — bản Screen List 13 màn hình, dự thảo.
2. `usage-flow-total (1).md` — Usage Flow tổng 4 actor, v2.2 ngày 07/11/2026.
3. `NBSP_UI_Conventions_Hospital_Flow1(2).md` — UI Conventions cũ, phạm vi Hospital Staff Flow 1.
4. Hình minh họa/wireframe HuyếtMạch trong quá trình workshop — chỉ tham chiếu để **đề xuất** màu sắc, bố cục, không thay thế các file W0/W1.
