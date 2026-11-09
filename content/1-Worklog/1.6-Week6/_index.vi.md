---
title: "Báo cáo công việc Tuần 6"
date: 2026-10-19
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

- Hiểu kết nối VPC bằng Peering và Transit Gateway.
- Kiểm tra route hai chiều, DNS, security group/NACL và chi phí.
- Hiểu cách triển khai ứng dụng Docker trên EC2, kết nối RDS và lưu image trên ECR/Docker Hub.
- Hiểu cách ECS Fargate quản lý task/service, kết nối ALB/Cloud Map và tự động scale.
- Hiểu pipeline phát hành ECS theo Git tag và cách theo dõi metric/log bằng Container Insights, FireLens.
- Kiểm chứng release, phân loại lỗi pipeline và kiểm tra đường chuyển log từ ECS sang S3.

### Các công việc cần triển khai trong tuần này:

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 1 | **Kết nối hai VPC bằng Peering** <br> - Tìm hiểu cách cho hai EC2 ở các VPC khác nhau giao tiếp bằng địa chỉ private. Peering nối riêng hai VPC có CIDR không chồng lấn; không hỗ trợ bắc cầu. <br> - Cần route tới CIDR peer ở cả hai phía, SG cho phép và NACL cho cả chiều phản hồi. Bật DNS support/hostnames và DNS peering để hostname EC2 public trả về IP private; private hosted zone cần liên kết riêng. Kiểm tra IP private rồi DNS. | 19/10/2026 | 19/10/2026 | - [Module 3 - 16: Setting up VPC Peering](https://000019.awsfcaj.com/) |
| 2 | **Kết nối nhiều VPC qua Transit Gateway** <br> - So sánh kết nối bốn VPC bằng peering với mô hình hub của Transit Gateway. Bốn VPC cần sáu peering hoặc bốn TGW attachment; mỗi VPC attachment chọn một subnet/AZ được bật. <br> - Association chọn bảng TGW cho traffic đi vào; propagation đưa CIDR vào bảng. Vẫn phải sửa route subnet VPC. Kiểm tra đường đi/về, SG/NACL và phân đoạn mạng. Route `0.0.0.0/0 → TGW` cần thêm VPC egress, NAT/IGW và route phù hợp để ra internet. | 20/10/2026 | 20/10/2026 | - [Module 3 - 17: Set up AWS Transit Gateway](https://000020.awsfcaj.com/) |
| 3 | **Deploy Application on Docker** <br> - Tìm hiểu cách chuyển ứng dụng local sang container trên EC2: build image frontend/backend rồi chạy từng container. Image đóng gói code và dependencies, giúp giữ môi trường triển khai nhất quán. <br> - Nginx nhận request, chuyển tới backend kết nối RDS private; SG database chỉ cho MySQL 3306 từ SG EC2. Mapping `3000:80` là cổng EC2 3000 tới cổng container 80. <br> - Dùng Compose quản lý service, network và cấu hình trong YAML. Sau đó login, tag, push image lên ECR/Docker Hub; EC2 truy cập ECR qua IAM role. Registry lưu image, còn EC2 chạy ứng dụng. | 21/10/2026 | 21/10/2026 | - [Module 3 - 18: Deploy Application on Docker](https://000015.awsfcaj.com/) |
| 4 | **Triển khai ứng dụng trên Amazon ECS** <br> - Tiếp nối bài Docker, tìm hiểu cách chạy frontend/backend trên ECS Fargate. Task definition khai báo image, CPU/RAM, port và cấu hình; service duy trì số task mong muốn và thay task lỗi. <br> - Task chạy trong private subnet; bài dùng NAT để pull image/gửi log. ALB public dùng target type `ip`, route `/api/*` tới backend. Cloud Map cung cấp `backend.FCAJResbar.internal` để Nginx gọi backend qua DNS thay vì gắn cứng IP. <br> - Tìm hiểu rolling update và target tracking theo CPU, phân biệt scale số task với scale cụm EC2. Kiểm tra health check, log và luồng đăng nhập/tạo đơn để đánh giá ứng dụng, thay vì chỉ nhìn trạng thái Running. | 22/10/2026 | 22/10/2026 | - [Module 3 - 19: Deploy applications on Amazon Elastic Container Service](https://000016.awsfcaj.com/) |
| 5 | **CI/CD và giám sát ứng dụng ECS** <br> - Tìm hiểu pipeline GitLab, GitHub Actions và CodeBuild. Tag release kích hoạt build frontend/backend → push image lên ECR → đăng ký task definition mới → cập nhật ECS service. <br> - GitLab dùng runner, GitHub dùng reusable workflow; CodeBuild chạy buildspec và lọc webhook `^refs/tags/`. Gắn version cho image để đối chiếu code với bản triển khai. <br> - Kiểm tra build log, image, task revision và giao diện qua ALB để xác nhận bản phát hành. Container Insights theo dõi CPU, RAM và network; job Succeeded chưa chứng minh ứng dụng hoạt động. <br> - FireLens dùng Fluent Bit sidecar chuyển stdout/stderr sang S3. Task role cấp quyền ghi bucket; execution role dùng cho pull image/CloudWatch Logs. Kiểm tra log thực tế sau buffering. | 23/10/2026 | 23/10/2026 | - [Module 3 - 20: Deploying CI/CD with ECS Container](https://000017.awsfcaj.com/) |
| 6 | **CI/CD ECS: kiểm chứng và xử lý lỗi** <br> - Tiếp tục bài CI/CD với hai CodeBuild project frontend/backend, mỗi project dùng buildspec riêng. Webhook `PUSH` kết hợp `HEAD_REF: ^refs/tags/` kích hoạt theo tag release; kiểm tra buildspec, biến môi trường và quyền IAM khi job lỗi. <br> - Theo dõi từ image tag `v1.0.2` tới task revision, trạng thái ECS, target health và giao diện qua ALB. Phân biệt build thành công với ứng dụng chạy đúng; đọc log đúng job/service để tìm điểm lỗi. <br> - Đối chiếu metric theo task family trong Container Insights; kiểm tra luồng log frontend → Fluent Bit → S3. Log có buffering, nên khi chưa thấy file cần xét cấu hình đích, quyền task role và đường mạng. | 24/10/2026 | 24/10/2026 | - [Module 3 - 20 (tiếp theo): Deploying CI/CD with ECS Container](https://000017.awsfcaj.com/) |

### Kết quả đạt được tuần 6:

- Phân biệt trạng thái kết nối Active với khả năng truy cập qua route, firewall và DNS.
- TGW giảm số kết nối cần quản lý; có phí attachment và xử lý dữ liệu. Peering khác AZ/Region có thể tính phí truyền dữ liệu.
- Phân biệt image, container và registry; hiểu port mapping, kết nối RDS và cách dùng Compose quản lý nhiều container.
- Phân biệt task definition, task và service; hiểu cách ALB/Cloud Map kết nối các thành phần và Fargate quản lý hạ tầng chạy container.
- Liên kết Git tag, image tag và task revision để kiểm chứng release; phân biệt metric với log và task role với execution role.
- Xác định lỗi ở trigger/build, triển khai ECS hoặc chuyển log; kiểm tra bản chạy và log lưu trữ trước khi kết luận hoàn tất.
