---
title: "Giám sát, cảnh báo và sự kiện"
menuTitle: "Giám sát, cảnh báo và sự kiện"
weight: 8
chapter: false
pre: "<b> 5.8. </b> "
---



Có ba luồng để thử: log/metric vận hành từ backend, telemetry từ trình duyệt, và sự kiện đơn hàng tới queue. Tạo SNS trước alarm, tạo SQS trước khi gắn target EventBridge.

- [5.8.1. Amazon CloudWatch Logs](5.8.1-cloudwatch-logs/) — Log stdout/stderr của Spring Boot để phân tích startup, lỗi API/tích hợp.
- [5.8.2. CloudWatch Metrics và Alarms](5.8.2-cloudwatch-metrics-alarms/) — Giám sát CPU/RAM ECS, lỗi/độ trễ/target ALB và kích hoạt cảnh báo.
- [5.8.3. Amazon SNS](5.8.3-amazon-sns/) — Gửi email cảnh báo alarm đến email subscriber đã xác nhận.
- [5.8.4. CloudWatch RUM](5.8.4-cloudwatch-rum/) — Theo dõi hiệu năng và lỗi JavaScript của frontend React từ trình duyệt.
- [5.8.5. Amazon Cognito Identity Pools](5.8.5-cognito-identity-pools/) — Cấp AWS credential tạm cho RUM client của trình duyệt theo cấu hình hiện tại.
- [5.8.6. Amazon EventBridge](5.8.6-amazon-eventbridge/)
- [5.8.7. Amazon SQS & DLQ](5.8.7-amazon-sqs/)
