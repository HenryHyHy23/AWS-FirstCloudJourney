---
title: "Chi phí của một môi trường BTB mẫu"
menuTitle: "Dự toán chi phí"
weight: 12
chapter: false
pre: "<b> 5.12. </b> "
---


Ảnh task definition chọn Linux/x86, 0,5 vCPU và 2 GB. Ví dụ tính cho Singapore, 1 task và 1 ALB chạy 730 giờ/tháng, giả định trung bình 1 LCU. Bảng dưới dùng giá niêm yết USD tra ngày 25/11/2026, chưa tính thuế, credit hay Free Tier. Số task, số giờ và LCU là giả định thực hành.

| Khoản tính | Khối lượng × đơn giá | USD/tháng |
|---|---|---:|
| Fargate vCPU | 0.5 × 730 × 0.05056 | 18.4544 |
| Fargate RAM | 2 × 730 × 0.00553 | 8.0738 |
| ALB hours | 1 × 730 × 0.0252 | 18.3960 |
| ALB LCU | 1 × 730 × 0.008 | 5.8400 |

**Tạm tính Fargate + ALB: 50,76 USD/tháng.** Với đúng các giả định trên, lab 4 giờ có tạm tính khoảng 0,28 USD. Đây là hai hạng mục của mô hình; hóa đơn toàn hệ thống còn phụ thuộc các dịch vụ dưới đây. Ngân sách 200 USD ở bài 5.4.1 là mức để cảnh báo.

## Hoàn thiện bảng dự toán cho buổi demo

1. Trong AWS Pricing Calculator tạo estimate, chọn Singapore cho compute và LCU đo từ CloudWatch. Với ECS chạy nhiều task, nhân CPU/RAM với tổng task-hours. Ghi thêm thời gian rollout có hai revision chạy đồng thời.
2. Thêm ECR (GB image), S3 (GB, request, version), CloudFront (dữ liệu theo khu vực người xem và request), Route 53 (hosted zone/query), WAF (số ACL, rule và request), Secrets Manager (secret và API call). Kiểm tra loại chứng chỉ ACM đang dùng.
3. Thêm phí mạng theo kiến trúc lab: public IPv4, NAT theo giờ/GB hoặc interface endpoints theo AZ, data transfer. Với CloudWatch, ghi GB log ingest/retention, alarm và số event RUM. Thêm SNS delivery, EventBridge custom events/archive, SQS requests/DLQ và token theo đúng model Bedrock.
4. Đối chiếu tổng Calculator với Cost Explorer bằng cùng khoảng thời gian, account và Region. Ghi chi phí dịch vụ ngoài AWS riêng. Lưu link chia sẻ/export cùng ngày và giả định sau khi nhập workload thực tế.

**Tối ưu theo phép đo.** Tắt tài nguyên lab sau buổi thử, đo CPU/memory trước khi hạ kích thước task, giảm sampling RUM sau khi hoàn tất test, đặt retention cho log/archive. So đường ra cần cho Supabase/SMTP trước khi chọn NAT hoặc endpoint.

[Amazon ECS price list · Singapore](https://pricing.us-east-1.amazonaws.com/offers/v1.0/aws/AmazonECS/current/ap-southeast-1/index.json) · [ELB price list · Singapore](https://pricing.us-east-1.amazonaws.com/offers/v1.0/aws/AWSELB/current/ap-southeast-1/index.json) · [AWS Pricing Calculator](https://calculator.aws/)
