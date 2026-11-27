---
title: "Push the Spring Boot image to Amazon ECR"
menuTitle: "Amazon ECR"
weight: 1
chapter: false
pre: "<b> 5.5.1. </b> "
---

1. **Find Elastic Container Registry in the AWS Console → open Private registry.**

{{< btbfigure file="aws-ecr-step-1.webp" caption="Find Elastic Container Registry in the AWS Console → open Private registry." step="yes" >}}

2. **Choose Repositories → Create repository.** Select Private, enter `generate-ui-be`, choose the tag setting and create the repository. Use a new tag for each immutable build.

{{< btbfigure file="aws-ecr-step-2-create-private-repository.webp" caption="Repository referenced by the task definition." step="yes" >}}

3. **Select the repository → View push commands → Windows.** Open PowerShell in the backend Dockerfile directory. Use Docker, AWS CLI and AWS Tools for PowerShell with the same lab account. The commands follow the four pictured operations and use the commit SHA for the ECR tag.

{{< btbfigure file="aws-ecr-step-3-display-commands.webp" caption="Windows commands in View push commands." step="yes" >}}


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


**Verify.** Open Images and select the uploaded tag. Compare its digest with describe-images and record the URI and digest for the task definition.
