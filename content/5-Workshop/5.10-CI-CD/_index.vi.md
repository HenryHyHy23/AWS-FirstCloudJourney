---
title: "Từ commit GitHub đến bản phát hành BTB"
menuTitle: "Triển khai CI/CD"
weight: 10
chapter: false
pre: "<b> 5.10. </b> "
---



Tài liệu dự án mô tả deploy FE và BE từ nhánh feat/prod. Mở repo ứng dụng → .github/workflows và tab Actions để lấy trigger, môi trường và run ID thực tế. Workflow gh-pages trong repo Hugo dùng để xuất bản workshop.

## 1. Xác thực runner với AWS

Nếu dùng GitHub OIDC, trong IAM cấu hình provider token.actions.githubusercontent.com, audience sts.amazonaws.com. Hạn chế sub trong trust policy theo repo/branch hoặc environment đúng với job. Job cần permissions id-token: write và contents: read. Role deploy FE cần quyền upload prefix S3 và invalidation distribution lab; role BE cần push ECR, đăng ký revision, cập nhật service và PassRole giới hạn ở hai role ECS.

## 2. Theo bản FE

Trong repo FE, đối chiếu SHA của run với checkout, npm ci và bước build. Upload asset có hash trước rồi cập nhật index.html. Invalidate /index.html và đường gốc khi cần. Mở View Source hoặc Network để đối chiếu tên asset mà HTML mới trỏ tới. Rollback bằng bản build trước còn nguyên file asset và cập nhật lại HTML.

## 3. Theo bản BE

Từ run BE lấy SHA/tag → ECR image digest → task definition revision dùng đúng image → ECS service deployment. Đợi service ổn định và kiểm tra ALB Healthy. Chỉ khi cả hai điều kiện đạt mới đánh dấu deploy thành công. Nếu service quay lại revision cũ, mở ECS Events và stopped reason để xác định task hỏng ở pull image, startup hay health check.


```powershell
$region = "ap-southeast-1"
aws ecs describe-services --cluster LAB_CLUSTER --services LAB_SERVICE --region $region --query "services[].{running:runningCount,desired:desiredCount,revision:taskDefinition}"
aws elbv2 describe-target-health --target-group-arn LAB_TARGET_GROUP_ARN --region $region --query "TargetHealthDescriptions[].{port:Target.Port,state:TargetHealth.State,reason:TargetHealth.Reason}"
```


Lưu cùng một lần deploy: URL Actions run, commit SHA, image digest, revision và thời gian target healthy. Chưa có Actions run trong bộ ảnh này.
