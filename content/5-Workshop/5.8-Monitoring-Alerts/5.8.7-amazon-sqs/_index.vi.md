---
title: "Đọc message SQS và phân biệt hai loại DLQ"
menuTitle: "Amazon SQS và hàng đợi lỗi"
weight: 7
chapter: false
pre: "<b> 5.8.7. </b> "
---

1. **Mở SQS → Create queue → Standard.** Tạo `btb-customer-events` và `btb-customer-events-dlq` cùng Region với bus. Ghi lại URL queue chính và ARN hai queue. Nếu dùng KMS key riêng, kiểm tra quyền của sender và receiver.

2. **Quay lại EventBridge → rule → Target.** Chọn queue chính làm target và queue lỗi làm target DLQ. Khi EventBridge gửi tới target thất bại, DLQ này nhận sự kiện theo retry policy.

{{< btbfigure file="aws-amazon-eventbridge-step-2-create-rule-4.webp" caption="Giới hạn retry và queue nhận sự kiện không giao được." step="yes" >}}

3. **Gửi event ở 5.8.6 → mở queue chính → Send and receive messages → Poll for messages.** Trong lab riêng, tạm dừng consumer nếu cần xem message trước khi consumer đọc.

4. **Mở Message body và đối chiếu eventId.** Có thể dùng PowerShell bên dưới thay cho thao tác Poll. Chỉ xóa message demo bằng receipt handle mới nhất sau khi xử lý xong.


```powershell
$region = "ap-southeast-1"
$queueUrl = aws sqs get-queue-url --queue-name btb-customer-events --region $region --query QueueUrl --output text
aws sqs receive-message --region $region --queue-url $queueUrl --wait-time-seconds 10 --max-number-of-messages 1 --visibility-timeout 30 --query "Messages[].{Body:Body,ReceiptHandle:ReceiptHandle}"
```


**Kiểm tra kết quả.** Kết quả mong đợi là lab-001 trong body. Message đã nhận nhưng chưa xóa có thể xuất hiện lại khi hết visibility timeout. Nếu consumer xử lý lỗi nhiều lần, dùng redrive policy và maxReceiveCount của queue chính để chuyển sang **consumer DLQ** riêng.
