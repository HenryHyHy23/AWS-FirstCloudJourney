---
title: "Chuẩn bị môi trường lab"
menuTitle: "Chuẩn bị"
weight: 2
chapter: false
pre: "<b> 5.2. </b> "
---



Dùng tài khoản AWS lab, clone repo React và Spring Boot của nhóm và chọn revision cần thực hành. Chuẩn bị quyền triển khai các dịch vụ của bài, một hộp thư test và quyền quản lý DNS nếu thử custom domain. Cài Docker, Git, Node/npm, AWS CLI, PowerShell và JDK theo file build backend.


```powershell
git --version
docker version
aws --version
aws sts get-caller-identity --query Arn --output text
node --version
npm --version
```


## Giá trị cần xác định trước khi tạo tài nguyên

**Region.** Bộ ảnh backend, Bedrock quota và EventBridge dùng Singapore; chứng chỉ cho CloudFront dùng N. Virginia. S3 cho frontend dùng Region đã chọn trên màn Create bucket.

**Tên và ID.** Trong ảnh có generate-ui-be (ECR), btb-backend (secret), btb-backend-tg (target group), btb-events (bus), btb-customer-events (queue), btb-system-alerts (SNS). Với môi trường dùng chung, thêm hậu tố lab và cập nhật ARN trong mọi ví dụ.

## Trình tự thực hành khi bắt đầu từ tài khoản trống

1. Budget → IAM → VPC và đường ra cho private task. Chuẩn bị IAM bổ sung sau khi có ARN secret, bus và queue.

2. ECR/image và S3/build → hosted zone/ủy quyền DNS → ACM hai Region → secret và log group → target group, ALB → ECS service.

3. Tạo alias api tới ALB → CloudFront/OAC và hai behavior → Alias FE → WAF. Kiểm tra FE, route con, API health.

4. SNS → alarm; RUM → Cognito → build lại FE; SQS → EventBridge → thử message. Hoàn thành demo, dự toán và cleanup ở 5.11–5.13.
