---
title: "Kiểm tra tìm địa chỉ và tọa độ với Vietmap"
menuTitle: "Vietmap bản đồ và vị trí"
weight: 3
chapter: false
pre: "<b> 5.9.3. </b> "
---

1. **Chọn hai địa điểm công cộng làm kho và điểm giao demo.** Trong project Vietmap lab kiểm tra quyền API tìm địa chỉ/định tuyến. Đặt server key trong secret backend; với SDK ở FE dùng client key có hạn chế domain phù hợp.

2. **Mở BTB → màn địa chỉ giao hàng.** Nhập địa điểm thứ nhất, chọn một gợi ý, đối chiếu phường/tỉnh, tọa độ và marker.

3. **Chọn địa điểm thứ hai và lưu.** Nếu màn hình hỗ trợ route, ghi khoảng cách cùng đơn vị và nguồn ETA. Thử một chuỗi không có kết quả rồi chỉnh lại địa chỉ.

**Kiểm tra kết quả.** Vị trí marker đúng với địa điểm đã chọn. Khi marker sai, kiểm tra thứ tự longitude/latitude theo API. Với 403 kiểm tra quyền/key restriction; với 429 kiểm tra quota.
