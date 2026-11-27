---
title: "Verify Bedrock model permissions and quotas"
menuTitle: "Amazon Bedrock"
weight: 4
chapter: false
pre: "<b> 5.6.4. </b> "
---

1. **Open Amazon Bedrock in Singapore → Quotas.** Filter by model and inference type. The policy in 5.4.2 covers APAC Nova Pro. The quota dialogs below are for Nova 2 Lite global cross-region.

{{< btbfigure file="aws-bedrock-step-1.webp" caption="Quotas by model and inference type." step="yes" >}}

2. **Select the model → Request RPM quota increase.** Read the requests-per-minute quota. The capture shows an outstanding request. Check its status in Service Quotas.

{{< btbfigure file="aws-bedrock-step-2-request-rpm.webp" caption="The RPM dialog reports a pending request." step="yes" >}}

3. **Open Request TPM quota increase.** Check tokens per minute for the correct model. If an increase is needed, enter the desired total for the workload and track the request separately.

{{< btbfigure file="aws-bedrock-step-2-request-tpm.webp" caption="Nova 2 Lite TPM request dialog." step="yes" >}}

**Verify.** Send a question through BTB using a lab account. Match the timestamp/request ID with ECS logs. For AccessDenied check task-role permissions and profile destination Regions; for ThrottlingException check the correct RPM/TPM. Production endpoint response is still unverified.
