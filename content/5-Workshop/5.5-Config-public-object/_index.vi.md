---
title : "5. Cấu hình public object"
date: 2025-09-09
weight : 5
chapter : false
pre : " <b> 5.5 </b> "
---

#### Cấu hình từng bước

1. Truy cập Quyền Bucket
  - Trong giao diện S3 bucket của bạn, chọn tab **Permissions**

![Permissions](/images/5-Workshop/5.5-Config-public-object/1.png)

2. Tìm Cài đặt Access Control List

Cuộn xuống để tìm phần **Access control list (ACL)**
  - Bạn sẽ thấy **Bucket owner enforced** hiện đang được chọn
  - Điều này có nghĩa là ACL bị tắt và chủ sở hữu bucket kiểm soát tất cả đối tượng

![Access control list](/images/5-Workshop/5.5-Config-public-object/2.png)

3. Bật ACL cho Kiểm soát Cấp Đối tượng
  - Chọn **Edit** trong phần Object Ownership, sau đó cấu hình:
    - Object ownership: Chọn **ACLS enabled**
    - Acknowledgment: Đánh dấu **I acknowledge that ACLs will be restored**
    - **Object ownership setting**: Chọn **Bucket owner preferred**
    - Chọn **Save changes**

![Object ownership](/images/5-Workshop/5.5-Config-public-object/3.png)

4. Xác minh Cấu hình ACL

Sau khi lưu, bạn sẽ thấy ACLs enabled trong phần Object Ownership.
- Cấu hình Đã Cập nhật: Bucket của bạn giờ hỗ trợ ACL, cho phép bạn thiết lập quyền cấp đối tượng bao gồm truy cập công khai.

![Object ownership](/images/5-Workshop/5.5-Config-public-object/4.png)

5. Làm Công khai Đối tượng Sử dụng ACL

Điều hướng trở lại tab Objects của bucket:
  - Chọn các đối tượng hoặc thư mục bạn muốn làm công khai
  - Chọn Actions từ thanh công cụ
  - Chọn Make public using ACL

![Object ownership](/images/5-Workshop/5.5-Config-public-object/5.png)

6.  Xác nhận Truy cập Công khai

Trên trang xác nhận Make public:

  - Xem lại các đối tượng sẽ được làm công khai
  - Hiểu rằng các đối tượng này sẽ có thể truy cập được bởi bất kỳ ai
  - Chọn Make public để xác nhận

**Cảnh báo Cuối cùng**: Khi bạn nhấp “Make public”, các đối tượng này sẽ ngay lập tức có thể truy cập được bởi bất kỳ ai trên internet biết URL.

![Object ownership](/images/5-Workshop/5.5-Config-public-object/6.png)

7. Xác minh Cấu hình Công khai

Thành công! Các đối tượng của bạn giờ có thể truy cập công khai.

![Object ownership](/images/5-Workshop/5.5-Config-public-object/7.png)
