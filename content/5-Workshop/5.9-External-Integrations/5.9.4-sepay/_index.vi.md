---
title: "Theo dõi thanh toán SePay từ QR đến webhook"
menuTitle: "SePay QR chuyển khoản và webhook"
weight: 4
chapter: false
pre: "<b> 5.9.4. </b> "
---

1. **Mở SePay dashboard của môi trường thử → cấu hình webhook.** Nhập đúng HTTPS callback lab, đối chiếu route của BE. Ảnh WAF dùng /api/payments/sepay/webhook.

2. **Chọn cơ chế xác thực webhook mà controller hỗ trợ.** Lưu credential trong secret phía BE. Đối chiếu callback đi qua CloudFront hay trực tiếp đến ALB với rule ở 5.7.4.

3. **Mở BTB → tạo đơn demo chờ thanh toán.** So mã đơn và số tiền QR với backend. Dùng test/sandbox của tài khoản nếu có, hoặc fixture đã duyệt trong môi trường test.

4. **Đối chiếu kết quả webhook.** Ghi event ID, status và thời gian. Kiểm tra đơn tương ứng sau callback hợp lệ. Replay cùng event demo và xem số bản ghi thanh toán.

**Kiểm tra kết quả.** Một giao dịch hợp lệ chỉ được ghi nhận một lần. Nếu 403, đối chiếu sampled requests ở WAF và log Spring cùng thời điểm để biết đoạn nào từ chối.
