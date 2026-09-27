---
title : "Tạo S3 bucket"
date: 2025-09-09
weight : 1
chapter : false
pre : " <b> 5.2.1 </b> "
---

### Tạo S3 bucket
#### S3 Bucket là gì?
S3 bucket là một container chứa các đối tượng được lưu trữ trong Amazon S3. Hãy nghĩ về nó như một thư mục cấp cao nhất chứa các tệp của bạn. Mỗi bucket có tên duy nhất trên toàn cầu và tồn tại trong một vùng AWS cụ thể. Bucket đóng vai trò là đơn vị tổ chức cơ bản trong S3 và cung cấp không gian tên cho việc lưu trữ đối tượng.

#### Hướng Dẫn Từng Bước
1. Truy cập Dịch vụ S3
    - Truy cập **AWS Management Console**
    - Tìm **S3** bằng thanh tìm kiếm hoặc điều hướng đến dịch vụ Storage
    - Chọn **S3**

2. Khởi tạo Tạo Bucket
    -   Trong giao diện **S3**, chọn **Create bucket**

3. Cấu hình Cài đặt Bucket Cơ bản

-   Trong giao diện **Create bucket**:
    - **Bucket name**: Nhập `workshop-demo-092025` (nếu trùng tên thì thêm số vào hoặc đặt tên tùy chỉnh)
    - **AWS Region**: Chọn vùng gần nhất với người dùng của bạn hoặc theo yêu cầu
    - **Object Ownership**: Chọn **ACLs disabled (recommended)**

![Create S3 Bucket](/images/5-Workshop/5.2-Prerequisite/5.2.1-Create-S3-bucket/1.png)
![Create S3 Bucket](/images/5-Workshop/5.2-Prerequisite/5.2.1-Create-S3-bucket/2.png)
![Create S3 Bucket](/images/5-Workshop/5.2-Prerequisite/5.2.1-Create-S3-bucket/3.png)
![Create S3 Bucket](/images/5-Workshop/5.2-Prerequisite/5.2.1-Create-S3-bucket/4.png)
![Create S3 Bucket](/images/5-Workshop/5.2-Prerequisite/5.2.1-Create-S3-bucket/5.png)

4. Xác minh Tạo Bucket

Thành công! Bạn đã tạo S3 bucket để lưu trữ mã nguồn website. Bucket sẽ được hiển thị ở **General purpose buckets**

![Create S3 Bucket](/images/5-Workshop/5.2-Prerequisite/5.2.1-Create-S3-bucket/6.png)

#### Những gì đã thực hiện được
1.  Tạo S3 bucket duy nhất trên toàn cầu
2.  Cấu hình cài đặt mặc định của S3
3.  Thiết lập nền tảng lưu trữ website tĩnh

#### Tiếp theo
Tiếp theo, chúng ta sẽ tải mã nguồn website vào S3 bucket và lưu trữ.