---
title: "Mạng cho ALB và các task Fargate"
menuTitle: "Amazon VPC"
weight: 3
chapter: false
pre: "<b> 5.4.3. </b> "
---

Chuẩn bị hai AZ tại Singapore cho ALB và đường mạng từ ALB đến task Fargate cổng 8080.

## VPC và subnet

1. **Tìm VPC trong AWS Console.**

{{< btbfigure file="aws-vpc-step-1.webp" caption="Tìm VPC trong AWS Console." step="yes" >}}

2. **Chọn Your VPCs → Create VPC.**

{{< btbfigure file="aws-vpc-step-2-create-vpc.webp" caption="Chọn Your VPCs → Create VPC." step="yes" >}}

3. **Chọn VPC only.** Nhập tên lab và IPv4 CIDR `10.0.0.0/16`, sau đó bấm Create VPC.

{{< btbfigure file="aws-vpc-step-3-setup-vpc.webp" caption="Thông số VPC trong màn hình tạo." step="yes" >}}

4. **Chọn VPC → Actions → Edit VPC settings.** Bật DNS resolution và DNS hostnames → Save.

{{< btbfigure file="aws-vpc-step-4-enable-dns.webp" caption="DNS trong VPC settings." step="yes" >}}

5. **Mở Subnets → Create subnet.** Chọn VPC vừa tạo. Chọn Availability Zone cho subnet đầu tiên.

{{< btbfigure file="aws-vpc-step-5-create-subnet.webp" caption="Mở Subnets → Create subnet." step="yes" >}}

6. **Nhập tên và CIDR của từng subnet.** Ảnh là private A. Tạo tiếp các subnet trong cùng VPC. Ví dụ cho lab:
   - Public A/B — `10.0.0.0/24`, `10.0.1.0/24` tại hai AZ
   - Private A/B — `10.0.128.0/20`, `10.0.144.0/20` tại hai AZ

{{< btbfigure file="aws-vpc-step-5-setup-subnet.webp" caption="Subnet private đầu tiên và AZ tương ứng." step="yes" >}}

## Internet Gateway và route table

7. **Mở Internet gateways → Create internet gateway.**

{{< btbfigure file="aws-vpc-step-6-create-internet-gateways.webp" caption="Mở Internet gateways → Create internet gateway." step="yes" >}}

8. **Nhập Name tag cho gateway → Create internet gateway.**

{{< btbfigure file="aws-vpc-step-6-name-internet-gateways.webp" caption="Nhập Name tag cho gateway → Create internet gateway." step="yes" >}}

9. **Chọn Actions → Attach to VPC.** Chọn VPC vừa tạo → Attach internet gateway.

{{< btbfigure file="aws-vpc-step-7-attach.webp" caption="Gắn gateway vào VPC của lab." step="yes" >}}

10. **Mở Route tables → Create route table.** Nhập tên cho bảng public, chọn VPC lab rồi bấm Create.

{{< btbfigure file="aws-vpc-step-8-create-route-table.webp" caption="Mở Route tables → Create route table." step="yes" >}}

11. **Chọn Routes → Edit routes → Add route.** Destination `0.0.0.0/0`, target Internet Gateway vừa tạo → Save changes. Trong Subnet associations gắn hai subnet public.

{{< btbfigure file="aws-vpc-step-9-edit-routes.webp" caption="Default route của bảng public trỏ đến Internet Gateway." step="yes" >}}

12. **Chuẩn bị egress cho task private.** Nếu lab dùng NAT, tạo public NAT Gateway trong subnet public, gắn Elastic IP, đợi Available rồi thêm default route tới NAT trong **bảng private**. Nếu dùng endpoints, kiểm tra cả đường ra tới Supabase và SMTP. NAT phát sinh phí theo giờ và dữ liệu.

13. **Mở Security groups → Create security group.** Tạo hai group trong VPC.
    - ALB-SG — inbound HTTPS 443 từ nguồn dự kiến, outbound TCP 8080 tới TASK-SG
    - TASK-SG — inbound TCP 8080 từ ALB-SG; cho phép outbound tới các dependency của backend

{{< btbfigure file="aws-vpc-step-10-create-security-groups.webp" caption="Màn hình tạo security group trong VPC." step="yes" >}}

**Kiểm tra kết quả.** Chọn từng subnet → Route table, kiểm tra route đích và association. Ở bài ECS, target phải thành Healthy khi ALB kiểm tra cổng 8080.
