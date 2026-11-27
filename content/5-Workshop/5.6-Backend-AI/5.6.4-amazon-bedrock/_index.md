---
title: "Verify Bedrock model permissions and quotas"
menuTitle: "Amazon Bedrock"
weight: 4
chapter: false
pre: "<b> 5.6.4. </b> "
---

1. **Open Amazon Bedrock in Singapore → Quotas.** Filter by model and inference type. The policy in 5.4.2 covers APAC Nova Pro. The quota dialogs below are for Nova 2 Lite global cross-region.

![Quotas by model and inference type.](/images/5-Workshop/5.6-Backend-AI/5.6.4-amazon-bedrock/aws-bedrock-step-1.webp)

2. **Select the model → Request RPM quota increase.** Read the requests-per-minute quota. The capture shows an outstanding request. Check its status in Service Quotas.

![The RPM dialog reports a pending request.](/images/5-Workshop/5.6-Backend-AI/5.6.4-amazon-bedrock/aws-bedrock-step-2-request-rpm.webp)

3. **Open Request TPM quota increase.** Check tokens per minute for the correct model. If an increase is needed, enter the desired total for the workload and track the request separately.

![Nova 2 Lite TPM request dialog.](/images/5-Workshop/5.6-Backend-AI/5.6.4-amazon-bedrock/aws-bedrock-step-2-request-tpm.webp)

**Verify.** Send a question through BTB using a lab account. Match the timestamp/request ID with ECS logs. For AccessDenied check task-role permissions and profile destination Regions; for ThrottlingException check the correct RPM/TPM. Production endpoint response is still unverified.
