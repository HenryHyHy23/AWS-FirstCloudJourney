---
title: "Gửi cảnh báo vận hành qua SNS"
menuTitle: "Amazon SNS"
weight: 3
chapter: false
pre: "<b> 5.8.3. </b> "
---

1. **Tìm Simple Notification Service → mở Topics.**

![Tìm Simple Notification Service → mở Topics.](/images/5-Workshop/5.8-Monitoring-Alerts/5.8.3-amazon-sns/aws-sns-step-1.webp)

2. **Chọn Create topic → Standard.** Nhập tên `btb-system-alerts`, giữ cùng Region với alarm, rồi chọn Create topic.

![Chọn Standard topic cho email cảnh báo.](/images/5-Workshop/5.8-Monitoring-Alerts/5.8.3-amazon-sns/aws-sns-step-2-create-topic.webp)

3. **Mở topic → Create subscription.** Protocol: Email; Endpoint: hộp thư lab. Bấm Create subscription. Trạng thái ban đầu có thể là Pending confirmation.

![Mở topic → Create subscription.](/images/5-Workshop/5.8-Monitoring-Alerts/5.8.3-amazon-sns/aws-sns-step-3-create-subscription.webp)

4. **Mở thư AWS Notification → Confirm subscription.** Quay lại Subscriptions và kiểm tra đã có subscription ID.

![Nội dung email mời xác nhận.](/images/5-Workshop/5.8-Monitoring-Alerts/5.8.3-amazon-sns/aws-sns-step-4-confirm.webp)

5. **Mở topic → Publish message.** Nhập subject BTB lab và nội dung thử; đối chiếu topic ARN, người nhận và thời gian email đến.

**Kiểm tra kết quả.** Hộp thư lab nhận đúng nội dung. Nếu chưa tới, kiểm tra Pending confirmation, spam và Region.
