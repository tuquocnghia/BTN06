# BTN06 — Yêu cầu nộp và đối soát tích hợp

## Tệp nộp

**`output/4.html`** là bài tích hợp của thành viên 4. Mở trực tiếp bằng trình duyệt, không cần máy chủ hoặc kết nối mạng.

Theo mục 09 của `debai.html`, nộp một tệp HTML với thứ tự:

| Mục | Yêu cầu | Nội dung trong bản nộp |
|---|---|---|
| 03 | Ít nhất 5 giả định ngoài ví dụ; nêu chỗ chưa rõ, quyết định, mô hình áp dụng; có giả định trao đổi dữ liệu | 12 giả định, gồm đồng bộ, đo đạc, mục tiêu, GPS, tạm dừng và xử lý lỗi |
| 04 | Use Case toàn hệ thống, hai ranh giới, actor, association, include, extend có điều kiện; hai bảng ghi chú | 12 use case; 1 include, 2 extend; ghi chú quan hệ, phân hệ và phạm vi chức năng |
| 05 | Hai use case khác phân hệ, mục tiêu và kích hoạt; mỗi bản có lý do chọn, đủ 14 mục, ít nhất 2 luồng thay thế và 1 ngoại lệ | UC-01 theo dõi trên Smartwatch; UC-02 đồng bộ trên Smartphone |
| 06 | Activity của một use case đã đặc tả; swimlane, decision, guard, merge, kết thúc và truy vết | UC-01; 3 làn; 3a, 5a, 2b; giải thích không dùng fork/join |
| 07 | Vòng đời Hoạt động thể chất; sự kiện/guard/action; bảng chuyển trạng thái 6 cột; ít nhất 3 cặp bị từ chối | 7 trạng thái, điểm đầu/cuối; T01–T14; 6 dòng từ chối |
| 08 | Checklist nhất quán đã đánh dấu | 8 tiêu chí có dẫn chiếu |

Yêu cầu kỹ thuật: `lang="vi"`, UTF-8, điều hướng 03–08, SVG nhúng có chữ văn bản, bảng HTML, CSS/JS nội bộ. Đề không yêu cầu lập trình ứng dụng sức khỏe hoặc nộp PDF thay HTML.

## Phiếu lỗi đã xử lý trong bản tích hợp

| Mã | Vấn đề trong phần bàn giao | Cách sửa tại `output/4.html` |
|---|---|---|
| D01 | UC-02 trên sơ đồ thiếu cụm “hoạt động”; nhiều tên khác lệch giữa hình và bảng | Dùng tên thống nhất trong hình, bảng và đặc tả |
| D02 | GĐ-03 trong phần TV1 là lịch biểu, khác kế hoạch; quy tắc bỏ lỡ có hai mốc khác nhau | GĐ-03 dùng cho mục tiêu; GĐ-07 làm rõ lịch biểu; bỏ quy tắc tự kết luận bỏ lỡ không cần thiết |
| D03 | Dùng use case lưu tạm và mũi tên Bluetooth để diễn tả các bước kỹ thuật | Lưu tạm trở thành bước UC-01; đồng bộ là UC-02; include dùng cho bản đồ theo hướng dẫn bài giảng |
| D04 | Thiếu use case nhắc nhở, đặt mục tiêu và thống kê sâu trên sơ đồ | Bổ sung UC-12, UC-13, UC-14; bảng phạm vi bao phủ thống kê tuần/tháng/năm, kcal, độ cao, tốc độ/nhịp tim |
| D05 | Mục include/extend của UC-02 còn là lời nhắc chờ đối chiếu | Hoàn thiện mục 14: UC-02 không có include/extend |
| D06 | Đồng bộ lại đi thẳng từ Đã lưu tạm đến Đã đồng bộ; thiếu nút cuối | Thử lại qua Đang đồng bộ, xác thực và ACK; thêm điểm cuối sau giải phóng nguồn |
| D07 | State Chart chưa phản ánh tạm dừng, truyền lại, trùng dữ liệu đầy đủ như đặc tả | Bổ sung Đang tạm dừng; T03–T04, T08, T10–T11 và bảng chuyển tương ứng |
| D08 | Chưa phân biệt điện thoại đã lưu với đồng hồ đã nhận ACK | ACK thất lạc giữ cả nguồn lẫn bản đích hoàn chỉnh; thử lại chống trùng; chỉ giải phóng sau ACK hợp lệ |
| D09 | Lỗi hết lượt checksum và xung đột định danh chưa có ngoại lệ riêng; số lượt truyền lại chưa rõ | Thêm 4c, 5b; tối đa 3 lần truyền lại ngoài lần gửi đầu; GĐ-06 làm nguồn quy tắc |
| D10 | Phần TV3 tải phông chữ ngoài; bố cục SVG có chữ ngoài khung | Bản nộp dùng phông hệ thống, SVG bố trí lại; CSS/JS nhúng |
| D11 | Thanh điều hướng không chọn mục cuối khi trang đã cuộn hết | Xử lý vị trí cuối trang; kiểm tra lại bằng thao tác bấm mục 08 |

## Kiểm tra kỹ thuật

Đã mở bằng `file://` trên Chrome và Edge ở chế độ chặn mạng của trình duyệt tự động:

- Đủ sáu mục 03–08, ba SVG, hai bảng đặc tả có đúng 14 dòng, tám ô đối chiếu được đánh dấu.
- Không ID trùng, liên kết nội bộ hỏng, marker SVG thiếu hoặc chữ SVG vượt viewBox.
- Không tài nguyên ngoài, yêu cầu HTTP/HTTPS hoặc lỗi JavaScript.
- Trang không tràn ngang ở khung nhìn 1500 px và 390 px; bảng/sơ đồ rộng cuộn trong khung riêng.
- Điều hướng đến mục 08 hoạt động; thanh điều hướng ẩn khi in; xuất được PDF bằng Chrome và Edge.
- Đã xem hình kết xuất ba sơ đồ và bản PDF để kiểm tra bố cục.

Firefox chưa được kiểm tra vì môi trường không có trình duyệt này. Thông tin họ tên, MSSV và mã nhóm chưa được cung cấp nên không tự điền vào bài.

Tài liệu đối chiếu: `debai.html`, `tasks.md`, `software-modeling.html` và ba bản bàn giao `output/1.html`, `output/2.html`, `output/3.html`.
