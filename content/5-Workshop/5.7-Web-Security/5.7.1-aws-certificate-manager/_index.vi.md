---
title: "Cấp chứng chỉ cho website và API"
menuTitle: "AWS Certificate Manager (ACM)"
weight: 1
chapter: false
pre: "<b> 5.7.1. </b> "
---

1. **Chọn N. Virginia (us-east-1) → ACM → Request a certificate → Request a public certificate.**

![Chọn N. Virginia (us-east-1) → ACM → Request a certificate → Request a public certificate.](/images/5-Workshop/5.7-Web-Security/5.7.1-aws-certificate-manager/aws-certificate-manager-step-1-request-a-public-certificate.webp)

2. **Nhập tên miền storefront → DNS validation → Request.** Thêm cả apex và www nếu phục vụ cả hai. Tại Domains, tạo validation CNAME trong hosted zone đang authoritative.

![Tên miền và DNS validation cho chứng chỉ frontend.](/images/5-Workshop/5.7-Web-Security/5.7.1-aws-certificate-manager/aws-certificate-manager-step-2-setup-certificate.webp)

3. **Mở Certificates tại N. Virginia.** Đợi trạng thái Issued, kiểm tra các tên miền khớp với CloudFront alternate domain names.

![Chứng chỉ N. Virginia ở trạng thái Issued.](/images/5-Workshop/5.7-Web-Security/5.7.1-aws-certificate-manager/aws-certificate-manager-step-3-issued-virginia.webp)

4. **Chuyển sang Singapore → Request public certificate cho hostname API.** Lặp lại DNS validation, dùng tên được listener ALB phục vụ.

![Yêu cầu chứng chỉ tại Singapore.](/images/5-Workshop/5.7-Web-Security/5.7.1-aws-certificate-manager/aws-certificate-manager-step-2-setup-certificate-singapore.webp)

5. **Mở Certificates tại Singapore → kiểm tra Issued.** Chọn cert này cho HTTPS:443 ở ALB. Giữ CNAME để ACM gia hạn.

![Chứng chỉ Singapore ở trạng thái Issued.](/images/5-Workshop/5.7-Web-Security/5.7.1-aws-certificate-manager/aws-certificate-manager-step-3-issued-singapore.webp)

**Kiểm tra kết quả.** Nếu Pending validation, kiểm tra nameserver của domain và DNS CNAME. Nếu cert đã Issued nhưng không thấy khi gắn, kiểm tra Region và account.