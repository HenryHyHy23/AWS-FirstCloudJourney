---
title: "Báo cáo công việc Tuần 3"
date: 2026-09-28
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

- Nắm cấu hình AWS CLI và cách quản lý dịch vụ.
- Tìm hiểu quy trình VM Import/Export giữa VMware và EC2.
- Hiểu chuyển đổi schema với AWS SCT và migration dữ liệu với AWS DMS.
- Tìm hiểu tối ưu chi phí EC2 bằng Lambda và EventBridge Scheduler.
- Hiểu dashboard Grafana và tích hợp CloudWatch.
- Nắm tổ chức tài nguyên bằng tag và Resource Groups.

### Các công việc cần triển khai trong tuần này:

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 1 | - Tìm hiểu AWS CLI: Credential, profile, Region và định dạng output <br> - Nắm các thao tác CLI cho S3, SNS, IAM, VPC và EC2 | 28/09/2026 | 28/09/2026 | - [Module 1 - 15: Làm quen với AWS CLI](https://000011.awsstudygroup.com/) |
| 2 | - Tìm hiểu VM Import/Export: Migration, backup, S3 và role `vmimport` <br> - Nắm quy trình VMware → VMDK → S3 → AMI → EC2 và export ngược | 29/09/2026 | 29/09/2026 | - [Module 2 - 1: VM Import/Export](https://000014.awsstudygroup.com/) |
| 3 | - Tìm hiểu AWS SCT: Chuyển đổi schema/code object, xử lý thủ công object không được hỗ trợ <br> - Tìm hiểu AWS DMS: Migration dữ liệu, đồng bộ transaction và giảm downtime | 30/09/2026 | 30/09/2026 | - [Module 2 - 2: Database Schema Conversion & Migration](https://000043.awsstudygroup.com/) |
| 4 | - Tìm hiểu Lambda/Boto3, quyền IAM, chọn instance theo tag và lịch rate/cron của EventBridge <br> - Nắm cơ chế start/stop EC2, thông báo Slack và Savings Plans cho workload chạy liên tục | 01/10/2026 | 01/10/2026 | - [Module 3 - 1: Optimize EC2 cost with Lambda](https://000022.awsstudygroup.com/) |
| 5 | - Tìm hiểu Grafana: Data source, dashboard, Explore và alerting <br> - Nắm tích hợp CloudWatch và query `CPUUtilization` của EC2 | 02/10/2026 | 02/10/2026 | - [Module 3 - 2: Getting started with Grafana basic](https://000029.awsstudygroup.com/) |
| 6 | - Tìm hiểu Tags: Metadata key/value theo Owner, Service và Environment <br> - Nắm lọc tag qua Console/CLI, Resource Groups cùng Region theo tag/CloudFormation và ví dụ `EnvTest` | 03/10/2026 | 03/10/2026 | - [Module 3 - 3: Manage Resources Using Tags and Resource Groups](https://000027.awsstudygroup.com/) |

### Kết quả đạt được tuần 3:

- **AWS CLI:** Nắm profile, credential, định dạng output và lệnh quản lý dịch vụ.
- **VM Import/Export:** Hiểu import VM thành AMI, triển khai EC2 và export qua S3.
- **AWS SCT & DMS:** Hiểu chuyển đổi schema và đồng bộ dữ liệu để giảm downtime.
- **Lambda:** Nắm start/stop EC2 theo lịch, quyền IAM và cơ chế thông báo.
- **Grafana:** Hiểu data source CloudWatch, query CPU và dashboard.
- **Tags & Resource Groups:** Nắm lọc tag và nhóm tài nguyên theo query.
