# KẾ HOẠCH PHÂN CHIA NHIỆM VỤ DỰ ÁN (TEAM 4 NGƯỜI)
## MÔ HÌNH HOÁ YÊU CẦU ỨNG DỤNG THEO DÕI SỨC KHOẺ (SMARTWATCH & SMARTPHONE)
### QUY TRÌNH THỰC HIỆN TUẦN TỰ THEO GIAI ĐOẠN (WATERFALL / STAGE-GATE PIPELINE)

---

## 1. NGUYÊN TẮC QUY TRÌNH TUẦN TỰ (SEQUENTIAL PIPELINE)

Theo đúng định hướng bài giảng tại **Mục 02 của [debai.html](file:///d:/code/test/debai.html)**, các mô hình yêu cầu phần mềm có mối quan hệ phụ thuộc chặt chẽ:
```
[GĐ 1: Giả định & Use Case] ──► [GĐ 2: Đặc tả Use Case] ──► [GĐ 3: Activity & State Chart] ──► [GĐ 4: Kiểm tra chéo] ──► [GĐ 5: Đóng gói HTML]
```

### Triết lý "Làm đúng từ đầu - Có kiểm soát":
1. **Sản phẩm trước làm đầu vào (Input) cho sản phẩm sau:** Không làm nhảy cóc để tránh việc phải sửa đi sửa lại nhiều lần.
2. **Cột mốc phê duyệt (Gate Approval):** Mỗi giai đoạn đều có điểm dừng nghiệm thu. Khi toàn nhóm ký duyệt (Sign-off) đóng băng giai đoạn trước thì giai đoạn sau mới được triển khai.
3. **Chấp nhận chờ có mục đích (Constructive Waiting):** Trong thời gian chờ thành viên khác hoàn thành giai đoạn chuyên trách, thành viên chưa đến lượt sẽ đóng vai trò **Phản biện (Reviewer)** hoặc **Chuẩn bị hạ tầng tích hợp**.

---

## 2. BỘ QUY ƯỚC KỸ THUẬT CỐ ĐỊNH (SYSTEM BASELINE)

Bắt buộc tuân thủ xuyên suốt từ Giai đoạn 1 đến Giai đoạn 5 để bảo đảm tính nhất quán từng ký tự:

### 2.1. Từ điển Trạng thái vòng đời chuẩn (State Dictionary)
| Tên trạng thái chuẩn | Ý nghĩa vòng đời của Hoạt động thể chất | Xuất hiện tại |
| :--- | :--- | :--- |
| **`Khởi tạo`** | Hệ thống ghi nhận bắt đầu vận động | Nút khởi đầu State Chart |
| **`Đang theo dõi`** | Cảm biến Smartwatch đang đo đạc thông số | Luồng chính UC-01, State Chart |
| **`Đã lưu tạm`** | Kết thúc vận động, dữ liệu lưu trong bộ nhớ Smartwatch | Hậu điều kiện UC-01, Tiền điều kiện UC-02, State Chart |
| **`Đang đồng bộ`** | Đang thiết lập phiên truyền Bluetooth sang Smartphone | Luồng chính UC-02, State Chart |
| **`Đã đồng bộ`** | Smartphone đã nhận đủ và xác thực toàn vẹn dữ liệu | Hậu điều kiện thành công UC-02, State Chart |
| **`Đã giải phóng lưu tạm`** | Smartwatch dọn dẹp vùng nhớ tạm (Điểm kết thúc) | Hậu điều kiện dọn dẹp, Trạng thái cuối State Chart |

### 2.2. Danh mục 2 Use Case trọng tâm bắt buộc
* **UC 1 (Phân hệ Smartwatch):** `Theo dõi hoạt động thể chất` (Kích hoạt tự động qua cảm biến/chuyển động).
* **UC 2 (Phân hệ Smartphone):** `Đồng bộ dữ liệu hoạt động qua Bluetooth` (Kích hoạt qua sự kiện kết nối Bluetooth đã ghép đôi).

### 2.3. Quy ước tiền tố mã SVG (SVG ID Prefix)
* **Use Case Diagram (GĐ 1):** Dùng tiền tố `uc-` (ví dụ: `id="uc-arrow"`, `id="uc-marker"`).
* **Activity Diagram (GĐ 3):** Dùng tiền tố `act-` (ví dụ: `id="act-arrow"`, `id="act-decision"`).
* **State Chart Diagram (GĐ 3):** Dùng tiền tố `st-` (ví dụ: `id="st-arrow"`, `id="st-state"`).

---

## 3. CHI TIẾT 5 GIAI ĐOẠN THỰC HIỆN TUẦN TỰ

---

### GIAI ĐOẠN 1: THIẾT LẬP GIẢ ĐỊNH & KIẾN TRÚC USE CASE (NGÀY 1 - NGÀY 2)
> **Mục tiêu:** Xác lập nền tảng giả định hệ thống (Mục 03) và dựng Sơ đồ Use Case tổng thể 2 phân hệ (Mục 04).

#### 1. Người thực hiện chính:
* **Thành viên 1 (System Architect):**
  * **Task 1.1:** Soạn thảo Bảng giả định (Mục 03) gồm tối thiểu 5 giả định (`GĐ-01` $\rightarrow$ `GĐ-05`), định lượng rõ ràng:
    * `GĐ-01`: Cơ chế Bluetooth, timeout 10s, đứt kết nối giữa chừng thì giữ nguyên bản ghi tạm trên Smartwatch.
    * `GĐ-02`: Định lượng "tức thời" là chu kỳ cập nhật màn hình đúng 1 giây/lần.
    * `GĐ-03`: Tiêu chí đạt mục tiêu là $\ge$ đối với quãng đường/tốc độ; $\le$ đối với thời gian.
    * `GĐ-04`: Mất tín hiệu GPS quá 15s thì chuyển sang nội suy bằng cảm biến bước chân (gia tốc kế).
    * `GĐ-05`: Tự động nhận diện hoạt động sau 30s vận động liên tục vượt ngưỡng.
  * **Task 1.2:** Thiết kế Sơ đồ Use Case tổng thể (Mục 04) bằng mã SVG thuần:
    * 2 ranh giới hệ thống riêng biệt: `Phân hệ Smartwatch` và `Phân hệ Smartphone`.
    * Actors: `Người dùng` (Primary), `Dịch vụ bản đồ` (Secondary), `Mạng xã hội` (Secondary - chỉ nối Smartphone).
    * Quan hệ: Ít nhất 1 `<<include>>` và 1 `<<extend>>` (mũi tên đúng chiều, có điều kiện trên nhánh extend).
  * **Task 1.3:** Soạn thảo 2 bảng đi kèm Mục 04:
    * *Bảng ghi chú quan hệ:* Giải thích lý do include/extend và điều kiện kích hoạt.
    * *Bảng ghi chú phân hệ:* Giải thích lý do phân bổ Use Case đa thiết bị (Đồng bộ dữ liệu).

#### 2. Vai trò của các thành viên trong thời gian chờ:
* **Thành viên 2 & 3 (Reviewers):** Đọc kỹ bản thảo giả định và sơ đồ của TV1; phản biện các tham số đo đạc (km, phút, km/h) xem có khả thi cho phân hệ mình phụ trách không; thống nhất ký duyệt baseline.
* **Thành viên 4 (Frontend Setup):** Xây dựng Khung HTML Boilerplate nộp bài (Mục 09) gồm CSS nhúng, thanh điều hướng navbar, thanh tiến trình cuộn trang, khung rỗng các thẻ `<section id="sec-03">` đến `<section id="sec-08">`.

#### 3. Điểm dừng nghiệm thu (Gate 1 Sign-off):
* [x] Cả 4 thành viên thống nhất 100% nội dung Mục 03 và Mục 04.
* [x] Đóng băng (Freeze) Bảng giả định và Danh mục Use Case $\rightarrow$ Bàn giao sang Giai đoạn 2.

---

### GIAI ĐOẠN 2: ĐẶC TẢ CHI TIẾT 2 USE CASE QUAN TRỌNG (NGÀY 3 - NGÀY 4)
> **Mục tiêu:** Xây dựng 2 bản đặc tả hoàn chỉnh chuẩn 14 mục (Mục 05) dựa trên đầu vào đã đóng băng của Giai đoạn 1.

#### 1. Người thực hiện chính:
* **Thành viên 2 (Smartwatch Specialist):**
  * **Task 2.1:** Viết bản đặc tả 14 mục cho `UC-01: Theo dõi hoạt động thể chất` (Phân hệ Smartwatch).
  * Có 2–3 câu giải thích lý do lựa chọn.
  * Luồng chính đánh số 1, 2, 3... (1 chủ thể/bước, không chứa câu "nếu").
  * Tối thiểu 2 luồng thay thế: `3a` (mất GPS chuyển sang cảm biến gia tốc theo `GĐ-04`), `5a` (người dùng tạm dừng).
  * Tối thiểu 1 luồng ngoại lệ: `Pin yếu đột ngột $\le 2\%$` $\rightarrow$ lưu khẩn cấp và dừng theo dõi.
  * Quy tắc nghiệp vụ: `BR-01`, `BR-02` kèm đơn vị km, phút, km/h và mã `GĐ-02`, `GĐ-04`.
  * Hậu điều kiện thành công: Trạng thái `Đã lưu tạm` (khớp State Dictionary).
* **Thành viên 3 (Smartphone Specialist):**
  * **Task 3.1:** Viết bản đặc tả 14 mục cho `UC-02: Đồng bộ dữ liệu hoạt động qua Bluetooth` (Phân hệ Smartphone).
  * Có 2–3 câu giải thích lý do lựa chọn.
  * Kích hoạt qua kết nối Bluetooth đã ghép đôi giữa Smartwatch và Smartphone.
  * Luồng chính truyền nhận gói tin, kiểm tra checksum, xác nhận ACK.
  * Tối thiểu 2 luồng thay thế: `4a` (lỗi checksum truyền lại tối đa 3 lần), `5a` (bản ghi trùng lặp).
  * Tối thiểu 1 luồng ngoại lệ: `4b` (đứt kết nối Bluetooth giữa chừng $\rightarrow$ hủy giao dịch, giữ nguyên `Đã lưu tạm` trên Smartwatch theo `GĐ-01`).
  * Quy tắc nghiệp vụ: `BR-03`, `BR-04` về timeout 10s theo `GĐ-01`.
  * Hậu điều kiện: `Đã đồng bộ` và `Đã giải phóng lưu tạm`.

#### 2. Vai trò của các thành viên trong thời gian chờ:
* **Thành viên 1 (Architect Reviewer):** Rà soát xem các bước trong đặc tả của TV2 và TV3 có bám sát đề bài và khớp với các giả định `GĐ-xx` đã lập ở Giai đoạn 1 không.
* **Thành viên 4 (QA & Integration):** Nhúng mã SVG Sơ đồ Use Case và Bảng giả định của Giai đoạn 1 vào file HTML, kiểm tra hiển thị.

#### 3. Điểm dừng nghiệm thu (Gate 2 Sign-off):
* [x] Bản đặc tả `UC-01` và `UC-02` đủ 14 mục, có đầy đủ luồng thay thế (`3a`, `5a`) và luồng ngoại lệ dẫn đến thất bại.
* [x] Đóng băng (Freeze) 2 bản đặc tả $\rightarrow$ Bàn giao sang Giai đoạn 3 làm dữ liệu nguồn để vẽ sơ đồ.

---

### GIAI ĐOẠN 3: DỰNG SƠ ĐỒ ACTIVITY & STATE CHART (NGÀY 5 - NGÀY 6)
> **Mục tiêu:** Dựng Sơ đồ Hoạt động từ UC-01 (Mục 06) và Sơ đồ Trạng thái vòng đời từ UC-01 & UC-02 (Mục 07).

#### 1. Người thực hiện chính:
* **Thành viên 2 (Dựng Mục 06 từ Đặc tả UC-01 đã chốt):**
  * **Task 2.2:** Vẽ SVG Activity Diagram (tiền tố `act-`):
    * 3 làn Swimlane: `Người dùng` | `Phân hệ Smartwatch` | `Cảm biến & GPS`.
    * Khớp từng bước từ luồng chính, luồng thay thế `3a`, `5a` và luồng ngoại lệ từ Đặc tả Giai đoạn 2.
    * Khối Decision có guard đối lập đầy đủ: `[GPS ổn định]` / `[Mất tín hiệu GPS]`.
  * **Task 2.3:** Lập Bảng truy vết (Traceability Matrix) đối soát 1-1 các nhánh `3a`, `5a`, ngoại lệ sang các node Decision, Guard, Action, Merge trên sơ đồ.
* **Thành viên 3 (Dựng Mục 07 từ Vòng đời UC-01 & UC-02 đã chốt):**
  * **Task 3.2:** Vẽ SVG State Chart Diagram (tiền tố `st-`):
    * Đối tượng duy nhất: `Hoạt động thể chất`.
    * Phủ trọn vòng đời: `Khởi tạo` $\rightarrow$ `Đang theo dõi` $\rightarrow$ `Đã lưu tạm` $\rightarrow$ `Đang đồng bộ` $\rightarrow$ `Đã đồng bộ` $\rightarrow$ `Đã giải phóng lưu tạm`.
    * Bao quát ca biên: Đứt kết nối Bluetooth quay về `Đã lưu tạm`.
    * Nhãn chuẩn UML: `Sự kiện [guard] / Action`.
  * **Task 3.3:** Lập 2 bảng đi kèm:
    * *Bảng chuyển trạng thái (6 cột):* Ghi rõ nguồn sự kiện (UC kích hoạt, thiết bị, thời gian).
    * *Bảng các cặp (trạng thái, sự kiện) bị từ chối:* Tối thiểu 3 cặp bất hợp lệ, chỉ rõ hệ thống Bỏ qua hay Báo lỗi.

#### 2. Vai trò của các thành viên trong thời gian chờ:
* **Thành viên 1:** Rà soát tính logic giữa Activity Diagram (TV2) và State Chart (TV3) để đảm bảo không có mâu thuẫn về trạng thái kết thúc.
* **Thành viên 4:** Nhúng 2 bản đặc tả Mục 05 vào file HTML, định dạng bảng `<table>` chuẩn UTF-8.

#### 3. Điểm dừng nghiệm thu (Gate 3 Sign-off):
* [x] Sơ đồ Activity Diagram khớp 100% với Đặc tả UC-01.
* [x] Sơ đồ State Chart Diagram khớp 100% với các trạng thái sinh ra từ UC-01 và UC-02.
* [x] Bàn giao toàn bộ 4 mô hình sang Giai đoạn 4.

---

### GIAI ĐOẠN 4: KIỂM TRA NHẤT QUÁN TOÀN DIỆN (NGÀY 7)
> **Mục tiêu:** Rà soát chéo toàn bộ các sản phẩm theo Checklist chuẩn Mục 08.

#### 1. Người thực hiện chính:
* **Thành viên 4 (QA Lead - Chủ trì):**
  * **Task 4.1:** Lập và đối soát trực tiếp Bảng kiểm tra nhất quán (Mục 08) qua 8 tiêu chí khắt khe:
    1. Tên Use Case giữa Mục 04 và Mục 05 trùng khớp 100% từng ký tự.
    2. Phân hệ của 2 Use Case trong đặc tả khớp với hộp phân hệ trên sơ đồ Use Case.
    3. Tên làn Swimlane trong Activity Diagram khớp với Actor và Phân hệ trong Sơ đồ Use Case.
    4. Từng luồng `3a`, `5a`, ngoại lệ trong đặc tả đều có nhánh trên Activity Diagram và Bảng truy vết.
    5. Tên trạng thái trong Hậu điều kiện của đặc tả khớp từng chữ cái với State Chart.
    6. Mọi sự kiện trong State Chart gắn với 1 Use Case, thiết bị hoặc thời gian.
    7. 100% các mã giả định (`GĐ-01` $\rightarrow$ `GĐ-05`) đều xuất hiện trong ít nhất một mô hình.
    8. Đơn vị đo lường trong quy tắc nghiệp vụ (km, phút, km/h, bpm) trùng khớp với đề bài.
  * Xuất Bảng phiếu lỗi (Defect Log) nếu phát hiện bất kỳ sự lệch chữ nào.

#### 2. Vai trò của các thành viên:
* **Thành viên 1, 2, 3:** Tiếp nhận phiếu đối soát từ TV4; lập tức hiệu chỉnh văn bản hoặc mã SVG của mình nếu có sai lệch ký tự; hoàn thành chỉnh sửa trong vòng 3 giờ.

#### 3. Điểm dừng nghiệm thu (Gate 4 Sign-off):
* [x] 100% các ô kiểm tra trong Checklist Mục 08 được đánh dấu đạt chuẩn.

---

### GIAI ĐOẠN 5: ĐÓNG GÓI TỆP HTML DUY NHẤT & NGHIỆM THU OFFLINE (NGÀY 8)
> **Mục tiêu:** Tích hợp thành 1 file HTML duy nhất (Mục 09), kiểm thử ngoại tuyến không có mạng.

#### 1. Người thực hiện chính:
* **Thành viên 4 (Integrator):**
  * **Task 4.2:** Đóng gói toàn bộ nội dung thành đúng 1 tệp `.html` duy nhất:
    * Nhúng toàn bộ mã SVG (làm sạch ID, kiểm tra thẻ `<text>`).
    * Nhúng 100% bảng biểu bằng thẻ ngữ nghĩa `<table>`, không dùng ảnh chụp.
    * Nhúng toàn bộ CSS/JS nội bộ (tuyệt đối không dùng CDN ngoài).
* **Cả 4 thành viên (Final Sign-off):**
  * Tải file HTML về máy tính cá nhân.
  * **Ngắt kết nối Wi-Fi/Internet hoàn toàn**.
  * Mở file trên các trình duyệt Chrome, Edge, Firefox để kiểm tra: bố cục bảng, độ sắc nét khi phóng to SVG, thanh điều hướng cuộn trang, chế độ in ấn PDF.
  * Trưởng nhóm TV1 cùng cả nhóm ký xác nhận nghiệm thu và nộp bài.

---

## 4. MA TRẬN PHÂN CÔNG RACI THEO TIẾN TRÌNH TUẦN TỰ

* **R (Responsible):** Người trực tiếp làm sản phẩm.
* **A (Accountable):** Người chịu trách nhiệm cao nhất về chất lượng mục đó.
* **C (Consulted):** Người phản biện, review chất lượng trong thời gian chờ.
* **I (Informed):** Người tiếp nhận kết quả đầu ra để làm giai đoạn tiếp theo.

| Giai đoạn & Hạng mục sản phẩm | TV 1 (Architect) | TV 2 (Smartwatch) | TV 3 (Smartphone) | TV 4 (QA / Integrator) | Trình tự thực hiện |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **GĐ 1: Bảng giả định (Mục 03)** | **R / A** | C | C | I | **Bắt buộc xong trước** |
| **GĐ 1: Sơ đồ Use Case & 2 Bảng (Mục 04)** | **R / A** | C | C | I | **Bắt buộc xong trước** |
| **GĐ 2: Đặc tả UC-01 Smartwatch (Mục 05)** | C | **R / A** | I | I | Chờ đầu vào từ GĐ 1 |
| **GĐ 2: Đặc tả UC-02 Smartphone (Mục 05)** | C | I | **R / A** | I | Chờ đầu vào từ GĐ 1 |
| **GĐ 3: Activity Diagram & Truy vết (Mục 06)** | I | **R / A** | I | I | Chờ Đặc tả UC-01 từ GĐ 2 |
| **GĐ 3: State Chart & 2 Bảng vòng đời (Mục 07)** | I | I | **R / A** | I | Chờ Đặc tả UC-01 & 02 từ GĐ 2 |
| **GĐ 4: Danh sách kiểm tra nhất quán (Mục 08)** | C | C | C | **R / A** | Chờ toàn bộ GĐ 1, 2, 3 xong |
| **GĐ 5: Tích hợp HTML & Nghiệm thu Offline (Mục 09)**| A | I | I | **R** | Bước cuối cùng đóng gói |

---

## 5. BẢNG THEO DÕI BÀN GIAO ĐẦU VÀO / ĐẦU RA (HANDOVER TABLE)

| Giai đoạn | Người bàn giao | Người tiếp nhận | Tài liệu / Dữ liệu bàn giao bắt buộc | Điều kiện tiên quyết để bắt đầu |
| :--- | :--- | :--- | :--- | :--- |
| **GĐ 1 $\rightarrow$ GĐ 2** | TV 1 | TV 2, TV 3 | • Danh mục `GĐ-01` $\rightarrow$ `GĐ-05`<br>• Tên chuẩn của 2 Use Case và Actor | Cả 4 thành viên đã ký duyệt Sơ đồ Use Case và Giả định. |
| **GĐ 2 $\rightarrow$ GĐ 3** | TV 2 | TV 2 (tự chuyển tiếp) | • Bản đặc tả `UC-01` có luồng chính, `3a`, `5a`, ngoại lệ | Đặc tả `UC-01` đã đủ 14 mục và được TV1 review. |
| **GĐ 2 $\rightarrow$ GĐ 3** | TV 2 & TV 3 | TV 3 | • Hậu điều kiện của `UC-01` và `UC-02`<br>• Luồng xử lý sự cố đứt kết nối Bluetooth | Hai bản đặc tả đã khớp tên trạng thái với State Dictionary. |
| **GĐ 3 $\rightarrow$ GĐ 4** | TV 1, TV 2, TV 3 | TV 4 | • 3 mã nguồn SVG hoàn chỉnh<br>• Toàn bộ nội dung các bảng biểu | Tất cả các mô hình Mục 03 đến Mục 07 đã hoàn tất 100%. |
| **GĐ 4 $\rightarrow$ GĐ 5** | TV 4 | Cả nhóm | • File HTML hoàn chỉnh tích hợp toàn bộ nội dung | Checklist Mục 08 đạt 8/8 tiêu chí không có lỗi lệch ký tự. |
