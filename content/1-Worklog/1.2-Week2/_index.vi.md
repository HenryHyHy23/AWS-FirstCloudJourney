---
title: "Báo cáo công việc Tuần 2"
date: 2026-09-21
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

### Mục tiêu tuần 2:

- Tìm hiểu Amazon S3 và thực hành hosting website tĩnh.
- Cấu hình CloudFront để phân phối nội dung an toàn và hiệu quả hơn.

### Các công việc cần triển khai trong tuần này:

| Ngày | Công việc                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                                                                                                  |
| ---- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | - Tìm hiểu Amazon S3: <br> &emsp; + Mô hình lưu trữ đối tượng, phân biệt Bucket và Object <br> &emsp; + Các Storage Class: Standard, Intelligent-Tiering, Standard-IA, Glacier <br> &emsp; + Cơ chế bảo mật: IAM Policy, Bucket Policy, ACL, Encryption <br> - Tìm hiểu Amazon CloudFront và cơ chế phân phối nội dung qua Edge Locations <br> - Tìm hiểu S3 Versioning và Cross-Region Replication (CRR) <br> - **Thực hành:** <br> &emsp; + Tạo S3 Bucket và upload source code website tĩnh <br> &emsp; + Bật Static Website Hosting, cấu hình Block Public Access và ACL <br> &emsp; + Tạo CloudFront Distribution, cấu hình Bucket Policy cho CloudFront <br> &emsp; + Bật Bucket Versioning, upload và khôi phục phiên bản cũ <br> &emsp; + Di chuyển Object giữa 2 Bucket <br> &emsp; + Cấu hình Cross-Region Replication sang us-east-1 | 21/09/2026   | 22/09/2026      | - [Lab 57: Hosting Static Website with Amazon S3](https://000057.awsstudygroup.com/) <br> - [Video hướng dẫn](https://youtu.be/3vSrTeWroSs?si=KmSKod77TeDMpitx) |

### Kết quả đạt được tuần 2:

- Nắm được mô hình lưu trữ của Amazon S3:
  - Bucket là container (tên globally unique, tối đa 100 bucket/tài khoản)
  - Object là dữ liệu thực sự bên trong (0 byte đến 5 TB/object)
  - Các Storage Class: Standard, Intelligent-Tiering, Standard-IA, Glacier
  - Phân quyền qua IAM Policy, Bucket Policy, ACL

- Triển khai thành công website tĩnh trên S3 và cấu hình CloudFront phân phối nội dung mà không cần công khai bucket:
  - Thời gian tải trang giảm từ ~136ms (S3 trực tiếp) xuống ~10ms (qua CloudFront)
  - PoP gần nhất là SGN50-P2 (TP. Hồ Chí Minh)

- Thực hành Bucket Versioning: upload phiên bản mới và khôi phục phiên bản cũ bằng cách xoá version mới nhất.

- Cấu hình Cross-Region Replication tự động sao chép object từ ap-southeast-1 sang us-east-1 cho mục đích Disaster Recovery, theo tiêu chí Reliability của AWS Well-Architected Framework.
