---
title: "Phân phối web và bảo mật"
menuTitle: "Phân phối web và bảo mật"
weight: 7
chapter: false
pre: "<b> 5.7. </b> "
---



Tạo hosted zone và ủy quyền DNS trước khi xác thực ACM. Sau khi có hai đích CloudFront và ALB, quay lại Route 53 để hoàn thành Alias.

- [5.7.1. AWS Certificate Manager (ACM)](5.7.1-aws-certificate-manager/) — Cấp TLS cho tên miền frontend CloudFront và backend ALB.
- [5.7.2. Amazon CloudFront](5.7.2-amazon-cloudfront/) — Phân phối React SPA từ S3 qua HTTPS/cache và liên kết WAF.
- [5.7.3. Amazon Route 53](5.7.3-amazon-route-53/) — Bản ghi DNS trỏ hostname frontend về CloudFront và API về ALB.
- [5.7.4. AWS WAF (Frontend & Backend)](5.7.4-aws-waf/) — Web ACL xử lý request vào CloudFront và ALB (Count/Block theo từng rule).
