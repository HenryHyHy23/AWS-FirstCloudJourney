---
title: "Gửi cảnh báo vận hành qua SNS"
menuTitle: "Amazon SNS"
weight: 3
chapter: false
pre: "<b> 5.8.3. </b> "
---

1. **Tìm Simple Notification Service → mở Topics.**

{{< btbfigure file="aws-sns-step-1.webp" caption="Tìm Simple Notification Service → mở Topics." step="yes" >}}

2. **Chọn Create topic → Standard.** Nhập tên `btb-system-alerts`, giữ cùng Region với alarm, rồi chọn Create topic.

{{< btbfigure file="aws-sns-step-2-create-topic.webp" caption="Chọn Standard topic cho email cảnh báo." step="yes" >}}

3. **Mở topic → Create subscription.** Protocol: Email; Endpoint: hộp thư lab. Bấm Create subscription. Trạng thái ban đầu có thể là Pending confirmation.

{{< btbfigure file="aws-sns-step-3-create-subscription.webp" caption="Mở topic → Create subscription." step="yes" >}}

4. **Mở thư AWS Notification → Confirm subscription.** Quay lại Subscriptions và kiểm tra đã có subscription ID.

{{< btbfigure file="aws-sns-step-4-confirm.webp" caption="Nội dung email mời xác nhận." step="yes" >}}

5. **Mở topic → Publish message.** Nhập subject BTB lab và nội dung thử; đối chiếu topic ARN, người nhận và thời gian email đến.

**Kiểm tra kết quả.** Hộp thư lab nhận đúng nội dung. Nếu chưa tới, kiểm tra Pending confirmation, spam và Region.
