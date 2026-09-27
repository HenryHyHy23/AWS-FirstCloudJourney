---
title : "3. Bật tính năng Host Static Website từ S3 bucket"
date: 2025-09-09
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

Trong phần này, bạn sẽ mở tính năng host website tĩnh từ S3 bucket.

#### Cấu hình từng bước
1. Truy cập Properties của Bucket
    - Trong giao diện S3 bucket của bạn, chọn tab Properties

![Properties](/images/5-Workshop/5.3-S3-static-web/1.png)

2. Tìm **Static Website Hosting**

Trong giao diện Properties:
- Cuộn xuống để tìm phần **Static website hosting**
- Chọn **Edit** để sửa đổi cài đặt
![Static Website Hosting](/images/5-Workshop/5.3-S3-static-web/2.png)

3. Cấu hình cài đặt Website Hosting

    - **Static website hosting**: Chọn Enable
    - **Hosting type**: Chọn Host a static website
    - **Index document**: Nhập `index.html`
    - **Error document** (tùy chọn): Nhập `error.html` nếu bạn có trang lỗi tùy chỉnh

![Static Website Hosting](/images/5-Workshop/5.3-S3-static-web/3.png)


4. Lưu cấu hình
![Static Website Hosting](/images/5-Workshop/5.3-S3-static-web/4.png)

### Lưu ý:

Sau khi bật static website hosting, bucket của bạn sẽ có URL website endpoint, nhưng nội dung sẽ không thể truy cập được cho đến khi bạn:
- Tải lên các tệp website của bạn
- Cấu hình quyền truy cập công khai
- Thiết lập chính sách bucket phù hợp

5. Xác minh cấu hình
#### Những gì đã thực hiện được
1.  Bật tính năng host website tĩnh từ S3 bucket
2.  Thiết lập cấu hình website hosting
3.  Có URL website endpoint


