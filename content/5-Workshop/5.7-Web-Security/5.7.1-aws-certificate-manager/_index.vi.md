---
title: "Cấp chứng chỉ cho website và API"
menuTitle: "AWS Certificate Manager (ACM)"
weight: 1
chapter: false
pre: "<b> 5.7.1. </b> "
---

1. **Chọn N. Virginia (us-east-1) → ACM → Request a certificate → Request a public certificate.**

{{< btbfigure file="aws-certificate-manager-step-1-request-a-public-certificate.webp" caption="Chọn N. Virginia (us-east-1) → ACM → Request a certificate → Request a public certificate." step="yes" >}}

2. **Nhập tên miền storefront → DNS validation → Request.** Thêm cả apex và www nếu phục vụ cả hai. Tại Domains, tạo validation CNAME trong hosted zone đang authoritative.

{{< btbfigure file="aws-certificate-manager-step-2-setup-certificate.webp" caption="Tên miền và DNS validation cho chứng chỉ frontend." step="yes" >}}

3. **Mở Certificates tại N. Virginia.** Đợi trạng thái Issued, kiểm tra các tên miền khớp với CloudFront alternate domain names.

{{< btbfigure file="aws-certificate-manager-step-3-issued-virginia.webp" caption="Chứng chỉ N. Virginia ở trạng thái Issued." step="yes" >}}

4. **Chuyển sang Singapore → Request public certificate cho hostname API.** Lặp lại DNS validation, dùng tên được listener ALB phục vụ.

{{< btbfigure file="aws-certificate-manager-step-2-setup-certificate-singapore.webp" caption="Yêu cầu chứng chỉ tại Singapore." step="yes" >}}

5. **Mở Certificates tại Singapore → kiểm tra Issued.** Chọn cert này cho HTTPS:443 ở ALB. Giữ CNAME để ACM gia hạn.

{{< btbfigure file="aws-certificate-manager-step-3-issued-singapore.webp" caption="Chứng chỉ Singapore ở trạng thái Issued." step="yes" >}}

**Kiểm tra kết quả.** Nếu Pending validation, kiểm tra nameserver của domain và DNS CNAME. Nếu cert đã Issued nhưng không thấy khi gắn, kiểm tra Region và account.