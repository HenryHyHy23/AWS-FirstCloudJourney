---
title: "Định tuyến sự kiện đơn hàng và thanh toán"
menuTitle: "Amazon EventBridge"
weight: 6
chapter: false
pre: "<b> 5.8.6. </b> "
---

Tạo trước hai Standard queue btb-customer-events và btb-customer-events-dlq theo 5.8.7. Dùng cùng Region cho bus, rule và queue.

1. **Mở EventBridge → Event buses → Create custom event bus.** Chọn loại classic như ảnh, nhập `btb-events`, chọn AWS owned key.

{{< btbfigure file="aws-amazon-eventbridge-step-1-create-custom-event-bus.webp" caption="Tên bus và phần encryption." step="yes" >}}

2. **Kiểm tra Logs, Resource-based policy và Archives.** Chọn đích log nếu lab cần ghi sự kiện. Với producer cùng account, cấp PutEvents đúng bus cho task role như 5.4.2.

{{< btbfigure file="aws-amazon-eventbridge-step-1-create-custom-event-bus-1.webp" caption="Kiểm tra Logs, Resource-based policy và Archives." step="yes" >}}

3. **Kiểm tra tùy chọn archive → Create.** Nếu bật replay, đặt tên btb-events-archive và retention phù hợp. Ghi lại event ID để consumer xử lý sự kiện phát lại.

{{< btbfigure file="aws-amazon-eventbridge-step-1-create-custom-event-bus-2.webp" caption="Archive tùy chọn khi tạo bus." step="yes" >}}

4. **Chọn Rules → Create rule.** Đặt tên `btb-customer-notifications`, chọn bus `btb-events` và rule with an event pattern.

{{< btbfigure file="aws-amazon-eventbridge-step-2-create-rule.webp" caption="Tên rule và bus được chọn." step="yes" >}}

5. **Trong Event pattern chọn Custom pattern → JSON editor.** Dán mẫu dưới ảnh. Source phải trùng chính xác; detail-type bắt đầu bằng order. hoặc payment. sẽ match.

{{< btbfigure file="aws-amazon-eventbridge-step-2-create-rule-2.webp" caption="Source và hai prefix trong event pattern." step="yes" >}}


```json
{
  "source": [
    "bitetobusiness.backend"
  ],
  "detail-type": [
    {
      "prefix": "order."
    },
    {
      "prefix": "payment."
    }
  ]
}
```


6. **Chọn Target → SQS queue → btb-customer-events.** Với execution role do console tạo, kiểm tra trust EventBridge và SendMessage trên ARN của queue.

{{< btbfigure file="aws-amazon-eventbridge-step-2-create-rule-3.webp" caption="Chọn SQS queue trong tài khoản lab." step="yes" >}}

7. **Mở Retry policy and dead-letter queue.** Cấu hình theo ảnh.
   - Maximum event age — 1 giờ
   - Retry attempts — 20
   - Dead-letter queue — btb-customer-events-dlq; kiểm tra queue policy cho phép EventBridge gửi từ rule ARN

{{< btbfigure file="aws-amazon-eventbridge-step-2-create-rule-4.webp" caption="Giới hạn retry và queue nhận sự kiện không giao được." step="yes" >}}

8. **Mở Input transformation → Part of the matched event.** Nhập `$.detail`, rồi Review → Create. Kiểm tra trạng thái Enabled. Body trên SQS chỉ chứa detail.

{{< btbfigure file="aws-amazon-eventbridge-step-2-create-rule-5.webp" caption="JSONPath chọn phần detail để gửi tới queue." step="yes" >}}

9. **Gửi sự kiện thử từ bus lab.** Mở event bus → Send events. Điền Source `bitetobusiness.backend`, Detail type `order.workshop`, Detail `{"eventId":"lab-001","orderRef":"DEMO-01"}`. Chọn Send và mở queue ở 5.8.7 để đọc body. Trong Event pattern sandbox, thử inventory.updated để kiểm tra trường hợp không match.

**Kiểm tra kết quả.** SQS nhận eventId lab-001 và orderRef DEMO-01. Nếu queue trống, kiểm tra bus, pattern, target role và FailedInvocations.
