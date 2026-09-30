---
title: "Báo cáo công việc Tuần 1"
date: 2026-09-14
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

### Mục tiêu tuần 1:

- Làm quen với nền tảng AWS Cloud và giao diện AWS Management Console.
- Tìm hiểu các dịch vụ cơ bản của AWS và thực hành các bài lab nền tảng trong AWS Free Tier.

### Các công việc cần triển khai trong tuần này:

| Ngày | Công việc                                                                                                                                                                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                                                                                                                                                                                                  |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | - Làm quen với AWS Management Console <br> - Tìm hiểu AWS Free Tier, các gói credit và mô hình tính phí <br> - Tìm hiểu AWS Billing & Cost Management <br> - **Thực hành:** <br> &emsp; + Tạo và cấu hình tài khoản AWS Free Tier <br> &emsp; + Thiết lập AWS Budgets để theo dõi chi phí                                                   | 14/09/2026   | 15/09/2026      | - [Module 1 - 1: Tạo tài khoản AWS mới](https://000001.awsstudygroup.com/) <br> - [Module 1 - 2: Quản lý chi phí với AWS Budgets](https://000007.awsstudygroup.com/) <br> - [Module 1 - 3: Gửi yêu cầu hỗ trợ với AWS Support](https://aws.amazon.com/support/) |
| 2    | - Tìm hiểu AWS Identity and Access Management (IAM) <br> &emsp; + Users, Groups, Roles, Policies <br> &emsp; + Nguyên tắc Least Privilege <br> - **Thực hành:** <br> &emsp; + Tạo IAM user, group và gán policy <br> &emsp; + Thực hành AWS CLI cơ bản                                                                                      | 15/09/2026   | 16/09/2026      | - [Module 1 - 4: Quản lý truy cập với AWS IAM](https://000002.awsstudygroup.com/)                                                                                                                                                                               |
| 3    | - Tìm hiểu Amazon Virtual Private Cloud (VPC): <br> &emsp; + CIDR, Subnet (public/private) <br> &emsp; + Route Table, Internet Gateway, NAT Gateway <br> &emsp; + Security Group, Network ACL <br> &emsp; + Site-to-Site VPN <br> - **Thực hành:** <br> &emsp; + Tạo VPC và cấu hình các thành phần mạng                                    | 16/09/2026   | 17/09/2026      | - [Module 1 - 5: Triển khai hạ tầng mạng với Amazon VPC](https://000003.awsstudygroup.com/)                                                                                                                                                                     |
| 4    | - Tìm hiểu Amazon EC2: <br> &emsp; + Instance Types, AMI, EBS <br> &emsp; + Key Pair, Security Group <br> - **Thực hành:** <br> &emsp; + Tạo và cấu hình EC2 instance <br> &emsp; + Kết nối SSH vào EC2 <br> &emsp; + Quản lý vòng đời instance                                                                                             | 17/09/2026   | 18/09/2026      | - [Module 1 - 6: Triển khai ứng dụng trên Amazon EC2](https://000004.awsstudygroup.com/)                                                                                                                                                                        |
| 5    | - Tìm hiểu cách cấp quyền cho ứng dụng truy cập dịch vụ AWS <br> - Hiểu rủi ro bảo mật khi hardcode Access Key trong code <br> - Tìm hiểu Instance Metadata và AWS STS <br> - **Thực hành:** <br> &emsp; + Kết nối Python app trên EC2 với S3 bằng Access Key <br> &emsp; + Kết nối bằng IAM Role gán vào EC2                               | 18/09/2026   | 19/09/2026      | - [Module 1 - 7: Cấp quyền cho ứng dụng thông qua IAM Role](https://000048.awsstudygroup.com/)                                                                                                                                                                  |
| 6    | - Tìm hiểu AWS CloudShell và môi trường dòng lệnh trên trình duyệt <br> - Tìm hiểu phân quyền `rwx` trên Linux <br> - **Thực hành:** <br> &emsp; + Các lệnh Linux cơ bản: `pwd`, `cd`, `mkdir`, `touch`, `cat`, `vi`, `ls -la` <br> &emsp; + Chạy lệnh AWS CLI trong CloudShell <br> &emsp; + Tải lên/xuống tệp giữa máy tính và CloudShell | 19/09/2026   | 20/09/2026      | - [Module 1 - 8: Bắt đầu với AWS CloudShell](https://000006.awsstudygroup.com/)                                                                                                                                                                                 |

### Kết quả đạt được tuần 1:

- Tạo thành công tài khoản AWS Free Tier, làm quen với AWS Management Console và thiết lập AWS Budgets để kiểm soát chi phí.

- Nắm được các thành phần cốt lõi của IAM:
  - Users, Groups, Policies, Roles
  - Nguyên tắc phân quyền tối thiểu (Least Privilege)
  - Thực hành tạo IAM user, group và gán quyền truy cập

- Hiểu kiến trúc mạng cơ bản với Amazon VPC:
  - Subnet (public/private), Route Table, Internet Gateway, NAT Gateway
  - Security Group và Network ACL
  - Tự tay tạo VPC và kết nối SSH vào EC2 instance thành công

- Nhận thức được rủi ro bảo mật của việc lưu cứng Access Key trong code, biết cách gán IAM Role cho EC2 để ứng dụng upload file lên S3 bằng credential tạm thời qua Instance Metadata và AWS STS.

- Làm quen với AWS CloudShell, thực hành các lệnh Linux cơ bản và chạy AWS CLI trực tiếp trên trình duyệt mà không cần cài đặt thêm gì trên máy cá nhân.
