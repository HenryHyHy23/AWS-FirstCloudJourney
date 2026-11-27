---
title: "Chạy backend bằng ECS Fargate"
menuTitle: "Amazon ECS kết hợp AWS Fargate"
weight: 3
chapter: false
pre: "<b> 5.6.3. </b> "
---

1. **Mở ECS → Clusters → Create cluster.** Đặt tên cluster và chọn Fargate. Chuẩn bị URI ECR, hai role IAM, secret, target group và log group trước khi tạo service.

2. **Mở Task definitions → Create new task definition.** Nhập family và chọn AWS Fargate.

{{< btbfigure file="aws-application-load-balancer-step-4-create-task-definition.webp" caption="Family và lựa chọn Fargate." step="yes" >}}

3. **Chọn môi trường và hai IAM role.** Đối chiếu các trường trong ảnh.
   - OS/architecture — Linux/X86_64; network mode — awsvpc
   - Task size — 0,5 vCPU, 2 GB
   - Task role — role backend ở 5.4.2; Task execution role — role tải ECR/secret và ghi log

{{< btbfigure file="aws-application-load-balancer-step-4-create-task-definition-1.webp" caption="CPU, RAM, task role và execution role trong cùng revision." step="yes" >}}

4. **Thêm container backend.** Nhập tên `btb-backend`, chọn image đã push, bật Essential và map TCP 8080. Spring Boot phải lắng nghe trên interface của container.

{{< btbfigure file="aws-application-load-balancer-step-4-create-task-definition-2.webp" caption="Port mapping container." step="yes" >}}

5. **Kiểm tra Resource allocation limits của container.** Đặt giới hạn CPU/RAM tương thích với task size. Chừa bộ nhớ cho JVM ngoài heap khi đặt giới hạn.

{{< btbfigure file="aws-application-load-balancer-step-4-create-task-definition-3.webp" caption="Kiểm tra Resource allocation limits của container." step="yes" >}}

6. **Mở Environment variables → Add environment variable.** Với Cloudinary, SePay, SMTP và database, chọn ValueFrom cho key nhạy cảm và tham chiếu ARN từ bài Secrets Manager.

{{< btbfigure file="aws-application-load-balancer-step-4-create-task-definition-4.webp" caption="Khóa cấu hình Cloudinary, SMTP, SePay và database trong task." step="yes" >}}

7. **Hoàn tất biến Supabase và Vietmap.** Kiểm tra tên key Spring đọc. Dùng Value cho cấu hình thường như JAVA_TOOL_OPTIONS. Trong Logging chọn awslogs, đúng Region, log group và stream prefix ecs. Tạo revision.

{{< btbfigure file="aws-application-load-balancer-step-4-create-task-definition-5.webp" caption="Biến Supabase và Vietmap trong task." step="yes" >}}

8. **Chọn Cluster → Services → Create.** Chọn family:revision vừa tạo.
   - Compute — Fargate; Desired tasks — 1 cho lab
   - Networking — subnet private có egress, TASK-SG, tắt public IP
   - Load balancing — ALB lab, btb-backend:8080 và target group IP; đặt grace period đủ cho Spring khởi động

**Kiểm tra kết quả.** Đợi Running = Desired, kiểm tra target Healthy rồi gọi GET /api/health qua HTTPS. Nếu rollout lỗi, xem ECS Events và stopped reason của task mới.
