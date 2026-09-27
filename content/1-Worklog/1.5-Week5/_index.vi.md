---
title: "Worklog Tuần 5"
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

- Hoàn thành Lab 10, 19
- Biên dịch Blog của AWS

### Các công việc cần triển khai trong tuần này:

| Day | Task                | Start Date | Completion Date | Reference Sources                                                                                                                                                   |
| --- | ------------------- | ---------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2   | - Hoàn thành Lab 10 | 06/10/2025 | 06/10/2025      | [Lab10](https://000010.awsstudygroup.com/)                                                                                                                          |
| 3   | - Hoàn thành Lab 19 | 07/10/2025 | 07/10/2025      | [Lab19](https://000019.awsstudygroup.com/)                                                                                                                          |
| 4   | - Dịch Blog 1       | 08/10/2025 | 08/10/2025      | [Blog 1](https://aws.amazon.com/blogs/machine-learning/integrate-tokenization-with-amazon-bedrock-guardrails-for-secure-data-handling/)                             |
| 5   | - Dịch Blog 2       | 09/10/2025 | 09/10/2025      | [Blog 2](https://aws.amazon.com/blogs/machine-learning/running-deep-research-ai-agents-on-amazon-bedrock-agentcore/)                                                |
| 6   | - Dịch Blog 3       | 10/10/2025 | 10/10/2025      | [Blog 3](https://aws.amazon.com/blogs/training-and-certification/introducing-aws-card-clash-mobile-learn-aws-architecture-through-strategic-gameplay/)              |

### Kết quả đạt được tuần 5:

- Hoàn thành Lab 10 với các kết quả sau:

  - Được giới thiệu tổng quan về dịch vụ AWS CloudFormation.
  - Thực hành tạo template, chỉnh sửa Security Group (SG), và truy cập EC2 thông qua Remote Desktop Gateway Server (RDGW).
  - Thực hành cấu hình Directory Service.

- Tìm hiểu các chức năng của Route 53:

  - Thiết lập Outbound Endpoint cho Route 53.
  - Tạo DNS Resolver Rules.
  - Cấu hình Inbound Endpoint cho Route 53.
  - Kiểm tra và xác nhận kết nối hoạt động thành công sau khi cấu hình.
  - Thiết lập thành công kết nối giữa hệ thống on-premises và môi trường AWS thông qua Directory Service và Remote Desktop Gateway Server.

- Hoàn thành Lab 19 với các kết quả sau:

  - Hiểu được cơ chế VPC hoạt động độc lập và mục tiêu của VPC Peering để cho phép hai VPC giao tiếp an toàn qua địa chỉ private.
  - Thiết lập thành công kết nối VPC Peering giữa hai VPC, cho phép định tuyến traffic trực tiếp mà không đi qua Internet công cộng.
  - Thực hành cấu hình Network ACL như một lớp firewall stateless ở cấp độ subnet và cập nhật rule để cho phép lưu lượng cần thiết.
  - Cập nhật Route Tables để hỗ trợ giao tiếp hai chiều giữa hai VPC.
  - Cấu hình Cross-Peer DNS giúp các tài nguyên ở mỗi VPC phân giải DNS của nhau thông qua kết nối Peering.
  - Hoàn tất toàn bộ quy trình: chuẩn bị → cấu hình Network ACL → tạo kết nối Peering → cập nhật route tables → thiết lập Cross-Peer DNS.
