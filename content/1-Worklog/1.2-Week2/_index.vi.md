---
title: "Báo cáo công việc Tuần 2"
date: 2026-09-21
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

- Phân biệt lưu trữ, RDS, Lightsail và shared file system.
- Hiểu Auto Scaling, CloudWatch và Hybrid DNS.

### Các công việc cần triển khai trong tuần này:

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 1 | **Lưu trữ và static website trên S3** <br> - Tìm hiểu lưu trữ trên AWS và cách kết hợp S3 với CloudFront để phân phối website. S3 lưu object; EBS lưu block; EFS/FSx cung cấp file dùng chung. <br> - S3 Versioning giữ phiên bản cũ; lifecycle quản lý thời gian lưu và chi phí. CloudFront cache nội dung; OAC bảo vệ S3 origin thông thường. CORS không cấp quyền S3. | 21/09/2026 | 22/09/2026 | - [Module 1 - 9: Hosting Static Website with Amazon S3](https://000057.awsstudygroup.com/) <br> - [Video lab: Tạo S3 Bucket](https://youtu.be/3vSrTeWroSs?si=KmSKod77TeDMpitx) <br> - [Video: Tổng quan lưu trữ AWS](https://youtu.be/hsCfP0IxoaM) <br> - [Video: S3, CORS, Access Control & Glacier](https://youtu.be/mPBjB6Ltl_Q) <br> - [Video: Snow Family, Storage Gateway & Backup](https://youtu.be/YXn8Q_Hpsu4) <br> - [Docs: S3 Versioning](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html) <br> - [Docs: CloudFront OAC](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html) <br> - [Docs: S3 CORS](https://docs.aws.amazon.com/AmazonS3/latest/userguide/cors.html) |
| 2 | **Tính sẵn sàng và khôi phục RDS** <br> - So sánh các lựa chọn duy trì hoạt động, mở rộng đọc và khôi phục database. Multi-AZ DB instance dùng standby để failover, standby không phục vụ đọc. <br> - Read Replica sao chép bất đồng bộ để mở rộng đọc, có thể trễ dữ liệu. Point-in-time recovery tạo DB instance mới; ứng dụng cần đổi endpoint. | 23/09/2026 | 23/09/2026 | - [Module 1 - 10: Amazon Relational Database Service](https://000005.awsstudygroup.com/) <br> - [Docs: Multi-AZ standby](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZSingleStandby.html) <br> - [Docs: Read Replicas](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ReadRepl.html) <br> - [Docs: Point-in-time recovery](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_PIT.html) |
| 3 | **Lightsail, shared storage và migration** <br> - Tìm hiểu khi nào dùng Lightsail, hệ thống file dùng chung hoặc dịch vụ di chuyển máy chủ. Lightsail dùng gói tài nguyên đơn giản; nâng cấu hình bằng instance lớn hơn từ snapshot. <br> - EFS dùng NFS; FSx hỗ trợ các loại file system theo workload. MGN di chuyển máy chủ; sau migration vẫn cần kiểm tra ứng dụng và kết nối. | 24/09/2026 | 24/09/2026 | - [Module 1 - 11: Amazon Lightsail Workshop](https://000045.awsstudygroup.com/) <br> - [Video: Auto Scaling, EFS/FSx, Lightsail & MGN](https://youtu.be/hFVYG8WqfU0) <br> - [Docs: Lightsail sizing](https://docs.aws.amazon.com/lightsail/latest/userguide/amazon-lightsail-right-size-instance.html) <br> - [Docs: Amazon EFS](https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html) |
| 4 | **Cân bằng tải và Auto Scaling** <br> - Phân tích cách ASG bổ sung/thay thế máy và load balancer phân phối lưu lượng. ASG duy trì desired capacity trong giới hạn min/max; load balancer gửi request tới target khỏe. <br> - Chọn metric và warmup phù hợp cho chính sách scale. Dùng launch template; lưu session và dữ liệu bền ngoài instance. | 25/09/2026 | 25/09/2026 | - [Module 1 - 12: EC2 Auto Scaling Group](https://000006.awsstudygroup.com/) <br> - [Video: EC2 Auto Scaling](https://youtu.be/bbLcPitXJSY) <br> - [Docs: Target tracking and warmup](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-scaling-target-tracking.html) <br> - [Docs: Load balancer targets](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-target-groups.html) |
| 5 | **Giám sát bằng CloudWatch** <br> - Tìm hiểu cách thu thập metric, đọc log và đặt cảnh báo để theo dõi hệ thống. Đọc đúng namespace, dimensions, statistic, period và khoảng thời gian của metric. <br> - EC2 cần agent để thu thập RAM; Logs Insights hỗ trợ phân tích log. Alarm cần ngưỡng, cách xử lý missing data và thông báo SNS phù hợp. | 26/09/2026 | 26/09/2026 | - [Module 1 - 13: Amazon CloudWatch Workshop](https://000008.awsstudygroup.com/) <br> - [Docs: CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Alarms.html) <br> - [Docs: CloudWatch Agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) |
| 6 | **Hybrid DNS với Route 53 Resolver** <br> - Phân tích đường đi truy vấn DNS giữa mạng on-premises và AWS. Inbound Resolver nhận DNS từ on-premises vào AWS; outbound chuyển truy vấn từ VPC ra DNS on-premises. <br> - Cần VPN/Direct Connect, route và TCP/UDP 53 phù hợp. Đặt endpoint qua nhiều AZ, dùng forwarding rule đúng domain và tránh vòng lặp. | 27/09/2026 | 27/09/2026 | - [Module 1 - 14: Set up Hybrid DNS with Route 53 Resolver](https://000010.awsstudygroup.com/) <br> - [Docs: Resolver query paths](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver-overview-DSN-queries-to-vpc.html) |

### Kết quả đạt được tuần 2:

- Phân biệt failover, mở rộng đọc và khôi phục database.
- Hiểu cách kết hợp scaling, metric/alert và DNS với yêu cầu ứng dụng.
