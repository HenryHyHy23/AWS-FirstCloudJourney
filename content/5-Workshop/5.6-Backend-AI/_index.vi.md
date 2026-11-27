---
title: "Triển khai Backend và AI"
menuTitle: "Triển khai Backend và AI"
weight: 6
chapter: false
pre: "<b> 5.6. </b> "
---



Chuẩn bị secret, log group ở 5.8.1 và chứng chỉ ALB ở 5.7.1. Kết quả cần kiểm tra là task chạy ổn định và target Healthy.

- [5.6.1. AWS Secrets Manager](5.6.1-aws-secrets-manager/) — Lưu cấu hình nhạy cảm của BE: database, AI, webhook, mail.
- [5.6.2. Application Load Balancer](5.6.2-application-load-balancer/) — Điểm vào API HTTPS, chuyển request về ECS target khỏe.
- [5.6.3. Amazon ECS kết hợp AWS Fargate](5.6.3-amazon-ecs-fargate/) — Chạy API Spring Boot modular monolith qua container managed.
- [5.6.4. Amazon Bedrock](5.6.4-amazon-bedrock/) — Cung cấp AI model inference cho backend/Generate UI; quyền nghiệp vụ vẫn do Spring Boot kiểm soát.
