---
title: "Báo cáo công việc Tuần 3"
date: 2026-09-28
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

- Dùng AWS CLI, tìm hiểu VM/database migration.
- Hiểu lập lịch EC2, dashboard Grafana và quản lý tài nguyên bằng tag.

### Các công việc cần triển khai trong tuần này:

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 1 | **Làm việc với AWS CLI** <br> - Tìm hiểu cách chạy lệnh, lọc đầu ra và xử lý lỗi AWS CLI. Kiểm tra profile, account, Region và caller identity trước khi chạy AWS CLI. <br> - Service filter lọc phía AWS; `--query` lọc/định dạng phía client. Chú ý phân trang. Dùng JSON, kiểm tra exit code và ưu tiên credential tạm thời. | 28/09/2026 | 28/09/2026 | - [Module 1 - 15: Làm quen với AWS CLI](https://000011.awsstudygroup.com/) <br> - [Docs: CLI troubleshooting](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-troubleshooting.html) <br> - [Docs: CLI filtering](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-filter.html) |
| 2 | **Di chuyển VM bằng Import/Export** <br> - Tìm hiểu quy trình đưa image máy ảo vào AWS và các giới hạn khi export. Kiểm tra OS, định dạng đĩa, boot mode, driver và bản quyền trước khi import VM. <br> - Role `vmimport` cần trust policy và quyền S3; theo dõi tác vụ tới khi hoàn tất. Import xong vẫn cần kiểm tra boot và ứng dụng; export có giới hạn riêng. | 29/09/2026 | 29/09/2026 | - [Module 2 - 1: VM Import/Export](https://000014.awsstudygroup.com/) <br> - [Docs: VM import requirements](https://docs.aws.amazon.com/vm-import/latest/userguide/prerequisites.html) <br> - [Docs: VM export limits](https://docs.aws.amazon.com/vm-import/latest/userguide/limits-image-export.html) |
| 3 | **Chuyển đổi và di chuyển database** <br> - Phân biệt phần chuyển cấu trúc database với phần di chuyển dữ liệu. SCT đánh giá và chuyển schema; đối tượng không tương thích cần sửa thủ công. <br> - DMS full load chuyển dữ liệu ban đầu; CDC đồng bộ thay đổi, có thể bị trễ. Trước cutover, kiểm tra dữ liệu, ứng dụng, độ trễ và phương án rollback. | 30/09/2026 | 30/09/2026 | - [Module 2 - 2: Database Schema Conversion & Migration](https://000043.awsstudygroup.com/) <br> - [Docs: SCT assessment reports](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_AssessmentReport.html) <br> - [Docs: DMS CDC](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Task.CDC.html) |
| 4 | **Lập lịch bật/tắt EC2** <br> - Tìm hiểu luồng Scheduler gọi Lambda để bật/tắt EC2 theo giờ làm việc. Dùng cron và múi giờ rõ ràng; `rate(8 hours)` không có nghĩa chạy lúc 08:00. <br> - Lambda lọc EC2 theo tag/trạng thái; tách quyền EC2 và quyền Scheduler invoke. Stop EC2 vẫn còn phí EBS; thông báo sau API chưa xác nhận trạng thái cuối. | 01/10/2026 | 01/10/2026 | - [Module 3 - 1: Optimize EC2 cost with Lambda](https://000022.awsstudygroup.com/) <br> - [Docs: Scheduler expressions](https://docs.aws.amazon.com/scheduler/latest/UserGuide/schedule-types.html) <br> - [Docs: EC2 lifecycle](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-lifecycle.html) |
| 5 | **Dashboard Grafana với CloudWatch** <br> - Tìm hiểu cách Grafana lấy dữ liệu CloudWatch và hiển thị dashboard. Grafana truy vấn CloudWatch; kiểm tra Region, InstanceId, statistic và period khi thiếu dữ liệu. <br> - Grafana trên EC2 có thể dùng instance role với quyền đọc cần thiết. Đối chiếu Explore với panel; cấu hình alert và nơi nhận thông báo. | 02/10/2026 | 02/10/2026 | - [Module 3 - 2: Getting started with Grafana basic](https://000029.awsstudygroup.com/) <br> - [Docs: Grafana AWS authentication](https://grafana.com/docs/grafana/latest/datasources/aws-cloudwatch/aws-authentication/) <br> - [Docs: CloudWatch data source](https://grafana.com/docs/grafana/latest/datasources/aws-cloudwatch/) |
| 6 | **Quản lý tag và Resource Groups** <br> - Tìm hiểu cách dùng tag để kiểm kê, chọn tài nguyên và hỗ trợ tự động hóa. Chuẩn hóa tag Owner, Service, Environment; tag phân biệt hoa/thường. <br> - Resource Groups chọn tài nguyên theo tag; đổi tag có thể đổi thành viên nhóm. Group không tạo ranh giới IAM/mạng; cần bảo vệ tag dùng để phân quyền. | 03/10/2026 | 03/10/2026 | - [Module 3 - 3: Manage Resources Using Tags and Resource Groups](https://000027.awsstudygroup.com/) <br> - [Docs: Resource Group queries](https://docs.aws.amazon.com/ARG/latest/userguide/gettingstarted-query.html) <br> - [Docs: Tagging practices](https://docs.aws.amazon.com/tag-editor/latest/userguide/best-practices-and-strats.html) |

### Kết quả đạt được tuần 3:

- Phân biệt chuyển schema, đồng bộ dữ liệu và kiểm chứng ứng dụng sau migration.
- Hiểu lịch EC2, cấu hình truy vấn Grafana và giới hạn của tag/Resource Groups.
