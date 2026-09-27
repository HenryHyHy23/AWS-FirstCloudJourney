---
title : "1. Giới thiệu"
date: 2025-09-09
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Tổng quan về những gì sẽ làm trong Workshop

Trong workshop này, chúng ta sẽ thực hành triển khai một website tĩnh (static website) trên dịch vụ lưu trữ **Amazon S3** và tối ưu hóa việc phân phối nội dung thông qua mạng phân phối nội dung (CDN) **Amazon CloudFront**.

#### Nội dung chính bao gồm:

1.  **Chuẩn bị (5.2)**: Tạo S3 Bucket và tải mã nguồn website mẫu lên bucket.
2.  **Cấu hình S3 Static Hosting (5.3)**: Bật tính năng hosting website tĩnh trên S3.
3.  **Quản lý quyền truy cập (5.4 - 5.5)**: Cấu hình *Block Public Access* và *Access Control List (ACL)* để cho phép người dùng truy cập website từ internet.
4.  **Kiểm tra Website (5.6)**: Xác minh website hoạt động thông qua S3 Endpoint.
5.  **Tích hợp CloudFront (5.7)**:
    -   Khởi tạo CloudFront Distribution trỏ về S3 origin.
    -   Tăng cường bảo mật bằng cách chặn truy cập trực tiếp vào S3 (Block Public Access) và chuyển hướng lưu lượng qua CloudFront.
6.  **Dọn dẹp tài nguyên (5.8)**: Hướng dẫn xóa các tài nguyên đã tạo để tránh phát sinh chi phí.
