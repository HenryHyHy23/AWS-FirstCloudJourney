---
title: "Tổng quan dự án và nghiệp vụ"
menuTitle: "Tổng quan"
weight: 1
chapter: false
pre: "<b> 5.1. </b> "
---

## Bite to Business là gì?

BTB là website **bán thực phẩm trực tiếp** cho Customer cá nhân/hộ gia đình và doanh nghiệp/hợp tác xã. Guest xem catalog/giá/review; Customer đã kích hoạt mới tạo Quote và xác nhận mua sau phê duyệt. Staff gồm **E-commerce Executive** (catalog, volume pricing, promotion, duyệt Quote), **Logistics Staff** (nhập kho, package, phân công chuyến), **Delivery Staff** (giao và xác nhận thực tế), **Admin** (duyệt tài khoản, cấp role, giám sát metric). (Sửa)

## Luồng nghiệp vụ trung tâm

```text
Guest → Đăng ký → Admin duyệt → OTP → Tạo mật khẩu -> Đăng nhập
Customer → Tìm/Lướt các danh mục -> Chọn sản phẩm -> Xem chi tiết sản phẩm -> review → Chọn số lượng → Thêm vào giỏ hàng
→ Tạo Quote → Phân bổ đủ hàng vào delivery packages
→ E-commerce Executive duyệt Quote → Customer xác nhận mua
→ Backend kiểm tra đủ tất cả dòng hàng & giữ tồn atomically
→ Order → Logistics đóng hàng → Thanh toán được xác minh (SePay)
→ Giao chuyến → Delivery Staff xác nhận đã giao thực tế
```

**Quote chưa giữ hàng**. Khi tạo Order, thiếu **bất cứ** dòng hàng nào thì không được giữ lẻ/tạo Order thành công. Một Order có thể có nhiều package/địa điểm, một chuyến tương ứng một Order trong MVP. ETA chỉ minh họa theo vận tốc cố định 40 km/h, không phải ETA giao thông trực tiếp.

## Công nghệ và phạm vi trách nhiệm

| Lớp                 | Công nghệ                          | Trách nhiệm                                                           |
| ------------------- | ---------------------------------- | --------------------------------------------------------------------- |
| Frontend            | React, TypeScript, i18n            | Giao diện tin cậy, song ngữ, thao tác người dùng                      |
| Backend             | Java, Spring Boot, Spring Security | Validation, role, giá, Quote/Order/tồn, giới hạn AI                   |
| Database/Auth       | Supabase                           | PostgreSQL và xác thực theo cập nhật; cần kiểm tra cơ chế JWT runtime |
| AI                  | Amazon Bedrock                     | AI inference; backend quyết định nghiệp vụ cuối                       |
| FE assets           | S3 + CloudFront                    | Build tĩnh React, CDN/HTTPS                                           |
| Ảnh sản phẩm/avatar | Cloudinary                         | Media, không thay S3 frontend                                         |
| Thanh toán          | SePay                              | QR chuyển khoản/webhook; FE không được tự quyết paid                  |
| Email               | Gmail SMTP + SNS                   | OTP ứng dụng và cảnh báo hạ tầng là hai mục khác nhau                 |

## Mục tiêu học tập

Nhóm có thể tự vẽ kiến trúc dùng AWS Icons, mô tả HTTP/CI/CD/telemetry, giải thích quyền IAM, kiểm thử tích hợp và hiểu chi phí/rollback production.
