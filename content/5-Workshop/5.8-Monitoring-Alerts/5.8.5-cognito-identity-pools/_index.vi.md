---
title: "Cấp credential tạm cho trình duyệt gửi RUM"
menuTitle: "Amazon Cognito Identity Pools"
weight: 5
chapter: false
pre: "<b> 5.8.5. </b> "
---

1. **Mở Cognito → Identity pools → pool mà RUM sử dụng → User access.** Kiểm tra Guest access Active, rồi bấm tên guest role. Kiểm tra trust policy giới hạn aud đúng pool, amr unauthenticated và principal cognito-identity.amazonaws.com.

![Guest access và guest role.](/images/5-Workshop/5.8-Monitoring-Alerts/5.8.5-cognito-identity-pools/aws-cognito-identity-pools-step-1-guest-access.webp)

2. **Mở IAM role → Permissions → putRumEvents → Edit → JSON.** Ảnh cho phép ghi vào mọi monitor của account. Mẫu lab dưới ảnh thu hẹp quyền xuống monitor BTB. Thay ACCOUNT_ID và tên monitor, rồi Review → Save.

![JSON cho phép rum:PutRumEvents.](/images/5-Workshop/5.8-Monitoring-Alerts/5.8.5-cognito-identity-pools/aws-cognito-identity-pools-step-2-policy-check.webp)


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
