---
title: "Kiểm thử và trình diễn"
menuTitle: "Kiểm thử và trình diễn"
weight: 11
chapter: false
pre: "<b> 5.11. </b> "
---



Thực hiện trên môi trường lab đã kết nối, dùng tài khoản và đơn hàng demo. Với mỗi ca lưu thời điểm, SHA/revision, kết quả quan sát và một minh chứng đã che dữ liệu. Bộ ảnh cấu hình đi kèm chưa xác nhận các ca dưới đây đã chạy trên production.

## 1. Kiểm tra từ mép hệ thống đến container

Thay hostname và route React bằng route có thật của bản build. Với route cần đăng nhập, HTTP vẫn có thể trả React shell trước khi ứng dụng chuyển trang. Kiểm tra response body/Content-Type để phân biệt API với HTML.


```powershell
$site = "https://YOUR_LAB_FRONTEND"
curl.exe -I "$site/"
curl.exe -i "$site/customer/profile"
curl.exe -i "$site/api/health"
curl.exe -i "$site/api/workshop-missing"
```


1. **Trang chủ và deep link:** mong đợi HTTPS hợp lệ, JS/CSS tải được, refresh route con vẫn hiển thị đúng hoặc điều hướng login theo quyền. API health trả 200 theo target group. API không tồn tại trả lỗi theo contract, không nhận HTML trang chủ.

2. **Phân quyền và mua hàng:** Guest xem catalog; tài khoản Customer và Staff demo đi qua luồng duyệt báo giá. Kiểm tra giá theo số lượng, tình huống thiếu một SKU, stock và trạng thái đơn trước/sau. Với route duyệt của Staff, yêu cầu từ Customer phải bị backend từ chối.

3. **Nhà cung cấp ngoài:** dùng sản phẩm có ảnh Cloudinary, địa chỉ Vietmap và hộp thư OTP test. Đối chiếu transaction mẫu SePay với đúng đơn và kiểm tra replay ở 5.9.4. Ghi trạng thái chuyến sau thao tác xác nhận giao.

## 2. Kiểm tra phần còn thiếu bằng chứng runtime

**AI:** gửi prompt từ BTB và đối chiếu log task cùng timestamp với model/profile thực tế. **RUM:** mở FE rồi đối chiếu dataplane request và event/page view trong monitor. **Queue:** gửi sự kiện order.workshop ở 5.8.6, kiểm tra SQS body `eventId=lab-001`; nếu có consumer thật, so kết quả với một bản ghi thông báo.

**WAF:** lưu rule action và associated resource theo scope tại thời điểm test. **Alarm:** ghép alarm history và email SNS cùng lần chuyển trạng thái. **CI/CD:** ghép run ID với task revision và asset frontend ở 5.10.

## 3. Ghi kết quả demo

Một dòng minh chứng nên có ca test, thời điểm có múi giờ, hostname lab, kết quả mong đợi, kết quả quan sát, liên kết ảnh/log đã che dữ liệu và trạng thái đạt/lỗi. Với test chưa chạy, ghi rõ điều kiện còn thiếu, ví dụ quyền InvokeModel hoặc chưa có phiên gửi RUM.
