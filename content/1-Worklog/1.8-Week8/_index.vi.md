---
title: "Worklog Tuần 8"
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

- Bắt đầu Project cuối khoá.
- Tìm hiểu cách di chuyển ứng dụng Java, tự động hóa phát hành và tách tác vụ xử lý ảnh thành microservice.
- Nắm cách chọn kích cỡ EC2 dựa trên số liệu sử dụng.
- Biết cách phân tích chi phí AWS bằng Cost Explorer.
- Truy vấn dữ liệu chi phí và mức sử dụng chi tiết bằng AWS Glue và Amazon Athena.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                     | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ----------------------------- | ------------ | --------------- | -------------- |
| 1 | **Tối ưu kích cỡ Amazon EC2** <br> - Tìm hiểu cách gắn IAM role cho EC2 để dùng Systems Manager, gửi metric lên CloudWatch và kiểm tra thông tin xác thực tạm thời bằng STS. <br> - Dùng Run Command để cài và cấu hình CloudWatch Agent, bổ sung chỉ số RAM bên cạnh CPU và mạng. <br> - Xem khuyến nghị từ Compute Optimizer trong Cost Optimization Hub. Dựa vào tải thực tế, gồm cả giờ cao điểm, trước khi đổi kích cỡ hoặc cam kết chi phí. <br> - **Final project:** Tìm hiểu các dịch vụ AWS phù hợp để chuẩn bị vẽ sơ đồ kiến trúc cho dự án. | 02/11/2026 | 02/11/2026 | - [Module 3 - 27: Right-sizing Amazon EC2](https://000032.awsfcaj.com/) |
| 2 | **Trực quan hóa chi phí với AWS Cost Explorer** <br> - Đọc biểu đồ theo ngày và tháng, lọc theo dịch vụ rồi nhóm theo Region hoặc loại sử dụng để tìm các khoản phí như giờ chạy NAT Gateway. Lưu và xóa báo cáo mẫu. <br> - Phân biệt coverage và utilization của Savings Plans và Reserved Instances. Dùng tag phân bổ đã kích hoạt để xem chi phí theo dự án. <br> - Tra phí truyền dữ liệu và NAT Gateway theo loại sử dụng. <br> - **Final project:** Tiếp tục làm dự án. | 03/11/2026 | 03/11/2026 | - [Module 3 - 28: Cost visualization with AWS Cost Explorer](https://000034.awsfcaj.com/) |
| 3 | **Phân tích chi phí và hiệu năng với AWS Glue và Amazon Athena** <br> - Tìm hiểu cách Glue Crawler lập danh mục cho tệp Parquet mẫu từ Cost and Usage Report (CUR) trên S3 mà không cần chạy ETL. <br> - Dùng SQL trong Athena để phân tích chi phí theo tài khoản, dịch vụ, loại sử dụng và tag `CostCenter`, đồng thời tìm Reserved Instances chưa được sử dụng. <br> - Chỉ chọn các cột và phân vùng cần thiết để giảm lượng dữ liệu quét. <br> - **Final project:** Tiếp tục làm dự án. | 04/11/2026 | 04/11/2026 | - [Module 3 - 29: Cost and performance analysis with AWS Glue and Amazon Athena](https://000040.awsfcaj.com/) |
| 4 | **Di chuyển ứng dụng monolith TravelBuddy** <br> - Tìm hiểu cách CloudFormation tạo VPC, EC2 và RDS cho bài lab. Chuẩn bị cơ sở dữ liệu và kiểm tra ứng dụng Java trên Eclipse. <br> - Tìm hiểu cách triển khai TravelBuddy lên Elastic Beanstalk, cập nhật ứng dụng bằng EB CLI và kiểm tra API sau triển khai. | 05/11/2026 | 05/11/2026 | - [Module 4 - 1: Migrating the Monolith](https://000050.awsfcaj.com/) |
| 5 | **Tự động hóa phát hành ứng dụng** <br> - Theo dõi luồng phát hành TravelBuddy từ Git push qua CodePipeline và CodeBuild đến Elastic Beanstalk. Tìm hiểu cache Maven trên S3 và cách kiểm tra bản triển khai qua endpoint ứng dụng. <br> - Đọc cấu hình CodeStar trong bài lab cũ để xác định các bước cần cấu hình trực tiếp trong pipeline hiện nay. | 06/11/2026 | 06/11/2026 | - [Module 4 - 2: Configure app auto-release](https://000051.awsfcaj.com/) |
| 6 | **Microservice serverless và xử lý ảnh** <br> - Tìm hiểu cách sự kiện tải ảnh lên S3 kích hoạt Lambda tạo ảnh thu nhỏ. Tách bucket đầu ra hoặc lọc theo tiền tố đầu vào để tránh kích hoạt lặp. <br> - Xem cách AWS SAM đóng gói hàm và trigger, cùng cách một hàm Lambda riêng ghi metadata sách vào DynamoDB. Khi kiểm thử, đối chiếu ảnh đầu ra, log và bản ghi. | 07/11/2026 | 07/11/2026 | - [Module 4 - 3: Create a Microservice](https://000052.awsfcaj.com/) <br> - [Image Processing and Optimization on AWS](https://000078.awsfcaj.com/) |

### Kết quả đạt được tuần 8:

- Bắt đầu lên ý tưởng cho bài Project cuối kỳ.
- Tìm hiểu các dịch vụ mà nhóm sẽ dùng trong bài Project.
- Thiết kế kiến trúc hệ thống cho các dịch vụ AWS sẽ triển khai trong Project.
- Biết cách đối chiếu khuyến nghị chọn kích cỡ EC2 với số liệu tải thực tế trước khi thay đổi cấu hình.
- Biết cách xác định nguồn phát sinh chi phí AWS và đọc chỉ số coverage, utilization.
- Biết cách truy vết chi phí AWS đến từng dòng trong CUR để phân tích sâu hơn báo cáo tổng hợp trong Cost Explorer.
- Nắm luồng triển khai TravelBuddy lên Elastic Beanstalk, các bước CI/CD và cách phối hợp S3, Lambda và DynamoDB.
