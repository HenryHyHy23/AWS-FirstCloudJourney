---
title: "Báo cáo công việc Tuần 7"
date: 2026-10-26
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

- Hiểu cách CodePipeline điều phối source, build/test và deploy ứng dụng Node.js lên EC2.
- Phân biệt buildspec, AppSpec, artifact và vai trò IAM trong quá trình triển khai.

### Các công việc cần triển khai trong tuần này:

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 1 | **Triển khai EC2 với AWS CodePipeline** <br> - Tìm hiểu pipeline triển khai Node.js lên EC2: commit vào CodeCommit kích hoạt CodePipeline qua EventBridge, rồi chuyển qua các bước build/test và deploy. <br> - CodeBuild chạy lệnh trong `buildspec.yml`, tạo artifact lưu trên S3. CodeDeploy dùng `appspec.yml` để biết file cần cài và các script hook như BeforeInstall, ApplicationStart. <br> - EC2 cần CodeDeploy Agent hoạt động, instance profile đọc được artifact và tag khớp deployment group. Phân biệt quyền service role với quyền của EC2; xét thêm quyền KMS khi artifact mã hóa bằng khóa KMS. <br> - Đọc log từng stage và lifecycle event để phân loại lỗi source, build, tải artifact hay khởi động ứng dụng. Kiểm tra ứng dụng qua HTTP sau deployment; build thành công chưa đủ xác nhận bản chạy đúng. | 26/10/2026 | 26/10/2026 | - [Module 3 - 21: Deploy applications to EC2 with AWS CodePipeline](https://000023.awsfcaj.com/) <br> - [AWS: CodePipeline concepts](https://docs.aws.amazon.com/codepipeline/latest/userguide/concepts.html) <br> - [AWS: CodeCommit EventBridge trigger](https://docs.aws.amazon.com/codepipeline/latest/userguide/pipelines-trigger-source-repo-changes-cli.html) <br> - [AWS: CodeDeploy AppSpec](https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file.html) <br> - [AWS: EC2 deployment prerequisites](https://docs.aws.amazon.com/codedeploy/latest/userguide/instances-ec2-configure.html) |

### Kết quả đạt được tuần 7:

- Phân biệt công việc của CodeCommit, CodeBuild, CodeDeploy và CodePipeline.
- Hiểu cách kiểm tra artifact, agent, quyền truy cập và lifecycle hook để tìm lỗi; xác nhận ứng dụng sau deployment.
