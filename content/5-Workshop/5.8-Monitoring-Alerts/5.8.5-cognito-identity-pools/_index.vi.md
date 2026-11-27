---
title: "Cấp credential tạm cho trình duyệt gửi RUM"
menuTitle: "Amazon Cognito Identity Pools"
weight: 5
chapter: false
pre: "<b> 5.8.5. </b> "
---

1. **Mở Cognito → Identity pools → pool mà RUM sử dụng → User access.** Kiểm tra Guest access Active, rồi bấm tên guest role. Kiểm tra trust policy giới hạn aud đúng pool, amr unauthenticated và principal cognito-identity.amazonaws.com.

{{< btbfigure file="aws-cognito-identity-pools-step-1-guest-access.webp" caption="Guest access và guest role." step="yes" >}}

2. **Mở IAM role → Permissions → putRumEvents → Edit → JSON.** Ảnh cho phép ghi vào mọi monitor của account. Mẫu lab dưới ảnh thu hẹp quyền xuống monitor BTB. Thay ACCOUNT_ID và tên monitor, rồi Review → Save.

{{< btbfigure file="aws-cognito-identity-pools-step-2-policy-check.webp" caption="JSON cho phép rum:PutRumEvents." step="yes" >}}


```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "SendFrontendTelemetry",
      "Effect": "Allow",
      "Action": "rum:PutRumEvents",
      "Resource": "arn:aws:rum:ap-southeast-1:ACCOUNT_ID:appmonitor/btb-frontend-performance"
    }
  ]
}
```


**Kiểm tra kết quả.** Gắn pool ID vào client ở 5.8.4, mở phiên trình duyệt mới và kiểm tra PutRumEvents thành công. Guest role này chỉ cấp quyền gửi telemetry.
