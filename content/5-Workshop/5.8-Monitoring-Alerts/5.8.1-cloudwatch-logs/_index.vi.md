---
title: "Theo dõi log ứng dụng và lưu lượng mạng"
menuTitle: "Amazon CloudWatch Logs"
weight: 1
chapter: false
pre: "<b> 5.8.1. </b> "
---

1. **Mở CloudWatch Console → Logs → Log management → Create log group.** Điền các trường sau.
   - Log group name — `/aws/vpc/flowlogs` như ảnh
   - Log class — Standard
   - Retention — ảnh chọn Never expire; có thể chọn 7 ngày cho lab, rồi bấm Create

{{< btbfigure file="aws-cloudwatch-step-1.webp" caption="Tên group trong ảnh là /aws/vpc/flowlogs." step="yes" >}}

2. **Mở VPC → Your VPCs → chọn VPC → Flow logs → Create flow log.** Chọn All hoặc Reject, destination CloudWatch Logs, group vừa tạo và delivery role có quyền ghi. Hoàn tất Create flow log.

3. **Tạo group riêng cho backend.** Quay lại CloudWatch → Create log group → nhập `/ecs/btb-backend`. Trong ECS task definition chọn awslogs, group này, Region Singapore và prefix ecs. Deploy revision mới.

4. **Mở log stream theo task ID.** Ghi giờ gọi /api/health và so với log ứng dụng, hoặc đọc bằng lệnh sau.


```powershell
aws logs tail /ecs/btb-backend --since 10m --region ap-southeast-1
```


**Kiểm tra kết quả.** Group ECS có log khởi động Spring. Group VPC có flow records khi traffic phù hợp đi qua. Để tìm mã HTTP của ALB, mở access logs trong S3 theo 5.6.2.
