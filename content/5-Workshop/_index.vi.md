---
title: "Bite to Business trên AWS"
menuTitle: "Workshop"
weight: 5
chapter: false
pre: "<b> 5. </b> "
---



Triển khai storefront React và API Spring Boot của Bite to Business, từ VPC đầu tiên tới lúc một yêu cầu mua hàng đi qua CloudFront, ALB và ECS. Sau đó theo dõi lỗi, chi phí và sự kiện đơn hàng.

## Cách thực hiện workshop

Dùng tài khoản lab cho các bước tạo tài nguyên. Tên trong ảnh giúp đối chiếu thao tác. Thay domain, account ID, ARN, UUID và credential bằng giá trị của môi trường riêng. Bấm vào ảnh để xem chi tiết. Mỗi bước thực hành ghi thao tác và các trường cần điền. Trong một nhóm policy, mẫu đầu tiên có ảnh cùng code ngay bên dưới. Các mục “Xem ảnh đối chiếu” mở thêm màn hình khi cần.

**Phạm vi minh chứng.** Note xác nhận nhóm đã triển khai FE/BE. Ảnh bổ sung custom bus EventBridge, queue SQS, rule WAF ở nhiều thời điểm và cấu hình quota Bedrock. Kết quả API AI production, RUM ingestion và consumer xử lý sự kiện cần kiểm thử tại 5.11.

## Các bài thực hành

- [5.1. Tổng quan](5.1-overview/)
- [5.2. Chuẩn bị](5.2-prerequisites/)
- [5.3. Kiến trúc và luồng dữ liệu](5.3-architecture/)
- [5.4. Mạng và thiết lập nền tảng](5.4-foundation/)
- [5.5. Container và lưu trữ](5.5-containers-storage/)
- [5.6. Triển khai Backend và AI](5.6-backend-ai/)
- [5.7. Phân phối web và bảo mật](5.7-web-security/)
- [5.8. Giám sát, cảnh báo và sự kiện](5.8-monitoring-alerts/)
- [5.9. Tích hợp dịch vụ bên ngoài](5.9-external-integrations/)
- [5.10. Triển khai CI/CD](5.10-ci-cd/)
- [5.11. Kiểm thử và trình diễn](5.11-testing/)
- [5.12. Dự toán chi phí](5.12-cost/)
- [5.13. Dọn tài nguyên và khắc phục lỗi](5.13-cleanup/)
