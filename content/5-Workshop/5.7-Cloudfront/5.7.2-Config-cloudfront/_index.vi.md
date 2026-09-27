---
title : "Cấu hình Cloudfront"
date: 2025-09-09
weight : 2
chapter : false
pre : " <b> 5.7.2 </b> "
---

Sử dụng bảng điều khiển quản lý AWS để tạo **CloudFront distribution** và cấu hình dịch vụ này phục vụ **s3 Bucket** mà chúng ta đã tạo trước đó.

1. Mở bảng điều khiển CLoudfront
    -   Ở AWS Console, tìm kiếm **CloudFront**

    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/1.png)

    Từ bảng điều khiển, nhấn vào Create a CloudFront distribution.
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/2.png)

2. Cấu hình Cloudfront
    -   Trong giao diện chọn kế hoạch, chọn **Free plan**
    -   Chọn Next
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/3.png)

    -   Distribution name: để tên tùy chỉnh (hoặc `workshop-demo`)
    -   Distribution type: chọn **Single website or app**
    -   Chọn Next
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/4.png)

    - Ở trang Specify Origin
        -   Origin type: chọn **Amazon S3**
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/5.png)
         -   Origin: chọn **S3 bucket** đã tạo 
         -  Nhấn **Choose**
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/6.png)
        -   Sau đó nhấn Next
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/7.png)
        -   Tiếp tục Next
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/8.png)
        -   Kiểm tra lại cấu hình, và sau đó chọn **Create distribution**
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/9.png)

Sau đó đợi chờ vài phút để Cloudfront khởi tạo

![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/10.png)

Qua S3, vào **Permissions** và kéo xuống tìm **Bucket policy**
![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/11.png)


#### Optional: Nếu không hoạt động thì thử các cách sau:
1. Tắt Host static website của S3
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/12.png)  
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/13.png)   
2. Sau đó vào cấu hình Cloudfront
    -   Nhấp vào **ID** Cloudfront
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/14.png)

    -   Trong **General**, chọn **Edit**
    -   Thêm `index.html` vào **Default root object**
    -   Sau đó chọn **Save changes**
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/15.png)

Sau đó chờ thêm vài phút để Cloudfront khởi tạo lại, và thử mở lại URL trong trang mới (hoặc trình duyệt mới)
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/16.png)
