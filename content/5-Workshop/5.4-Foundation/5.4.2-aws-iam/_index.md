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

![Search for IAM in the AWS Console and open it.](/images/5-Workshop/5.4-Foundation/5.4.2-aws-iam/aws-iam-step-1.webp)

2. **Choose Access management → Roles → Create role.**

![Choose Access management → Roles → Create role.](/images/5-Workshop/5.4-Foundation/5.4.2-aws-iam/aws-iam-step-2-create-roles.webp)

3. **Select the trusted entity.** Choose AWS service → Elastic Container Service → Elastic Container Service Task → Next.

![Select the trusted entity for ECS tasks.](/images/5-Workshop/5.4-Foundation/5.4.2-aws-iam/aws-iam-step-3-select-entity.webp)

4. **Attach AmazonECSTaskExecutionRolePolicy → Next.** This lets ECS pull the ECR image and write logs through awslogs.

![Managed policy for the execution role.](/images/5-Workshop/5.4-Foundation/5.4.2-aws-iam/aws-iam-step-4-add-permission.webp)

5. **Name the role → Create role.** Use `btb-ecs-execution` in the lab. Verify the trust policy names `ecs-tasks.amazonaws.com`.

![Name the role → Create role.](/images/5-Workshop/5.4-Foundation/5.4.2-aws-iam/aws-iam-step-4-name-review-create.webp)

6. **Open the new role → Permissions.** To inject the secret in 5.6.1 choose Add permissions → Create inline policy. In the visual editor choose Secrets Manager, GetSecretValue and the specific secret ARN. For a customer managed KMS key also grant kms:Decrypt on that key.

![Open the new role → Permissions.](/images/5-Workshop/5.4-Foundation/5.4.2-aws-iam/aws-iam-step-5-check-permissions.webp)

## Create the task role and the Nova Pro example policy

7. **Return to Roles → Create role.** Select the ECS Task trusted entity from step 3. Create `btb-backend-bedrock-role`. Open Permissions → Add permissions → Create inline policy → JSON.

8. **Enter the Nova Pro invocation policy.** Copy the JSON below the screenshot. Replace ACCOUNT_ID and verify the inference profile in Bedrock before choosing Next.

![InvokeModel for the Nova Pro inference profile and foundation model.](/images/5-Workshop/5.4-Foundation/5.4.2-aws-iam/aws-iam-step-6-create-policy.webp)


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

![Set Policy name to btb-nova-pro-invoke → Create policy.](/images/5-Workshop/5.4-Foundation/5.4.2-aws-iam/aws-iam-step-6-review-policy.webp)

## Permissions by backend feature

In **task role → Permissions**, add the permissions required by the enabled features. Use **Create inline policy → Visual** to select each action and resource below.

**Direct SQS delivery · btb-outbox-sqs-send.** The outbox uses `sqs:SendMessage`, `sqs:GetQueueUrl` and `sqs:GetQueueAttributes` on `btb-customer-events`. If the same event is published through EventBridge, check the chosen delivery path to prevent duplicate notifications.

<details>
<summary>View the SQS permission and review captures</summary>

![The pictured PublishOutbox policy is for direct SQS delivery.](/images/5-Workshop/5.4-Foundation/5.4.2-aws-iam/aws-iam-step-6-create-policy-1.webp)

![View the SQS permission and review captures](/images/5-Workshop/5.4-Foundation/5.4.2-aws-iam/aws-iam-step-6-review-policy-1.webp)

![View the SQS permission and review captures](/images/5-Workshop/5.4-Foundation/5.4.2-aws-iam/aws-iam-step-6-review-policy-2.webp)

</details>

**Apply a guardrail · btb-bedrock-guardrail.** `bedrock:ApplyGuardrail` lets the application apply its configured filters. Select the guardrail and guardrail profile ARNs used in the lab.

<details>
<summary>View the guardrail and policy name captures</summary>

![ApplyGuardrail permission for the guardrail and profile.](/images/5-Workshop/5.4-Foundation/5.4.2-aws-iam/aws-iam-step-6-create-policy-2.webp)

![View the guardrail and policy name captures](/images/5-Workshop/5.4-Foundation/5.4.2-aws-iam/aws-iam-step-6-create-policy-3.webp)

![View the guardrail and policy name captures](/images/5-Workshop/5.4-Foundation/5.4.2-aws-iam/aws-iam-step-6-review-policy-3.webp)

</details>

**Publish domain events · btb-eventbridge-put-events.** `events:PutEvents` lets Spring Boot publish order and payment events to `btb-events`. Configure the rule and target queue in 5.8.6.

<details>
<summary>View the event publishing permission captures</summary>

![PutEvents to the BTB custom bus.](/images/5-Workshop/5.4-Foundation/5.4.2-aws-iam/aws-iam-step-6-create-policy-4.webp)

![View the event publishing permission captures](/images/5-Workshop/5.4-Foundation/5.4.2-aws-iam/aws-iam-step-6-review-policy-4.webp)

</details>

**Verify.** In ECS → running task → Task definition, check Task role and Task execution role. The ECS capture names the execution role ecsTaskExecutionRole. Select the role configured in your lab account.
