---
title: "Bảo vệ hai điểm vào bằng AWS WAF"
menuTitle: "AWS WAF (Frontend & Backend)"
weight: 4
chapter: false
pre: "<b> 5.7.4. </b> "
---

Chuẩn bị distribution CloudFront, ALB lab và hostname API. Trong bài này CloudFront và ALB dùng hai web ACL khác scope. So tên ACL trên thanh điều hướng trước khi lưu mỗi rule.

## Tạo web ACL và nhập rule mẫu

1. **Tìm WAF & Shield trong AWS Console.**

![Tìm WAF & Shield trong AWS Console.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-1.webp)

2. **Mở Protection packs (web ACLs) → Create protection pack.** Chọn Global cho CloudFront hoặc Regional/Singapore cho ALB. Chọn resource đúng scope. Nếu CloudFront đã tạo sẵn ACL ở 5.7.2, mở chính ACL đó.

![Màn hình tạo protection pack.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-2-create-protection-pack.webp)

3. **Mở Choose initial protections → Build your own pack → Add rules.** Chọn Custom rule → JSON editor.

![Mở Choose initial protections → Build your own pack → Add rules.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-add-rules.webp)

4. **Nhập rule giới hạn request theo IP.** JSON đầu tiên trong ảnh dùng 500 request/300 giây, Block với response 429. Rule này đếm mọi URI trong phạm vi ACL. Nếu ACL đã có rule, chọn priority còn trống. Đối với lab chỉ quan sát, chọn Count trong editor.

![JSON rule rate limit trong console.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-add-rules-1.webp)


```json
{
  "Name": "btb-api-rate-limit-per-ip",
  "Priority": 0,
  "Statement": {
    "RateBasedStatement": {
      "Limit": 500,
      "AggregateKeyType": "IP",
      "EvaluationWindowSec": 300
    }
  },
  "VisibilityConfig": {
    "SampledRequestsEnabled": true,
    "CloudWatchMetricsEnabled": true,
    "MetricName": "btb-api-rate-limit-per-ip"
  },
  "Action": {
    "Block": {
      "CustomResponse": {
        "ResponseCode": 429
      }
    }
  }
}
```


## Đối chiếu các trường của rule trong Visual editor

5. **Chọn Rules → Add rules → Rate-based rule.** Chuỗi ảnh dưới minh họa cấu hình edge bằng giao diện. Nếu đã tạo rule mẫu, mở Edit cho rule tương ứng để tránh trùng tên.

![Chọn Rules → Add rules → Rate-based rule.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-add-rules-2.webp)

6. **Trong Rule condition, chọn cửa sổ và cách gom request.** Ảnh visual editor dùng 2.000 request, 5 phút, Source IP. Đây là giá trị ở lần cấu hình khác với JSON 500 bên trên.

![Cửa sổ 5 phút, limit 2.000 và Source IP.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-add-rules-3.webp)

7. **Chọn Rule action → Block → Customize response.** Đặt Response code 429 để client nhận biết giới hạn tần suất.

![Chọn Rule action → Block → Customize response.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-add-rules-4.webp)

8. **Đặt tên btb-api-rate-limit-per-ip → Add rule.** Bật CloudWatch metrics và sampled requests nếu cần đối chiếu traffic.

![Đặt tên btb-api-rate-limit-per-ip → Add rule.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-add-rules-5.webp)

9. **Mở rule API → Edit → Scope of inspection.** Nếu chỉ giới hạn API, chọn Only consider requests that match → URI path → Starts with string → /api/. Lưu và xem lại scope sau khi lưu.

![Chỉ kiểm tra URI path bắt đầu bằng /api/.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-4-edit-rules.webp)

## Các rule riêng cho AI, đăng nhập và OTP

10. **Thêm rate rule cho AI.** Ở Rule condition chọn 30 request, cửa sổ 5 phút, Source IP như trong ảnh.

![Ngưỡng 30 trong chuỗi cấu hình AI.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-add-rules-6.webp)

11. **Chọn Only consider requests that match.** Tại Inspect chọn URI path và nhập route AI của BE lab, ví dụ /api/ai/agent. Ảnh minh họa bước chọn kiểu điều kiện; kiểm tra URI thực tế trước khi lưu.

![Chọn Only consider requests that match.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-add-rules-9.webp)

12. **Chọn action và custom response cho AI.** Nếu dùng Block, nhập 429 vào Response code.

![Chọn action và custom response cho AI.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-add-rules-7.webp)

13. **Đặt tên btb-ai-rate-limit-per-ip.** Kiểm tra metric name và priority rồi lưu.

![Đặt tên btb-ai-rate-limit-per-ip.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-add-rules-8.webp)

14. **Tạo rule đăng nhập → URI path → Exactly matches string.** Nhập `/api/auth/login`. Chọn limit cho luồng login theo tần suất đăng nhập hợp lệ của lab.

![Tạo rule đăng nhập → URI path → Exactly matches string.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-add-rules-10.webp)

15. **Đặt tên btb-login-rate-limit-per-ip → Add rule.**

![Đặt tên btb-login-rate-limit-per-ip → Add rule.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-add-rules-11.webp)

16. **Tạo rule verify OTP → Rate-based rule.** Ảnh dùng 10 request/5 phút theo Source IP.

![Tạo rule verify OTP → Rate-based rule.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-add-rules-12.webp)

17. **Giới hạn phạm vi verify OTP.** Chọn URI path → Exactly matches string → `/api/auth/verify-otp`.

![Giới hạn phạm vi verify OTP.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-add-rules-13.webp)

18. **Đặt tên btb-verify-otp-rate-limit-per-ip → Add rule.**

![Đặt tên btb-verify-otp-rate-limit-per-ip → Add rule.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-add-rules-14.webp)

19. **Tạo rule resend OTP.** Trong Rule condition đặt ngưỡng phù hợp; ảnh minh họa 10 request trong 5 phút.

![Tạo rule resend OTP.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-add-rules-15.webp)

20. **Chọn Source IP → Only consider requests that match.** Mở phần điều kiện để giới hạn route resend thực sự dùng.

![Chọn Source IP → Only consider requests that match.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-add-rules-16.webp)

21. **Kiểm tra điều kiện prefix.** Ảnh dùng URI path bắt đầu bằng `/api/admin/registration-requests/`. Prefix này có thể gồm nhiều thao tác quản trị. Đối chiếu path và method của thao tác gửi lại OTP trong BE trước khi chọn phạm vi cuối cùng.

![Kiểm tra điều kiện prefix.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-add-rules-17.webp)

22. **Đặt tên btb-resend-otp-rate-limit-per-ip.** Kiểm tra metric, action và priority rồi lưu.

![Đặt tên btb-resend-otp-rate-limit-per-ip.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-add-rules-18.webp)

23. **Mở Rules để kiểm tra năm rate rule.** Trong lần chụp này cả năm hiển thị Block. Mở từng tên nếu cần kiểm tra limit và URI.

![Danh sách năm rate rule hiển thị Block ở một lần chụp.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-display-rules.webp)

24. **Đối chiếu toàn bộ danh sách của ACL CloudFront.** Ưu tiên số nhỏ được xét trước. Hai managed group trong bộ ảnh kiểm tra IP reputation và các mẫu request đã biết gây rủi ro. Override action None giữ action của rule thành phần.

![Đối chiếu toàn bộ danh sách của ACL CloudFront.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-display-rules-1.webp)

<details>
<summary>Xem ảnh cấu hình hai managed rule group của AWS</summary>

![AWS managed IP reputation list ở chế độ JSON.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-4-edit-rules-1.webp)

![AWS managed Known Bad Inputs trong JSON editor.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-4-edit-rules-2.webp)

</details>

## Bảo vệ ALB và kiểm tra webhook SePay

25. **Chuyển sang Regional/Singapore → btb-backend-waf → Add rate-based rule.** Ảnh minh họa ngưỡng 100 cho webhook. Chọn cửa sổ và action phù hợp với tần suất callback thực tế.

![Chuyển sang Regional/Singapore → btb-backend-waf → Add rate-based rule.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-add-rules-19.webp)

26. **Chọn URI path → Exactly matches string.** Nhập `/api/payments/sepay/webhook` cho rule webhook.

![Chọn URI path → Exactly matches string.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-add-rules-20.webp)

27. **Đặt tên btb-sepay-webhook-rate-limit → Add rule.**

![Đặt tên btb-sepay-webhook-rate-limit → Add rule.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-add-rules-21.webp)

28. **Chọn Add rule → Custom rule.** Tạo rule kiểm tra request đến origin ALB. Chuẩn bị cùng giá trị X-Origin-Verify đã cấu hình ở CloudFront.

![Chọn Add rule → Custom rule.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-add-rules-22.webp)

29. **Chọn Match all of the following.** Cấu hình Block khi **cả hai** điều kiện đúng: header origin không khớp **và** URI không phải callback SePay đã định. Chỉ dùng ngoại lệ callback nếu lab cho phép SePay gọi trực tiếp; backend vẫn xác thực callback. Ảnh đã cắt cột chứa giá trị riêng.

![Chọn Match all of the following.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-add-rules-23.webp)

30. **Đặt tên btb-allow-only-cloudfront → Save.** Kiểm tra thứ tự thực thi với rule rate limit webhook trước khi thử request.

![Đặt tên btb-allow-only-cloudfront → Save.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-add-rules-24.webp)

31. **Kiểm tra các rule cũ còn nằm trên ACL regional.** Ảnh Delete 5 selected rules ghi lại lần nhóm dọn cấu hình. Khi thực hành trên ACL mới, bỏ qua thao tác xóa. Với ACL lab cũ, đối chiếu association và tên trước khi dọn rule thừa.

![Kiểm tra các rule cũ còn nằm trên ACL regional.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-5-delete-rules.webp)

32. **Mở btb-backend-waf → Rules và Associated resources.** Kiểm tra đúng ALB, tên từng rule, action và priority. Ảnh danh sách hiển thị Block cho hai rule regional; note nhóm từng ghi Count. Ghi cấu hình hiện hành cùng thời điểm kiểm tra.

![Hai action và priority trên ACL backend trong bộ ảnh.](/images/5-Workshop/5.7-Web-Security/5.7.4-aws-waf/aws-waf-step-3-display-rules-2.webp)

**Kiểm tra kết quả.** Gọi một request hợp lệ từ storefront và xem phản hồi. Với webhook, dùng giao dịch demo của bài 5.9.4. Đối chiếu metric, log và sampled requests từng ACL theo timestamp.
