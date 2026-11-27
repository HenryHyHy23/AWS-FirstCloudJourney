---
title: "Gửi OTP thử qua Gmail SMTP"
menuTitle: "Gmail SMTP gửi OTP hệ thống"
weight: 5
chapter: false
pre: "<b> 5.9.5. </b> "
---

1. **Mở tài khoản Gmail lab được phép gửi mail.** Nếu dùng App Password và chính sách cho phép, bật 2-Step Verification rồi tạo App Password. Với Workspace dùng cơ chế xác thực được quản trị viên cho phép.

2. **Mở secret backend và cấu hình Spring Mail.** Đối với STARTTLS đặt các trường sau.
   - Host — smtp.gmail.com
   - Port — 587
   - SMTP auth và STARTTLS — bật
   - Username và credential — inject vào ECS qua ValueFrom theo tên biến của ứng dụng

3. **Deploy task mới và mở luồng kích hoạt hoặc đặt lại mật khẩu trên BTB.** Dùng tài khoản demo và email nhận riêng. Ghi thời điểm gửi, mở Inbox/Spam rồi nhập mã trong ứng dụng.

**Kiểm tra kết quả.** Mã dùng được trong thời hạn và được xử lý theo quy tắc dùng một lần của ứng dụng. Nếu authentication failed, kiểm tra credential. Với timeout, kiểm tra egress tới cổng SMTP. Che OTP khi lưu ảnh demo.
