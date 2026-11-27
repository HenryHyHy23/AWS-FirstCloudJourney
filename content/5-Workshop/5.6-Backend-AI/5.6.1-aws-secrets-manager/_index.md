---
title: "Store backend configuration in Secrets Manager"
menuTitle: "AWS Secrets Manager"
weight: 1
chapter: false
pre: "<b> 5.6.1. </b> "
---

1. **Open Secrets Manager → Store a new secret → Other type of secret.** Under Key/value enter the names Spring Boot reads and their values from your lab project. The capture shows the key column.

![Connection keys shown in the Secrets Manager capture.](/images/5-Workshop/5.6-Backend-AI/5.6.1-aws-secrets-manager/aws-secret-manager-step-1.webp)

2. **Choose Next → Configure secret.** Enter `btb-backend` and a description. Use the ECS Region.

![Secret name and description.](/images/5-Workshop/5.6-Backend-AI/5.6.1-aws-secrets-manager/aws-secret-manager-step-2-configure.webp)

3. **Open Configure rotation.** For a lab using manually managed credentials leave automatic rotation off. To enable rotation first configure matching provider-side updates. Choose Next.

![Open Configure rotation.](/images/5-Workshop/5.6-Backend-AI/5.6.1-aws-secrets-manager/aws-secret-manager-step-3-rotation.webp)

4. **Review and choose Store.** Check the name and encryption key. Copy the complete secret ARN, including its AWS-generated suffix.

![Verify the secret type and encryption key on Review.](/images/5-Workshop/5.6-Backend-AI/5.6.1-aws-secrets-manager/aws-secret-manager-step-4-review.webp)

5. **Open ECS → Task definitions → Create new revision → Container → Environment variables.** Choose ValueFrom for each key using `SECRET_ARN:JSON_KEY::`. Grant GetSecretValue to the execution role. Use Linux Fargate 1.4.0 or later for individual JSON keys.

**Verify.** After deploying a new task, read the catalog through the API. For ResourceInitializationError check the ARN, JSON key name, execution role permission and egress to Secrets Manager.
