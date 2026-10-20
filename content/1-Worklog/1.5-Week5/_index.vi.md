---
title: "Báo cáo công việc Tuần 5"
date: 2026-10-12
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

- Phân biệt quyền user/group, trust policy và quyền của phiên assume role.
- Hiểu credential tạm thời do STS cấp, đánh giá truy cập role theo ngữ cảnh request.
- Diễn giải điều kiện IP/thời gian, múi giờ và phạm vi kiểm soát lúc assume role so với khi gọi dịch vụ.

### Các công việc cần triển khai trong tuần này:

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 1 | - **Danh tính và phạm vi quyền:** So sánh policy EC2/RDS gắn trực tiếp với quyền nhận qua group. Group tổ chức user, không phải principal tự gửi request. Policy FullAccess cấp dịch vụ minh họa phân chia trách nhiệm nhưng vẫn rộng hơn nhiều công việc thông thường. <br> - **Luồng AssumeRole:** Trust policy xác định ai được assume role; permissions policy xác định hành động dịch vụ được phép. STS trả access key ID, secret access key, session token và thời điểm hết hạn. Phiên role dùng quyền của role thay vì cộng thêm vào quyền sẵn có của user. <br> - **Điều kiện request:** `IpAddress` với `aws:SourceIp` kiểm tra public IP mà AWS quan sát, có thể là địa chỉ NAT/VPN. `DateGreaterThan`/`DateLessThan` với `aws:CurrentTime` tạo khoảng thời gian cố định; hậu tố `Z` là UTC, không phải giờ Việt Nam. <br> - **Phạm vi kiểm soát:** Điều kiện IP/thời gian trong trust policy kiểm soát `sts:AssumeRole`. Chúng không tự giới hạn mọi API EC2/RDS sau đó hoặc vô hiệu credential đã cấp; muốn kiểm soát việc tiếp tục sử dụng cần điều kiện phù hợp trong permissions policy hoặc kiểm soát cấp tổ chức. | 12/10/2026 | 12/10/2026 | - [Module 3 - 10: IAM Role & Condition](https://000044.awsstudygroup.com/) <br> - [AWS Docs: AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) <br> - [AWS Docs: Condition keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) <br> - [AWS Security Blog: Role trust policies](https://aws.amazon.com/blogs/security/how-to-use-trust-policies-with-iam-roles/) |

### Kết quả đạt được tuần 5:

- **Least privilege:** Phân biệt tách trách nhiệm quản trị EC2/RDS với chỉ cấp hành động cần thiết. Managed policy rộng là điểm khởi đầu để phân tích, chưa chứng minh đã đạt quyền tối thiểu.
- **Phân tích trust:** User cùng account được role tin cậy trực tiếp có thể được phép assume mà không cần Allow riêng trong identity policy. Không có quyền dịch vụ trực tiếp khác với bị Explicit Deny áp dụng.
- **Truy cập có điều kiện:** Phân biệt lỗi lúc assume với lỗi API sau đó. Khoảng thời gian trong trust policy không phải lịch lặp hằng ngày và không bảo đảm phiên đang tồn tại kết thúc ngay tại mốc đó.
