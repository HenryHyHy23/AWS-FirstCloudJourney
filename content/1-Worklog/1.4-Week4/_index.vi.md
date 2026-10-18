---
title: "Báo cáo công việc Tuần 4"
date: 2026-10-05
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

- Nắm IAM Least Privilege, điều kiện theo tag/Region và AssumeRole cho EC2.
- Tìm hiểu quản lý node tập trung, tuân thủ bản vá và chạy lệnh hàng loạt bằng Systems Manager.
- Hiểu truy cập riêng tư, ghi log và RDP port forwarding với Session Manager.
- Tìm hiểu template CloudFormation, vòng đời stack, kiểm tra template, Custom Resources, StackSets và Drift Detection.

### Các công việc cần triển khai trong tuần này:

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 1 | - Tìm hiểu IAM Least Privilege và phân quyền EC2 theo `Team=Alpha`, Region (`us-east-1`, `us-west-1`) <br> - Nắm 5 nhóm policy: đọc, gắn tag khi tạo, gắn tag tài nguyên có sẵn, tạo và quản lý instance; phân biệt tag yêu cầu, tag tài nguyên và tag key được phép <br> - Hiểu Trust Policy, MFA, AssumeRole và các trường hợp cho phép/từ chối theo điều kiện | 05/10/2026 | 05/10/2026 | - [Module 3 - 4: Managing EC2 Resource Tag Access with AWS IAM](https://000028.awsstudygroup.com/) |
| 2 | - Tìm hiểu Systems Manager: Vận hành tập trung, Fleet Manager, managed node và quyền `AmazonSSMManagedInstanceCore` <br> - Nắm Patch Manager: Quét/cài bản vá, trạng thái tuân thủ và tùy chọn reboot <br> - Tìm hiểu Run Command: Chạy `AWS-RunPowerShellScript` (`net user`) trên nhiều Windows node, xem output/lỗi từng node và tùy chọn lưu output vào S3 | 06/10/2026 | 06/10/2026 | - [Module 3 - 5: Manage Patches and Run Commands on Multiple Servers with AWS Systems Manager](https://000031.awsstudygroup.com/) |
| 3 | - Tìm hiểu Session Manager: Quyền IAM/SSM role, truy cập EC2 public/private mà không mở inbound SSH/RDP <br> - Nắm Interface Endpoint, private DNS, HTTPS 443 và định tuyến S3 Gateway Endpoint để gửi log qua mạng riêng <br> - Phân biệt Session history với log phiên shell trong S3; tìm hiểu CLI/Session Manager Plugin và RDP forwarding (`localhost:9999` → `3389`) | 07/10/2026 | 07/10/2026 | - [Module 3 - 6: Working with AWS Systems Manager - Session Manager](https://000058.awsstudygroup.com/) <br> - [Giới hạn ghi log phiên](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-logging.html) |
| 4 | - Tìm hiểu CloudFormation IaC: Template YAML/JSON, Parameters/Resources/Outputs, intrinsic function (`Ref`, `GetAtt`, `Sub`) và quan hệ phụ thuộc <br> - Nắm kiểm tra/triển khai stack qua CloudShell/CLI với `cfn-lint`, `taskcat`; Custom Resources dùng Lambda và SSM Parameter Store <br> - Tìm hiểu Mappings, StackSets qua nhiều account/Region và Drift Detection (`IN_SYNC`/`DRIFTED`) | 08/10/2026 | 08/10/2026 | - [Module 3 - 7: AWS CloudFormation](https://000037.awsstudygroup.com/) |

### Kết quả đạt được tuần 4:

- **IAM:** Phân biệt tag yêu cầu/tài nguyên; nắm điều kiện Region/tag key, Trust Policy, MFA và AssumeRole.
- **Systems Manager:** Hiểu quyền IAM cho managed node, quét/cài bản vá, tuân thủ và Run Command với kết quả từng node.
- **Session Manager:** Nắm truy cập qua endpoint riêng, log shell trong S3 và RDP tunnel; không ghi nội dung phiên port forwarding.
- **CloudFormation:** Hiểu template tái sử dụng, intrinsic function, thứ tự phụ thuộc, Lambda Custom Resources với SSM Parameter Store, Mappings, StackSets nhiều account/Region và Drift Detection.
