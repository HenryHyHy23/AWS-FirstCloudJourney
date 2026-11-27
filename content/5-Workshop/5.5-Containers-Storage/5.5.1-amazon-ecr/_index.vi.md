---
title: "Đưa image Spring Boot lên Amazon ECR"
menuTitle: "Amazon ECR"
weight: 1
chapter: false
pre: "<b> 5.5.1. </b> "
---

1. **Tìm Elastic Container Registry trong AWS Console → mở Private registry.**

![Tìm Elastic Container Registry trong AWS Console → mở Private registry.](/images/5-Workshop/5.5-Containers-Storage/5.5.1-amazon-ecr/aws-ecr-step-1.webp)

2. **Chọn Repositories → Create repository.** Chọn Private, nhập `generate-ui-be`, chọn chế độ tag rồi bấm Create. Với immutable tags, dùng tag mới cho từng build.

![Tên repository mà task definition sẽ tham chiếu.](/images/5-Workshop/5.5-Containers-Storage/5.5.1-amazon-ecr/aws-ecr-step-2-create-private-repository.webp)

3. **Chọn repository → View push commands → Windows.** Mở PowerShell tại thư mục Dockerfile của BE. Dùng Docker, AWS CLI và AWS Tools for PowerShell cùng tài khoản lab. Đoạn dưới làm theo bốn thao tác trong ảnh và dùng commit SHA làm tag đẩy lên ECR.

![Lệnh Windows trong View push commands.](/images/5-Workshop/5.5-Containers-Storage/5.5.1-amazon-ecr/aws-ecr-step-3-display-commands.webp)


```powershell
$region = "ap-southeast-1"
$account = aws sts get-caller-identity --query Account --output text
$registry = "$account.dkr.ecr.$region.amazonaws.com"
$tag = git rev-parse --short HEAD
(Get-ECRLoginCommand -Region $region).Password | docker login --username AWS --password-stdin $registry
docker build --platform linux/amd64 -t generate-ui-be .
docker tag generate-ui-be:latest "${registry}/generate-ui-be:${tag}"
docker push "${registry}/generate-ui-be:${tag}"
aws ecr describe-images --region $region --repository-name generate-ui-be --image-ids imageTag=$tag --query "imageDetails[0].imageDigest"
```


**Kiểm tra kết quả.** Mở Images → chọn tag vừa push. So digest với kết quả describe-images. Ghi lại URI và digest cho task definition.
