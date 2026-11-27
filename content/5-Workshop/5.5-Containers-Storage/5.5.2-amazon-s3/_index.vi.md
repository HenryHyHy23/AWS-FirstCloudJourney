---
title: "Lưu bản build React trong S3"
menuTitle: "Amazon S3"
weight: 2
chapter: false
pre: "<b> 5.5.2. </b> "
---

1. **Tìm Amazon S3 → mở Buckets.**

![Tìm Amazon S3 → mở Buckets.](/images/5-Workshop/5.5-Containers-Storage/5.5.2-amazon-s3/aws-s3-step-1.webp)

2. **Chọn Create bucket → General purpose.**

![Chọn Create bucket → General purpose.](/images/5-Workshop/5.5-Containers-Storage/5.5.2-amazon-s3/aws-s3-step-2-create-bucket.webp)

3. **Nhập Bucket name và AWS Region.** Chọn tên hợp lệ, duy nhất. Ghi lại tên bucket để upload frontend và cấu hình CloudFront origin.

![General configuration của bucket frontend.](/images/5-Workshop/5.5-Containers-Storage/5.5.2-amazon-s3/aws-s3-step-3-general-configuration.webp)

4. **Chọn Bucket owner enforced.** Giữ Block all public access, chọn default encryption rồi Create bucket. CloudFront sẽ đọc qua OAC ở 5.7.2.

![Object Ownership và Block Public Access.](/images/5-Workshop/5.5-Containers-Storage/5.5.2-amazon-s3/aws-s3-step-4-setup-ownership.webp)

5. **Build và upload storefront.** Ở repo FE, đặt API base URL cho lab. Thay BUCKET_NAME và BUCKET_REGION trước khi chạy lệnh.


```powershell
npm ci
npm run build
Get-ChildItem dist
aws s3 sync dist/ s3://BUCKET_NAME/ --region BUCKET_REGION
```


**Kiểm tra kết quả.** Trong Objects, kiểm tra index.html và assets/ cùng nằm ở root. Sau khi nối CloudFront, mở một asset JS và kiểm tra Content-Type.
