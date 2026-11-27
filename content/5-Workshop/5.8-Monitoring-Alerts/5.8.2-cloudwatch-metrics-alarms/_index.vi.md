---
title: "Cảnh báo khi ALB trả lỗi 5xx"
menuTitle: "CloudWatch Metrics và Alarms"
weight: 2
chapter: false
pre: "<b> 5.8.2. </b> "
---

Tạo SNS topic và xác nhận email ở 5.8.3 trước khi gắn action cho alarm.

1. **Mở CloudWatch → Alarms → All alarms → Create alarm → Select metric.** Chọn nguồn CloudWatch metrics.

![Mở CloudWatch → Alarms → All alarms → Create alarm → Select metric.](/images/5-Workshop/5.8-Monitoring-Alerts/5.8.2-cloudwatch-metrics-alarms/aws-cloudwatch-metrics--alarms-step-1-create-alarm.webp)

2. **Chọn ApplicationELB → Per AppELB Metrics.** Tìm đúng LoadBalancer của lab và chọn HTTPCode_ELB_5XX_Count. Dùng HTTPCode_Target_5XX_Count khi cần đo lỗi ứng dụng trả về.

![Chọn dimension LoadBalancer thay cho số liệu gộp.](/images/5-Workshop/5.8-Monitoring-Alerts/5.8.2-cloudwatch-metrics-alarms/aws-cloudwatch-metrics--alarms-step-2-select-metric.webp)

3. **Chọn điều kiện alarm → Next.** Ví dụ cho lab ít traffic:
   - Statistic — Sum; period — 5 minutes
   - Threshold type — Static; Greater than — nhập 0
   - Datapoints — 1 of 1; missing data — not breaching

![Metric, statistic, period và điều kiện của alarm.](/images/5-Workshop/5.8-Monitoring-Alerts/5.8.2-cloudwatch-metrics-alarms/aws-cloudwatch-metrics--alarms-step-3-setup-metric.webp)

4. **Trong Configure actions, chọn In alarm → Select an existing SNS topic.** Chọn btb-system-alerts và bấm Next.

![Gắn SNS vào trạng thái In alarm.](/images/5-Workshop/5.8-Monitoring-Alerts/5.8.2-cloudwatch-metrics-alarms/aws-cloudwatch-metrics--alarms-step-4-configure.webp)

5. **Nhập Alarm name → Review → Create alarm.** Dùng `btb-backend-alb-5xx` như ảnh.

![Tên alarm của backend ALB.](/images/5-Workshop/5.8-Monitoring-Alerts/5.8.2-cloudwatch-metrics-alarms/aws-cloudwatch-metrics--alarms-step-5-name.webp)

**Kiểm tra kết quả.** Mở alarm → History. Khi có chuyển trạng thái thật, ghép timestamp action với thư SNS. Publish test ở SNS chỉ kiểm tra được đoạn SNS → email.
