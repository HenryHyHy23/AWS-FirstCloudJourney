---
title : "Block public access"
date: 2025-09-09
weight : 1
chapter : false
pre : " <b> 5.7.1 </b> "
---

1. Trong giao diện S3 bucket
    - Chọn **Permissions**
    - Hiện tại, chức năng **Block all public access** đang **Off** do chúng ta đã bật tắt tại bước 4
    - Chọn **Edit**
![Block public access](/images/5-Workshop/5.7-Cloudfront/5.7.1-Block-public-access/1.png)
    -   Chọn **Block all public access**
    -   Chọn **Save changes**

![Block public access](/images/5-Workshop/5.7-Cloudfront/5.7.1-Block-public-access/2.png)
    - Một cửa số khác xuất hiện để xác nhận việc **edit**, điền `confirm`
    - Chọn **Confirm**

![Block public access](/images/5-Workshop/5.7-Cloudfront/5.7.1-Block-public-access/3.png)
    -   **Block all public access** đã được bật, lúc này sẽ không có bất kỳ một truy cập công cộng nào có thể kết nối được với S3 Bucket của bạn.

![Block public access](/images/5-Workshop/5.7-Cloudfront/5.7.1-Block-public-access/4.png)

2. Kiểm tra chức năng Block all public access
    -   Trong giao diện S3 bucket
    - Chọn **Properties**

![Block public access](/images/5-Workshop/5.7-Cloudfront/5.7.1-Block-public-access/5.png)
    -   Cuộn xuống cuối trang, tại mục **Static website hosting**
    -   Chọn ký hiệu **ô vuông** để copy URL

![Block public access](/images/5-Workshop/5.7-Cloudfront/5.7.1-Block-public-access/6.png)
    -   Mở URL trong tab mới của trình duyệt

![Block public access](/images/5-Workshop/5.7-Cloudfront/5.7.1-Block-public-access/7.png)

-> Chúc mừng bạn đã Block public access thành công