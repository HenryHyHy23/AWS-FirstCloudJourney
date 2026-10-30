---
title: "Báo cáo công việc Tuần 4"
date: 2026-10-05
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

- Hiểu phân quyền theo tag, permissions boundary và Identity Center.
- Nắm điều kiện dùng Systems Manager và quản lý hạ tầng bằng CloudFormation.

### Các công việc cần triển khai trong tuần này:

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 1 | **Phân quyền EC2 theo tag** <br> - Phân tích policy giới hạn EC2 theo tag và điều kiện khi tạo hoặc sửa tài nguyên. Phân biệt `aws:RequestTag` với tag hiện có; `aws:TagKeys` giới hạn key được phép. <br> - Tạo EC2 kèm tag cần `CreateTags`; bảo vệ quyền sửa tag phân quyền. Trust policy quy định ai được assume role; permissions policy quy định được làm gì. | 05/10/2026 | 05/10/2026 | - [Module 3 - 4: Managing EC2 Resource Tag Access with AWS IAM](https://000028.awsstudygroup.com/) <br> - [Docs: Tag-on-create authorization](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/supported-iam-actions-tagging.html) <br> - [Docs: IAM tag conditions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html) |
| 2 | **Vá lỗi và chạy lệnh bằng Systems Manager** <br> - Tìm hiểu Patch Manager và Run Command để vận hành nhiều máy chủ. Managed node cần SSM Agent, IAM role và kết nối endpoint. <br> - Patch baseline xác định bản vá; phân biệt `Scan`/`Scan and install`, kiểm tra compliance và reboot. Run Command chạy lệnh nhiều máy; xem output từng node, đặt concurrency và ngưỡng lỗi. | 06/10/2026 | 06/10/2026 | - [Module 3 - 5: Manage Patches and Run Commands on Multiple Servers with AWS Systems Manager](https://000031.awsstudygroup.com/) <br> - [Docs: Patch Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager.html) <br> - [Docs: Command concurrency](https://docs.aws.amazon.com/systems-manager/latest/userguide/send-commands-multiple.html) |
| 3 | **Truy cập máy chủ qua Session Manager** <br> - Tìm hiểu kết nối EC2 public/private, lưu log và chuyển tiếp cổng bằng Session Manager. Session Manager dùng HTTPS outbound, không cần mở SSH/RDP inbound. <br> - Private instance cần endpoint, private DNS và rule TCP 443 phù hợp. Ghi log S3 cần quyền và đường mạng riêng; nội dung port forwarding không được ghi trong session log. | 07/10/2026 | 07/10/2026 | - [Module 3 - 6: Working with AWS Systems Manager - Session Manager](https://000058.awsstudygroup.com/) <br> - [Giới hạn ghi log phiên](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-logging.html) <br> - [Docs: SSM VPC endpoints](https://docs.aws.amazon.com/systems-manager/latest/userguide/setup-create-vpc.html) |
| 4 | **Quản lý hạ tầng bằng CloudFormation** <br> - Tìm hiểu quy trình từ khai báo template đến kiểm tra và quản lý thay đổi stack. Template YAML/JSON khai báo tài nguyên; stack quản lý vòng đời hạ tầng. <br> - Dùng cfn-lint, change set; drift detection phát hiện nhưng không tự sửa sai lệch. Custom resource Lambda xử lý Create/Update/Delete; StackSets triển khai nhiều account/Region. | 08/10/2026 | 08/10/2026 | - [Module 3 - 7: AWS CloudFormation](https://000037.awsstudygroup.com/) <br> - [Docs: Change sets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-changesets.html) <br> - [Docs: Custom resources](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-custom-resources.html) <br> - [Docs: Drift detection](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-stack-drift.html) |
| 5 | **Danh tính tập trung với IAM Identity Center** <br> - Tìm hiểu cách tổ chức nhiều account và cấp quyền cho nhân sự theo nhóm. Organizations/OU tổ chức account; SCP giới hạn quyền, không tự cấp quyền. <br> - Identity Center dùng group, account assignment và permission set để cấp role. CLI dùng `aws configure sso`/`aws sso login`; customer-managed policy cần tồn tại đúng tên/path ở account đích. | 09/10/2026 | 09/10/2026 | - [Module 3 - 8: Using AWS IAM Identity Center for Robust Identity Management](https://000012.awsstudygroup.com/) <br> - [Docs: Permission sets](https://docs.aws.amazon.com/singlesignon/latest/userguide/permissionsetsconcept.html) <br> - [Docs: Customer-managed policies](https://docs.aws.amazon.com/singlesignon/latest/userguide/howtocmp.html) <br> - [Docs: CLI SSO sessions](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html) |
| 6 | **Giới hạn quyền bằng permissions boundary** <br> - Phân tích quyền có hiệu lực khi IAM user có policy EC2 rộng nhưng bị giới hạn Region. Permissions boundary giới hạn quyền identity-based của user/role, không gắn cho group và không tự cấp quyền. <br> - Boundary của bài chỉ cho EC2 tại `ap-southeast-1`; Sydney bị Implicit Deny. Bảo vệ quyền sửa/gỡ boundary; xét thêm các policy áp dụng khi đánh giá truy cập. | 10/10/2026 | 10/10/2026 | - [Module 3 - 9: Limitation of User Rights with IAM Permission Boundary](https://000030.awsstudygroup.com/) <br> - [Permissions boundaries (AWS IAM)](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html) <br> - [Docs: Policy evaluation](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html) |

### Kết quả đạt được tuần 4:

- Phân biệt tag condition, trust policy, boundary và SCP.
- Hiểu điều kiện kết nối/ghi log SSM và vai trò của change set, drift detection, permission set.
