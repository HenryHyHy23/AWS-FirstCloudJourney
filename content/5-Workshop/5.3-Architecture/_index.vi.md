---
title: "Kiến trúc của Bite to Business"
menuTitle: "Kiến trúc và luồng dữ liệu"
weight: 3
chapter: false
pre: "<b> 5.3. </b> "
---

Bắt đầu ở biểu tượng **User** bên trái. Theo mũi tên **1** để xem cách trình duyệt tìm tên miền, rồi theo mũi tên **2** để xem kết nối HTTPS đến CloudFront. Bấm vào hình để đọc các nhãn ở kích thước gốc.

{{< btbfigure file="architecture-btb.png" caption="Kiến trúc AWS của nhóm Bite to Business. Các số từ 1 đến 28 đánh dấu những luồng được giải thích bên dưới." >}}

| Số trên hình | Luồng | Cách lần theo trong workshop |
|---|---|---|
| 1–3 | Mở trang bán hàng | Mắt Bão ủy quyền DNS cho Route 53. Trình duyệt phân giải tên miền (1), kết nối HTTPS đến CloudFront (2), rồi nhận các tệp giao diện React từ nguồn S3 (3). Đối chiếu chứng chỉ ACM của CloudFront tại `us-east-1`. |
| 4–6 | Gọi API | Theo nhánh API từ CloudFront qua Internet Gateway đến Application Load Balancer (ALB), rồi đến task ECS Fargate. Trong bài ALB, mở target group để kiểm tra trạng thái task. Sơ đồ thể hiện WAF cho cả CloudFront và ALB. |
| 7–13 | Tính năng của backend | Theo từng nhánh từ ECS đến Supabase Auth (7), VietMap (8), Cloudinary (9), Bedrock Nova Pro với Guardrails (10) và SePay (11). Khi gửi OTP, backend dùng Gmail SMTP (12) để chuyển thư đến người dùng (13). |
| 14–18 | Gửi email theo sự kiện | ECS phát sự kiện lên EventBridge (14). Rule chuyển sự kiện vào SQS (15). Lambda xử lý thông điệp (16) rồi yêu cầu SES gửi email (17–18). Khi làm lab, đối chiếu target của rule với queue mà Lambda đọc. |
| 19–23 | Theo dõi và cảnh báo | Xem log ứng dụng của ECS trong CloudWatch (19). Lần theo cảnh báo qua SNS đến hộp thư vận hành (20–21). Dùng Athena để truy vấn access log của ALB trong S3 (22). CloudWatch RUM ghi nhận hiệu năng và lỗi trên trình duyệt (23). |
| 24–28 | Triển khai và khởi động backend | Lần theo thay đổi từ lập trình viên đến GitHub và GitHub Actions. Workflow dùng OIDC để nhận quyền AWS và đẩy image lên ECR (24–26). ECS kéo image để chạy task (27) và lấy cấu hình bí mật từ Secrets Manager (28). |
