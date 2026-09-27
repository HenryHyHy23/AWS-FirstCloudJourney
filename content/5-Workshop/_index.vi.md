---
title: "Workshop 5"
date: 2025-09-09
weight: 5
chapter: true
pre: " <b> 5. </b> "
---

# S3 Static Website & CloudFront Hosting System

Trong workshop này, chúng ta sẽ xây dựng một hệ thống lưu trữ và phân phối website tĩnh sử dụng **Amazon S3** và **Amazon CloudFront**.

#### Kiến trúc hệ thống
Hệ thống bao gồm các thành phần chính:
1.  **Amazon S3 Bucket**: Lưu trữ mã nguồn website (HTML, CSS, JS, Images). Được cấu hình tính năng **Static Website Hosting**.
2.  **Amazon CloudFront**: Mạng phân phối nội dung (CDN) giúp tăng tốc độ truy cập website cho người dùng toàn cầu và giảm tải cho S3.
3.  **Bảo mật**: Quản lý quyền truy cập thông qua **Block Public Access** và **Bucket Policies/ACLs**.

#### Nội dung Workshop
Các bài thực hành được chia thành các phần nhỏ để bạn dễ dàng theo dõi:

1.  **[Giới thiệu (5.1)](./5.1-workshop-overview)**: Tổng quan về workshop và các dịch vụ sử dụng.
2.  **[Chuẩn bị (5.2)](./5.2-prerequiste)**: Tạo S3 Bucket và tải lên mã nguồn website du lịch mẫu.
3.  **[Cấu hình S3 Hosting (5.3)](./5.3-s3-static-web)**: Bật tính năng Static Website Hosting trên bucket.
4.  **[Cấu hình Bảo mật (5.4 - 5.5)](./5.4-config-block-public)**: Cấu hình `Block Public Access` và `ACL` để công khai website.
5.  **[Kiểm tra Website (5.6)](./5.6-check-website)**: Truy cập website thông qua S3 Endpoint.
6.  **[Tích hợp CloudFront (5.7)](./5.7-cloudfront)**: Triển khai CDN CloudFront để phân phối nội dung website.
7.  **[Dọn dẹp (5.8)](./5.8-cleanup)**: Xóa tài nguyên để tránh phát sinh chi phí.