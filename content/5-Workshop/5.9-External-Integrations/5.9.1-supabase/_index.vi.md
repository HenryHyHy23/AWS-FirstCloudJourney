---
title: "Nối Spring Boot với Supabase PostgreSQL"
menuTitle: "Supabase PostgreSQL và xác thực"
weight: 1
chapter: false
pre: "<b> 5.9.1. </b> "
---

1. **Mở Supabase Dashboard → project lab → Connect.** Chọn direct connection hoặc Session pooler theo đường mạng của task. Nếu task chỉ có egress IPv4 và direct endpoint cần IPv6, chọn Session pooler. Ghi host, port, database và user hiển thị.

2. **Mở Secrets Manager → secret backend → Edit.** Điền SUPABASE_DB_URL dạng JDBC với TLS theo project, SUPABASE_DB_USERNAME và SUPABASE_DB_PASSWORD. Trong ECS map từng tên qua ValueFrom rồi deploy revision mới.

{{< btbfigure file="aws-secret-manager-step-1.webp" caption="Các key kết nối xuất hiện trong ảnh Secrets Manager." step="yes" >}}

3. **Trong Supabase, mở SQL Editor → New query → Run.** Kiểm tra database và role đang dùng.


```sql
select current_database() as database_name, current_user as role_name;
select 1 as connection_ok;
```


4. **Mở catalog BTB → tìm một SKU demo đã biết.** Đối chiếu dữ liệu trả về với project lab. Nếu có lỗi kết nối, kiểm tra username của pooler, JDBC URL, task revision và khả năng egress.

**Kiểm tra kết quả.** SQL Editor xác nhận phía DB. Truy vấn catalog từ backend kiểm tra được kết nối thực từ ECS. Xác định Auth/JWT theo cấu hình Spring Security của phiên bản đang chạy.
