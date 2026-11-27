---
title: "Kiểm tra ảnh sản phẩm qua Cloudinary"
menuTitle: "Cloudinary lưu trữ ảnh"
weight: 2
chapter: false
pre: "<b> 5.9.2. </b> "
---

1. **Mở Cloudinary Console → product environment của lab → Media Library.** Tạo thư mục lab, chọn Upload rồi tải một ảnh PNG/JPEG thử.

2. **Mở asset vừa tạo → Asset details.** Ghi public_id và secure_url. Giữ credential ký upload ở backend trong Secrets Manager.

3. **Trong BTB, mở màn quản lý catalog → SKU thử.** Gán ảnh và lưu. Mở storefront, kiểm tra thẻ sản phẩm và trang chi tiết của cùng SKU.

4. **Mở DevTools → Network → Img → refresh.** Chọn request ảnh, đối chiếu hostname Cloudinary, Content-Type và kích thước truyền.

**Kiểm tra kết quả.** Ảnh tải được khi mở lại trang. Nếu URL trả 404, đối chiếu public_id và chữ hoa/thường. Nếu URL đúng nhưng thẻ trống, kiểm tra dữ liệu API và img src. Gỡ liên kết ở SKU trước khi xóa ảnh lab.
