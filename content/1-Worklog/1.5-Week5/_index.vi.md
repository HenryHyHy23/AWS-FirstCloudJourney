---
title: "Báo cáo công việc Tuần 5"
date: 2026-10-12
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

- Hiểu IAM Role/Condition và cách đọc finding Security Hub CSPM.
- Nắm rule/log AWS WAF, quyền SSE-KMS và quy trình backup/restore.

### Các công việc cần triển khai trong tuần này:

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 1 | **IAM Role và điều kiện truy cập** <br> - Tìm hiểu phân quyền EC2/RDS và giới hạn việc assume role theo IP, thời gian. Phân quyền EC2/RDS qua user/group; tách trust policy và permissions policy của role. <br> - STS cấp credential tạm thời; phiên assume role dùng quyền của role. Điều kiện IP/thời gian trong trust policy chỉ kiểm soát AssumeRole. `Z` là UTC; hết khoảng thời gian không tự kết thúc phiên đã cấp. | 12/10/2026 | 12/10/2026 | - [Module 3 - 10: IAM Role & Condition](https://000044.awsstudygroup.com/) |
| 2 | **Đánh giá bảo mật với Security Hub CSPM** <br> - Tìm hiểu cách tổng hợp cảnh báo và kiểm tra cấu hình theo tiêu chuẩn bảo mật. Security Hub CSPM tổng hợp finding và kiểm tra cấu hình theo FSBP, CIS, PCI DSS. <br> - Kiểm tra phạm vi AWS Config theo account/Region và dữ liệu đánh giá trước khi đọc score. Ưu tiên finding theo severity; disable control cần lý do và không sửa lỗi tài nguyên. | 13/10/2026 | 13/10/2026 | - [Module 3 - 11: Getting Started with AWS Security Hub](https://000018.awsfcaj.com/) |
| 3 | **Bảo vệ ứng dụng bằng AWS WAF** <br> - Tìm hiểu managed/custom rules để kiểm tra request tới ứng dụng Juice Shop. Tạo Web ACL cho Juice Shop, dùng managed Core/SQL và custom rule. <br> - Priority nhỏ chạy trước; Allow/Block kết thúc đánh giá, Count tiếp tục qua rule sau. CloudFront Web ACL quản lý tại `us-east-1`; WCU phản ánh năng lực rule. Log redaction không che sampled requests. | 14/10/2026 | 14/10/2026 | - [Module 3 - 12: AWS Web Application Firewall](https://000026.awsfcaj.com/) |
| 4 | **Kiểm thử và tinh chỉnh AWS WAF** <br> - Phân tích request khớp/không khớp để tránh chặn nhầm người dùng hợp lệ. Size constraint kiểm tra byte; `username GT 0` chỉ có nghĩa giá trị không rỗng. <br> - Kiểm tra cặp header chocolate/nuts OR cặp query banana/sauce; mỗi cặp phải đủ hai điều kiện. Thử Count, xem rule khớp/log và false positive trước Block. Firehose cần Direct PUT, tiền tố `aws-waf-logs-`, đúng Region. | 15/10/2026 | 15/10/2026 | - [Module 3 - 13: Using AWS WAF](https://000026.awsfcaj.com/3-useawswaf/) |
| 5 | **Mã hóa S3 với AWS KMS** <br> - Tìm hiểu cách mã hóa object, cấp quyền giải mã và theo dõi truy cập. SSE-KMS dùng data key mã hóa object; khóa KMS bảo vệ data key. <br> - Đọc cần `s3:GetObject` và `kms:Decrypt`; key policy/IAM quyết định quyền dùng khóa. Bật S3 data events, dùng Athena truy vấn CloudTrail. Presigned URL chịu giới hạn quyền người ký, thời hạn URL và credential. | 16/10/2026 | 16/10/2026 | - [Module 3 - 14: Encryption at Rest with AWS KMS](https://000033.awsfcaj.com/) |
| 6 | **Sao lưu và kiểm thử khôi phục** <br> - Tìm hiểu backup plan và luồng tự động khôi phục để đánh giá khả năng phục hồi. RPO là khoảng thời gian mất dữ liệu tối đa chấp nhận được; RTO là thời gian gián đoạn tối đa. <br> - Backup plan quy định lịch/retention; resource assignment chọn tài nguyên, vault chứa recovery point. Luồng Backup → SNS → Lambda tự động restore. Kiểm tra ứng dụng, log và dọn tài nguyên thử; xử lý lỗi/thông báo trùng. | 17/10/2026 | 17/10/2026 | - [Module 3 - 15: Deploy AWS Backup to the System](https://000013.awsfcaj.com/) |

### Kết quả đạt được tuần 5:

- Phân biệt điều kiện assume role với quyền API, control thất bại với thiếu dữ liệu đánh giá.
- Hiểu Count/Block, logic AND/OR và cách dùng log tìm rule chặn.
- Phân biệt quyền S3/KMS; backup hoàn tất cần thêm kiểm chứng restore và ứng dụng.
