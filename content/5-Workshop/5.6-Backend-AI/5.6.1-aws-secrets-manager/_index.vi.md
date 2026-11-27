---
title: "Lưu cấu hình backend trong Secrets Manager"
menuTitle: "AWS Secrets Manager"
weight: 1
chapter: false
pre: "<b> 5.6.1. </b> "
---

1. **Mở Secrets Manager → Store a new secret → Other type of secret.** Trong Key/value, nhập các tên biến đúng với cấu hình Spring Boot và giá trị lấy từ project lab. Ảnh chỉ hiển thị cột key.

{{< btbfigure file="aws-secret-manager-step-1.webp" caption="Các key kết nối xuất hiện trong ảnh Secrets Manager." step="yes" >}}

2. **Chọn Next → Configure secret.** Nhập tên `btb-backend` và mô tả cấu hình. Chọn Region cùng ECS.

{{< btbfigure file="aws-secret-manager-step-2-configure.webp" caption="Tên và mô tả secret." step="yes" >}}

3. **Mở Configure rotation.** Với lab nhập credential thủ công, để automatic rotation tắt. Nếu bật, chuẩn bị cơ chế đổi đồng thời credential ở nhà cung cấp. Chọn Next.

{{< btbfigure file="aws-secret-manager-step-3-rotation.webp" caption="Mở Configure rotation." step="yes" >}}

4. **Kiểm tra Review → Store.** So tên secret và encryption key. Sau khi tạo, copy ARN đầy đủ, gồm cả hậu tố AWS sinh.

{{< btbfigure file="aws-secret-manager-step-4-review.webp" caption="Kiểm tra loại secret và encryption key ở màn hình Review." step="yes" >}}

5. **Mở ECS → Task definitions → Create new revision → Container → Environment variables.** Chọn ValueFrom cho từng key, dạng `SECRET_ARN:JSON_KEY::`. Cho execution role quyền GetSecretValue. Dùng Linux Fargate 1.4.0 trở lên để lấy từng JSON key.

**Kiểm tra kết quả.** Sau khi triển khai task mới, gọi một API đọc catalog. Nếu task dừng ở ResourceInitializationError, kiểm tra ARN, tên JSON key, quyền execution role và egress tới Secrets Manager.
