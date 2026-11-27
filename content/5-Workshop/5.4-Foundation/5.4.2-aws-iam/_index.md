---
title: "IAM for ECS and the Spring Boot application"
menuTitle: "AWS IAM"
weight: 2
chapter: false
pre: "<b> 5.4.2. </b> "
---

ECS uses the execution role to start containers. Spring Boot uses the task role to call AWS APIs. Map both roles to the corresponding fields in lab 5.6.3.

## Create the execution role

1. **Search for IAM in the AWS Console and open it.**

{{< btbfigure file="aws-iam-step-1.webp" caption="Search for IAM in the AWS Console and open it." step="yes" >}}

2. **Choose Access management → Roles → Create role.**

{{< btbfigure file="aws-iam-step-2-create-roles.webp" caption="Choose Access management → Roles → Create role." step="yes" >}}

3. **Select the trusted entity.** Choose AWS service → Elastic Container Service → Elastic Container Service Task → Next.

{{< btbfigure file="aws-iam-step-3-select-entity.webp" caption="Select the trusted entity for ECS tasks." step="yes" >}}

4. **Attach AmazonECSTaskExecutionRolePolicy → Next.** This lets ECS pull the ECR image and write logs through awslogs.

{{< btbfigure file="aws-iam-step-4-add-permission.webp" caption="Managed policy for the execution role." step="yes" >}}

5. **Name the role → Create role.** Use `btb-ecs-execution` in the lab. Verify the trust policy names `ecs-tasks.amazonaws.com`.

{{< btbfigure file="aws-iam-step-4-name-review-create.webp" caption="Name the role → Create role." step="yes" >}}

6. **Open the new role → Permissions.** To inject the secret in 5.6.1 choose Add permissions → Create inline policy. In the visual editor choose Secrets Manager, GetSecretValue and the specific secret ARN. For a customer managed KMS key also grant kms:Decrypt on that key.

{{< btbfigure file="aws-iam-step-5-check-permissions.webp" caption="Open the new role → Permissions." step="yes" >}}

## Create the task role and the Nova Pro example policy

7. **Return to Roles → Create role.** Select the ECS Task trusted entity from step 3. Create `btb-backend-bedrock-role`. Open Permissions → Add permissions → Create inline policy → JSON.

8. **Enter the Nova Pro invocation policy.** Copy the JSON below the screenshot. Replace ACCOUNT_ID and verify the inference profile in Bedrock before choosing Next.

{{< btbfigure file="aws-iam-step-6-create-policy.webp" caption="InvokeModel for the Nova Pro inference profile and foundation model." step="yes" >}}


```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "InvokeNovaProProfile",
      "Effect": "Allow",
      "Action": [
        "bedrock:InvokeModel",
        "bedrock:InvokeModelWithResponseStream"
      ],
      "Resource": "arn:aws:bedrock:ap-southeast-1:ACCOUNT_ID:inference-profile/apac.amazon.nova-pro-v1:0"
    },
    {
      "Sid": "InvokeNovaProThroughProfile",
      "Effect": "Allow",
      "Action": [
        "bedrock:InvokeModel",
        "bedrock:InvokeModelWithResponseStream"
      ],
      "Resource": "arn:aws:bedrock:*::foundation-model/amazon.nova-pro-v1:0",
      "Condition": {
        "StringEquals": {
          "bedrock:InferenceProfileArn": "arn:aws:bedrock:ap-southeast-1:ACCOUNT_ID:inference-profile/apac.amazon.nova-pro-v1:0"
        }
      }
    }
  ]
}
```


9. **Set Policy name to btb-nova-pro-invoke → Create policy.** The two statements allow calls to the profile and its foundation model through that profile.

{{< btbfigure file="aws-iam-step-6-review-policy.webp" caption="Set Policy name to btb-nova-pro-invoke → Create policy." step="yes" >}}

## Permissions by backend feature

In **task role → Permissions**, add the permissions required by the enabled features. Use **Create inline policy → Visual** to select each action and resource below.

**Direct SQS delivery · btb-outbox-sqs-send.** The outbox uses `sqs:SendMessage`, `sqs:GetQueueUrl` and `sqs:GetQueueAttributes` on `btb-customer-events`. If the same event is published through EventBridge, check the chosen delivery path to prevent duplicate notifications.

<details>
<summary>View the SQS permission and review captures</summary>

{{< btbfigure file="aws-iam-step-6-create-policy-1.webp" caption="The pictured PublishOutbox policy is for direct SQS delivery." step="yes" >}}

{{< btbfigure file="aws-iam-step-6-review-policy-1.webp" caption="View the SQS permission and review captures" step="yes" >}}

{{< btbfigure file="aws-iam-step-6-review-policy-2.webp" caption="View the SQS permission and review captures" step="yes" >}}

</details>

**Apply a guardrail · btb-bedrock-guardrail.** `bedrock:ApplyGuardrail` lets the application apply its configured filters. Select the guardrail and guardrail profile ARNs used in the lab.

<details>
<summary>View the guardrail and policy name captures</summary>

{{< btbfigure file="aws-iam-step-6-create-policy-2.webp" caption="ApplyGuardrail permission for the guardrail and profile." step="yes" >}}

{{< btbfigure file="aws-iam-step-6-create-policy-3.webp" caption="View the guardrail and policy name captures" step="yes" >}}

{{< btbfigure file="aws-iam-step-6-review-policy-3.webp" caption="View the guardrail and policy name captures" step="yes" >}}

</details>

**Publish domain events · btb-eventbridge-put-events.** `events:PutEvents` lets Spring Boot publish order and payment events to `btb-events`. Configure the rule and target queue in 5.8.6.

<details>
<summary>View the event publishing permission captures</summary>

{{< btbfigure file="aws-iam-step-6-create-policy-4.webp" caption="PutEvents to the BTB custom bus." step="yes" >}}

{{< btbfigure file="aws-iam-step-6-review-policy-4.webp" caption="View the event publishing permission captures" step="yes" >}}

</details>

**Verify.** In ECS → running task → Task definition, check Task role and Task execution role. The ECS capture names the execution role ecsTaskExecutionRole. Select the role configured in your lab account.
