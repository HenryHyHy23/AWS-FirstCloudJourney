---
title: "Thu telemetry từ React bằng CloudWatch RUM"
menuTitle: "CloudWatch RUM"
weight: 4
chapter: false
pre: "<b> 5.8.4. </b> "
---

1. **Mở CloudWatch → RUM → Add app monitor.** Chọn các trường sau.
   - Platform — Web
   - App monitor name — `btb-frontend-performance`
   - Domains — các hostname FE thực sự dùng, ví dụ apex và www
   - Authorization — chọn Cognito Identity Pool và hoàn tất bước guest role ở 5.8.5

{{< btbfigure file="aws-cloudwatch-rum-step-1-add-app-monitor.webp" caption="Tên app monitor, platform Web và domain list." step="yes" >}}

2. **Mở Code snippet → TypeScript.** Trong repo FE chạy `npm install aws-rum-web`. Đặt code dưới đây trong module khởi tạo một lần trước khi render React. Thay APP_MONITOR_UUID và IDENTITY_POOL_UUID bằng ID của lab.

{{< btbfigure file="aws-cloudwatch-rum-step-2.webp" caption="Hướng dẫn tích hợp TypeScript từ monitor." step="yes" >}}


```typescript
import { AwsRum, type AwsRumConfig } from 'aws-rum-web';

try {
  const config: AwsRumConfig = {
    sessionSampleRate: 1,
    identityPoolId: 'ap-southeast-1:IDENTITY_POOL_UUID',
    endpoint: 'https://dataplane.rum.ap-southeast-1.amazonaws.com',
    telemetries: ['performance', 'errors', 'http'],
    allowCookies: true,
    enableXRay: false,
    signing: true,
  };
  new AwsRum('APP_MONITOR_UUID', '1.0.0', 'ap-southeast-1', config);
} catch {
  // Telemetry initialization must not prevent React from starting.
}
```


3. **Build lại frontend → upload S3 → làm mới cache CloudFront.** Mở website trong tab mới trên hostname đã khai báo. Ảnh dùng sampling 100% và cookie; áp dụng cơ chế đồng ý của website khi sử dụng thực tế.

**Kiểm tra kết quả.** DevTools → Network: lọc dataplane.rum và xem status. Trong RUM → monitor, chọn time range của lần test và tìm page view. Với 403 kiểm tra guest role và signing. Note nhóm ghi đã thêm code, còn ingestion production cần test.
