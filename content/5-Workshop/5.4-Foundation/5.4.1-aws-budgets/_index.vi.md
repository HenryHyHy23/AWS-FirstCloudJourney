---
title: "Theo dõi chi phí bằng AWS Budgets"
menuTitle: "AWS Budgets"
weight: 1
chapter: false
pre: "<b> 5.4.1. </b> "
---

Tạo ngân sách tháng trước khi khởi động Fargate và ALB. Chuẩn bị một hộp thư lab để nhận cảnh báo.

1. **Tìm Billing and Cost Management trong AWS Console.**

{{< btbfigure file="aws-budgets-step-1.webp" caption="Tìm Billing and Cost Management trong AWS Console." step="yes" >}}

2. **Mở Budgets → Create budget.**

{{< btbfigure file="aws-budgets-step-2-create-budget.webp" caption="Mở Budgets → Create budget." step="yes" >}}

3. **Chọn Customize (advanced) → Cost budget → Next.**

{{< btbfigure file="aws-budgets-step-3-customization-setup.webp" caption="Chọn Cost budget để theo dõi chi tiêu bằng tiền." step="yes" >}}

4. **Đặt tên và chu kỳ.** Dùng các trường trong Set your budget.
   - Budget name — `BTB-Monthly`
   - Period — Monthly
   - Renewal — Recurring
   - Budgeting method — Fixed

{{< btbfigure file="aws-budgets-step-4-set-your-budget.webp" caption="Tên ngân sách và cách gia hạn theo tháng." step="yes" >}}

5. **Nhập số tiền và chọn phạm vi.** Ảnh dùng 200 USD, All AWS services. Chọn số tiền phù hợp tài khoản lab rồi bấm Next.

{{< btbfigure file="aws-budgets-step-4-set-your-budget-1.webp" caption="Số tiền cố định và phạm vi dịch vụ." step="yes" >}}

6. **Chọn Add an alert threshold.** Chọn Actual cost, Greater than và 75% of budgeted amount. Với ngân sách trong ảnh, ngưỡng là 150 USD.

{{< btbfigure file="aws-budgets-step-5-set-alerts.webp" caption="Ngưỡng dựa trên chi phí thực tế." step="yes" >}}

7. **Điền Email recipients.** Dùng hộp thư mà bạn có thể mở để xác minh, rồi chọn Next.

{{< btbfigure file="aws-budgets-step-5-set-alerts-1.webp" caption="Điền Email recipients." step="yes" >}}

8. **Kiểm tra mục Attach actions.** Để trống nếu chỉ cần email cảnh báo; bấm Next.

{{< btbfigure file="aws-budgets-step-6-review-alert.webp" caption="Kiểm tra mục Attach actions." step="yes" >}}

9. **Đối chiếu Budget details.** Kiểm tra Cost budget, Monthly và số tiền đã nhập.

{{< btbfigure file="aws-budgets-step-7-review.webp" caption="Đối chiếu Budget details." step="yes" >}}

10. **Kiểm tra Alert #1 → Create budget.** Mở thư xác minh nếu AWS yêu cầu.

{{< btbfigure file="aws-budgets-step-7-review-1.webp" caption="Bản review hiển thị ngưỡng 75% và không có hành động tự động." step="yes" >}}

**Kiểm tra kết quả.** Mở lại budget, kiểm tra ngưỡng Actual 75% và trạng thái người nhận. Dữ liệu billing có độ trễ.
