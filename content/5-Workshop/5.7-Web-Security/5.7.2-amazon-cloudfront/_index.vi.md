---
title: "CloudFront với hai origin cho storefront BTB"
menuTitle: "Amazon CloudFront"
weight: 2
chapter: false
pre: "<b> 5.7.2. </b> "
---

## Phân phối frontend từ S3

1. **Mở CloudFront → Distributions → Create distribution.** Chọn Single website or app, nhập tên và các hostname FE của lab.

{{< btbfigure file="aws-cloudfront-step-1-create-distribution.webp" caption="Mở CloudFront → Distributions → Create distribution." step="yes" >}}

2. **Trong Specify origin, chọn Amazon S3.** Chọn REST endpoint của bucket FE, dùng Origin Access Control với signing. Áp dụng bucket policy do CloudFront đề xuất: GetObject trên bucket/*, giới hạn theo ARN distribution. Đặt default root object là index.html.

{{< btbfigure file="aws-cloudfront-step-2-origin.webp" caption="Chọn origin cho bản build frontend." step="yes" >}}

3. **Mở Enable security.** Đọc lựa chọn WAF cho distribution. Nếu bật ở đây, ghi lại web ACL mà CloudFront tạo để dùng ở bài 5.7.4.

{{< btbfigure file="aws-cloudfront-step-3-enable-security.webp" caption="Mở Enable security." step="yes" >}}

4. **Chọn Get TLS certificate → chọn cert us-east-1.** Kiểm tra cả tên apex và www nếu lab dùng cả hai.

{{< btbfigure file="aws-cloudfront-step-4-setup-certificate.webp" caption="Gắn chứng chỉ TLS vào distribution." step="yes" >}}

5. **Chọn Review and create → Create distribution.** Đợi Deployed, mở domain distribution và kiểm tra trang chủ trước khi thêm rewrite.

{{< btbfigure file="aws-cloudfront-step-5-review.webp" caption="Chọn Review and create → Create distribution." step="yes" >}}

## Cho phép refresh route React

6. **Mở CloudFront → Functions → Create function.** Đặt tên `btb-spa-rewrite`, chọn JavaScript runtime 2.0 rồi Create.

{{< btbfigure file="aws-cloudfront-step-6-create-function.webp" caption="Mở CloudFront → Functions → Create function." step="yes" >}}

7. **Mở Build → Development, dán function rồi Save changes.** Hàm đổi URI sang /index.html khi đoạn cuối không có dấu chấm.

{{< btbfigure file="aws-cloudfront-step-7-build-code.webp" caption="Editor của CloudFront Function." step="yes" >}}


```javascript
function handler(event) {
    var request = event.request;
    var uri = request.uri;
    var lastSegment = uri.substring(uri.lastIndexOf('/') + 1);
    if (lastSegment.indexOf('.') === -1) {
        request.uri = '/index.html';
    }
    return request;
}
```


8. **Test → Publish → Associate.** Thử /customer/profile và /assets/app.js. Gắn bản Live vào Default (*) tại sự kiện Viewer request.

{{< btbfigure file="aws-cloudfront-step-8-associate-function.webp" caption="Chọn behavior và sự kiện cho function đã publish." step="yes" >}}

9. **Mở Error pages → kiểm tra mapping 403 cũ.** Nếu lab đang trả /index.html và 200 cho mọi lỗi 403, thử function trước, rồi bỏ mapping cũ để API giữ đúng status.

{{< btbfigure file="aws-cloudfront-step-9-delete-403-error-page.webp" caption="Mở Error pages → kiểm tra mapping 403 cũ." step="yes" >}}

10. **Kiểm tra mapping 404 tương tự.** Nếu không có custom error response, chuyển sang bước tạo origin API.

{{< btbfigure file="aws-cloudfront-step-9-delete-404-error-page.webp" caption="Kiểm tra mapping 404 tương tự." step="yes" >}}

## Thêm origin và behavior cho API

11. **Chuẩn bị giá trị riêng cho X-Origin-Verify.** Ảnh minh họa mục đích tạo chuỗi trong PowerShell. Với lab đang chạy, dùng giá trị mới từ bộ sinh ngẫu nhiên an toàn và chỉ lưu ở origin cùng nơi kiểm tra.

{{< btbfigure file="aws-cloudfront-step-10-idk.webp" caption="Dòng lệnh PowerShell trong ảnh, đã bỏ phần kết quả." step="yes" >}}

12. **Chọn Distributions → Origins → Create origin.** Nhập hostname API trỏ đến ALB, chọn HTTPS only và port 443. Chứng chỉ của ALB phải phủ hostname này.

{{< btbfigure file="aws-cloudfront-step-11-create-origin.webp" caption="Giao thức HTTPS và hostname của origin API." step="yes" >}}

13. **Mở Add custom header.** Header name: X-Origin-Verify. Điền giá trị riêng đang dùng ở WAF regional. Đặt tên origin btb-backend-alb.

{{< btbfigure file="aws-cloudfront-step-11-create-origin-1.webp" caption="Tên custom header tại origin, đã lược bỏ cột giá trị." step="yes" >}}

14. **Kiểm tra Additional settings → Create origin.** Đối chiếu connection attempts, connection timeout và response timeout với thời gian phản hồi dự kiến.

{{< btbfigure file="aws-cloudfront-step-11-create-origin-2.webp" caption="Kiểm tra Additional settings → Create origin." step="yes" >}}

15. **Mở Behaviors → Create behavior.** Chọn path /api/*, origin btb-backend-alb, Redirect HTTP to HTTPS và đầy đủ các HTTP method mà API dùng.

{{< btbfigure file="aws-cloudfront-step-12-edit-behavior.webp" caption="Behavior API chuyển request sang ALB." step="yes" >}}

16. **Đặt cache policy và origin request policy → Save.** Chọn CachingDisabled và AllViewerExceptHostHeader. Để trống function association của behavior API.

{{< btbfigure file="aws-cloudfront-step-12-create-behavior.webp" caption="Cache policy và origin request policy cho API." step="yes" >}}

**Kiểm tra kết quả.** Đợi Deployed. Refresh một route con, mở một file JS, rồi gọi /api/health trên hostname FE. API phải trả response của backend; nếu nhận HTML, kiểm tra behavior /api/*, function association và custom error mapping.
