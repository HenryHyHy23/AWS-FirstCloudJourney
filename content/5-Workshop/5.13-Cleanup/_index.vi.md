---
title: "Dọn tài nguyên lab theo thứ tự phụ thuộc"
menuTitle: "Dọn tài nguyên và khắc phục lỗi"
weight: 13
chapter: false
pre: "<b> 5.13. </b> "
---



Chỉ làm với tài nguyên riêng của buổi thực hành. Trước khi xóa, đối chiếu account, Region, tên/tag và các association. Lưu kết quả demo, số revision và số liệu chi phí cần báo cáo.

## 1. Dừng nguồn phát sinh dữ liệu

Tắt trigger deploy của lab; gỡ RUM client khỏi bản FE lab. Disable rule EventBridge, gỡ target rồi xóa rule. Kiểm tra queue chính/DLQ và archive; sau khi xử lý hoặc lưu message demo cần thiết, xóa archive, custom bus và queue lab. Xóa alarm trước topic SNS và subscription.

## 2. Gỡ đường vào và compute

Gỡ Alias FE/API của lab và xác nhận hostname nào dùng chung. Gỡ association WAF khỏi distribution/ALB, gỡ function association, disable CloudFront rồi đợi deployed trước khi xóa distribution và OAC riêng. Xóa function/ACL riêng sau khi hết association.

ECS service → desired count 0 → đợi task dừng → xóa service và cluster lab. Xóa ALB (tắt deletion protection riêng cho ALB lab nếu đã bật), sau đó target group. Deregister task revision lab nếu không còn dùng. Xóa cert ACM sau khi hết attachment.

## 3. Dọn lưu trữ, quyền và mạng

Empty bucket lab, gồm version và delete marker nếu bật versioning, rồi xóa bucket FE/log riêng. Xóa image và repo ECR lab. Lên lịch xóa secret với recovery window. Xóa RUM monitor, identity pool, flow log delivery, log group và role/policy lab khi mọi producer đã dừng.

Nếu lab tạo NAT, xóa NAT → chờ Deleted → release Elastic IP. Xóa endpoint riêng, kiểm tra ENI, rồi dọn security group, subnet, route table tùy chỉnh; detach/delete IGW, xóa VPC lab. Chỉ xóa public zone khi domain đã chuyển DNS hoặc hết dùng và đã dọn record ngoài NS/SOA.

## 4. Kiểm tra sau buổi thực hành

Kiểm tra cả Singapore và us-east-1. Đối chiếu ECS running tasks, ALB, NAT/IPv4, CloudFront, log và dữ liệu tích trữ trong S3/ECR. Sau khi billing cập nhật, xem Cost Explorer với bộ lọc service/date. Giữ budget cảnh báo nếu tiếp tục dùng tài khoản.
