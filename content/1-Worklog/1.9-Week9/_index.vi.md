---
title: "Báo cáo công việc Tuần 9"
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu tuần 9:

- Tiếp tục Project cuối khoá.
- Ôn thi Coursera và tìm hiểu các mô hình triển khai container trên AWS.

### Các công việc cần triển khai trong tuần này:

| Ngày | Công việc             | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------------------- | ------------ | --------------- | -------------- |
| 1 | **Amazon Lightsail Containers** <br> - Tìm hiểu cách triển khai image NGINX công khai, sau đó build và push image riêng từ CloudShell. Kiểm tra phiên bản mới qua endpoint HTTPS. <br> - **Coursera:** Ôn tập chuẩn bị thi. <br> - **Final project:** Cấu hình GitHub Actions và viết file workflow YAML để chạy CI/CD cho dự án. Tiếp tục hoàn thiện dự án. | 09/11/2026 | 09/11/2026 | [Module 5 - 1: Amazon Lightsail Containers](https://000046.awsfcaj.com/) |
| 2 | **Làm quen với Amazon EKS** <br> - Phân biệt control plane và worker node. Tìm hiểu cách tạo cluster bằng `eksctl`, kiểm tra Deployment và Service bằng `kubectl`, và vai trò của readiness probe, liveness probe. <br> - **Coursera:** Ôn tập chuẩn bị thi. <br> - **Final project:** Tiếp tục làm để hoàn thành dự án. | 10/11/2026 | 10/11/2026 | [Module 5 - 2: Getting Started with Amazon EKS](https://000126.awsfcaj.com/) |
| 3 | **CI/CD với AWS CodePipeline** <br> - Tìm hiểu các bước lấy mã nguồn, build, kiểm thử và triển khai. Theo dõi artifact giữa các bước và dùng lịch sử chạy, log build để xác định bước bị lỗi. <br> - **Coursera:** Ôn tập chuẩn bị thi. <br> - **Final project:** Tiếp tục làm để hoàn thành dự án. | 11/11/2026 | 11/11/2026 | [Module 5 - 3: CI/CD with CodePipeline](https://000152.awsfcaj.com/) |
| 4 | **Đóng gói ứng dụng monolith Mythical Mysfits** <br> - Tìm hiểu cách Dockerfile đóng gói ứng dụng cùng các thư viện phụ thuộc. Build và kiểm tra image ở máy cục bộ trước khi push lên Amazon ECR. <br> - **Coursera:** Ôn tập chuẩn bị thi. <br> - **Final project:** Tiếp tục làm để hoàn thành dự án. | 12/11/2026 | 12/11/2026 | [Module 5 - 4: Monolith to Microservices with Docker and AWS Fargate](https://000067.awsfcaj.com/) |
| 5 | **Triển khai microservice với ECS và Fargate** <br> - Tìm hiểu task definition, ECS service và ALB target group. Xem cách tách một tính năng thành container riêng và kiểm tra health check, định tuyến sau khi cập nhật service. <br> - **Coursera:** Ôn tập chuẩn bị thi. <br> - **Final project:** Tiếp tục làm để hoàn thành dự án. | 13/11/2026 | 13/11/2026 | [Module 5 - 4: Monolith to Microservices with Docker and AWS Fargate (continued)](https://000067.awsfcaj.com/) |
| 6 | **CI/CD trên Amazon EKS với CodePipeline và GitHub** <br> - Tìm hiểu cách thay đổi trên GitHub kích hoạt pipeline và cách CodeBuild kết nối với EKS để triển khai. Phân biệt service role của AWS với Kubernetes RBAC và kiểm tra quá trình rollout. <br> - **Coursera:** Ôn tập chuẩn bị thi. <br> - **Final project:** Tiếp tục làm để hoàn thành dự án. | 14/11/2026 | 14/11/2026 | [Module 5 - 5: CI/CD on EKS with CodePipeline and GitHub](https://000062.awsfcaj.com/) |

### Kết quả đạt được tuần 9:

- Nắm các cách triển khai container trên Lightsail, ECS/Fargate và EKS, cùng các bước CI/CD với CodePipeline.
