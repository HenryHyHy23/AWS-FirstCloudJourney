---
title: "Chuyển HTTPS từ ALB đến Spring Boot"
menuTitle: "Application Load Balancer"
weight: 2
chapter: false
pre: "<b> 5.6.2. </b> "
---

## Tạo target group cho Fargate

1. **Tìm EC2 → Load Balancing → Target Groups.**

![Tìm EC2 → Load Balancing → Target Groups.](/images/5-Workshop/5.6-Backend-AI/5.6.2-application-load-balancer/aws-application-load-balancer-step-1.webp)

2. **Chọn Create target group → IP addresses.** Nhập tên `btb-backend-tg`. Task dùng awsvpc nên ECS đăng ký IP riêng của task.

![Tạo target group loại IP.](/images/5-Workshop/5.6-Backend-AI/5.6.2-application-load-balancer/aws-application-load-balancer-step-2-create.webp)

3. **Chọn protocol, port và VPC.** Đặt Protocol HTTP, Port 8080, IPv4 và VPC chứa task.

![Chọn protocol, port và VPC.](/images/5-Workshop/5.6-Backend-AI/5.6.2-application-load-balancer/aws-application-load-balancer-step-2.webp)

4. **Mở Health checks.** Chọn HTTP, traffic port và path `/api/health`. Endpoint phải trả 200 cho health checker.

![Path health check của BTB.](/images/5-Workshop/5.6-Backend-AI/5.6.2-application-load-balancer/aws-application-load-balancer-step-22.webp)

5. **Mở Advanced health check settings → Next.** Đối chiếu ví dụ trong ảnh.
   - Unhealthy threshold — 2
   - Timeout — 5 giây
   - Interval — 30 giây
   - Success codes — 200

![Timeout, interval và mã thành công.](/images/5-Workshop/5.6-Backend-AI/5.6.2-application-load-balancer/aws-application-load-balancer-step-23.webp)

6. **Mở Register targets.** Màn hình minh họa chọn network và nhập IP. Nếu theo luồng ECS service ở 5.6.3, để ECS tự đăng ký IP khi task chạy.

![Mở Register targets.](/images/5-Workshop/5.6-Backend-AI/5.6.2-application-load-balancer/aws-application-load-balancer-step-3-register-target.webp)

7. **Kiểm tra cổng target → hoàn tất tạo group.** Trong ảnh target đăng ký trên 8080. Ghi lại tên target group cho listener và service.

![IP của target và cổng backend.](/images/5-Workshop/5.6-Backend-AI/5.6.2-application-load-balancer/aws-application-load-balancer-step-3-register-target-1.webp)

## Tạo listener HTTPS và kiểm tra thuộc tính

8. **Mở Load balancers → Create → Application Load Balancer.** Chọn internet-facing, VPC lab, hai subnet public khác AZ và ALB-SG. Tạo HTTPS:443, chọn cert Singapore từ 5.7.1, đặt Forward → btb-backend-tg.

9. **Kiểm tra listener HTTP:80 nếu có.** Ảnh là hộp thoại xóa listener cũ. Khi làm mới, giữ HTTPS:443. Có thể giữ HTTP:80 để redirect sang HTTPS; chỉ dọn listener lab thừa sau khi thử đường thay thế.

![Kiểm tra listener HTTP:80 nếu có.](/images/5-Workshop/5.6-Backend-AI/5.6.2-application-load-balancer/aws-application-load-balancer-step-5-delete-listener.webp)

10. **Mở ALB → Attributes → Edit.** Đọc connection idle timeout và cách xử lý X-Forwarded-For. Chỉ điều chỉnh timeout khi có số liệu của ứng dụng.

![Mở ALB → Attributes → Edit.](/images/5-Workshop/5.6-Backend-AI/5.6.2-application-load-balancer/aws-application-load-balancer-step-6-edit-attributes.webp)

11. **Trong Logging, chọn đích log khi cần.** Với access logs tới S3, chọn bucket log cùng Region đã cho phép ELB ghi. Review → Save changes.

![Attributes của ALB, gồm bảo vệ xóa và access logs.](/images/5-Workshop/5.6-Backend-AI/5.6.2-application-load-balancer/aws-application-load-balancer-step-6-edit-attributes-1.webp)

**Kiểm tra kết quả.** Sau bài ECS, mở Targets → Health details. Timeout thường liên quan SG/cổng; ResponseCodeMismatch cần đối chiếu path và status. So log Java trong CloudWatch với status ALB/target trong access log.
