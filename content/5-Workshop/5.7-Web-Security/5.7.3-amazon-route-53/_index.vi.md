---
title: "Ủy quyền DNS từ Mắt Bão sang Route 53"
menuTitle: "Amazon Route 53"
weight: 3
chapter: false
pre: "<b> 5.7.3. </b> "
---

1. **Chọn Route 53 → Hosted zones → Create hosted zone.** Nhập domain đã đăng ký, chọn Public hosted zone → Create. Ghi đủ bốn nameserver trong NS record.

{{< btbfigure file="aws-route-53-step-1-create-hosted-zone.webp" caption="Public hosted zone cho domain của nhóm." step="yes" >}}

2. **Mở Mắt Bão → Quản trị tên miền → Name Server.** Nếu chuyển domain đang dùng, sao chép các record MX/TXT/ACM cần thiết sang zone mới trước. Chọn Sử dụng Name Server tùy chỉnh, nhập bốn giá trị của zone và lưu.

{{< btbfigure file="aws-route-53-step-2-setup-route.webp" caption="Màn hình cấu hình nameserver tại Mắt Bão." step="yes" >}}

3. **Quay lại Route 53 → Create record.** Tạo A Alias cho apex và www tới CloudFront. Nếu distribution bật IPv6, thêm AAAA Alias. Tạo A Alias cho api tới ALB Singapore. Hostname origin API phải phân giải tới ALB.

4. **Đối chiếu DNS và HTTPS.** Thay YOUR_LAB_DOMAIN bằng tên miền của lab.


```powershell
$domain = "YOUR_LAB_DOMAIN"
Resolve-DnsName $domain -Type NS
Resolve-DnsName $domain -Type A
Resolve-DnsName "api.$domain" -Type A
curl.exe -I "https://$domain/"
curl.exe -i "https://$domain/api/health"
```


**Kiểm tra kết quả.** NS trả về đúng delegation, trình duyệt mở FE qua HTTPS và /api/health trên hostname FE phản hồi từ backend.
