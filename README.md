# Lab S1 – Mô hình đe dọa EduPortal

## Hệ thống đã chọn

**EduPortal** – hệ thống quản lý học tập trực tuyến giả định, gồm ba thành phần:

- **Trình duyệt (Client):** sinh viên và giảng viên đăng nhập, xem bài giảng, nộp bài, tra điểm.
- **Máy chủ ứng dụng (App Server):** xác thực danh tính, phân quyền, xử lý bài nộp và điều phối dữ liệu.
- **Cơ sở dữ liệu (Database):** lưu tài khoản, mật khẩu, điểm số, bài nộp và thông tin khóa học.

Luồng dữ liệu chính: trình duyệt gửi thông tin đăng nhập và bài nộp lên máy chủ; máy chủ truy vấn và ghi dữ liệu vào database; kết quả trả ngược về trình duyệt để hiển thị.

---

## Ba mối được chọn xử lý

### M02 – SQL Injection vào ô đăng nhập (20 điểm rủi ro)

Nếu ô nhập liệu không lọc ký tự đặc biệt, kẻ tấn công chèn câu lệnh SQL để truy xuất toàn bộ bảng tài khoản và điểm số. Đây là mối có điểm rủi ro cao nhất (tác động 5 × khả năng 4 = 20) và hậu quả toàn diện nhất: một cuộc tấn công thành công làm lộ dữ liệu của mọi người dùng cùng lúc, không phân biệt vai trò.

### M06 – Brute-force tài khoản đăng nhập (16 điểm rủi ro)

Nếu hệ thống không giới hạn số lần thử sai, công cụ tự động có thể thử hàng nghìn mật khẩu mà không bị chặn. Chi phí xử lý thấp nhất trong ba mối được chọn (0,5 ngày công, dùng thư viện mã nguồn mở), trong khi mức độ tác động và khả năng xảy ra đều ở ngưỡng cao. Tỉ lệ chi phí–lợi ích là lý do chính để ưu tiên mối này.

### M03 – Leo thang quyền xem điểm qua tham số URL (16 điểm rủi ro)

Khi máy chủ không kiểm tra vai trò ở mỗi yêu cầu, sinh viên A có thể xem điểm của sinh viên B bằng cách thay ID trên URL. Mối này cùng điểm với M06 (16), nhưng được chọn thay vì M01 (15) vì nó tấn công trực tiếp vào tính toàn vẹn của điểm số – tài sản cốt lõi của một hệ thống LMS. Lộ điểm số của người khác còn gây hệ quả pháp lý về bảo vệ dữ liệu cá nhân, vượt ra ngoài thiệt hại kỹ thuật thuần túy.

---

## Điều bị bỏ lại và lý do

Năm mối còn lại (M01, M04, M05, M07, M08) không được chọn không phải vì không nguy hiểm, mà vì nguồn lực xử lý có giới hạn và các mối được chọn bảo vệ tốt hơn cho tài sản cốt lõi của hệ thống.

- **M01** (nghe lén HTTP, điểm 15) bị gác lại vì việc bật HTTPS là bước hạ tầng cần làm song song với nhiều thứ khác, không chỉ riêng mối đe dọa này; nó sẽ được xử lý ở tầng vận hành, không phải tầng ứng dụng.
- **M04** (database lộ internet, điểm 10) có tác động cao nhưng khả năng xảy ra thấp hơn nếu cấu hình mạng cơ bản đã đúng; là việc của đội vận hành hơn là lập trình viên.
- **M05, M07, M08** có điểm rủi ro thấp hơn và sẽ được đưa vào backlog xử lý ở vòng tiếp theo.
