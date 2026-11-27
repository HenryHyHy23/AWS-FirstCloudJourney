---
title: "IAM cho ECS và ứng dụng Spring Boot"
menuTitle: "AWS IAM"
weight: 2
chapter: false
pre: "<b> 5.4.2. </b> "
---

ECS dùng execution role khi khởi động container. Spring Boot dùng task role khi gọi các API AWS. Tạo và gắn hai role vào đúng ô ở bài 5.6.3.

## Tạo execution role

1. **Trong AWS Console, tìm IAM → mở IAM.**

{{< btbfigure file="aws-iam-step-1.webp" caption="Trong AWS Console, tìm IAM → mở IAM." step="yes" >}}

2. **Chọn Access management → Roles → Create role.**

{{< btbfigure file="aws-iam-step-2-create-roles.webp" caption="Chọn Access management → Roles → Create role." step="yes" >}}

3. **Chọn trusted entity.** AWS service → Elastic Container Service → Elastic Container Service Task → Next.

{{< btbfigure file="aws-iam-step-3-select-entity.webp" caption="Chọn loại trusted entity cho ECS task." step="yes" >}}

4. **Gắn AmazonECSTaskExecutionRolePolicy → Next.** Policy này cho ECS tải image ECR và ghi log qua awslogs.

{{< btbfigure file="aws-iam-step-4-add-permission.webp" caption="Policy quản lý cho execution role." step="yes" >}}

5. **Đặt tên role → Create role.** Dùng `btb-ecs-execution` cho lab. Trong trust policy, kiểm tra service principal là `ecs-tasks.amazonaws.com`. 

{{< btbfigure file="aws-iam-step-4-name-review-create.webp" caption="Đặt tên role → Create role." step="yes" >}}

6. **Mở role vừa tạo → Permissions.** Nếu inject secret như bài 5.6.1, chọn Add permissions → Create inline policy. Trong Visual editor chọn Secrets Manager, action GetSecretValue và ARN chính xác của secret. Nếu dùng customer managed KMS key, cấp thêm kms:Decrypt trên key đó.

{{< btbfigure file="aws-iam-step-5-check-permissions.webp" caption="Mở role vừa tạo → Permissions." step="yes" >}}

## Tạo task role và policy mẫu cho Nova Pro

7. **Quay lại Roles → Create role.** Chọn lại trusted entity ECS Task ở bước 3. Tạo role `btb-backend-bedrock-role`. Mở role → Permissions → Add permissions → Create inline policy → JSON.

8. **Nhập policy gọi Nova Pro.** Copy JSON dưới ảnh. Thay ACCOUNT_ID và đối chiếu inference profile trong Bedrock trước khi chọn Next.

{{< btbfigure file="aws-iam-step-6-create-policy.webp" caption="InvokeModel cho inference profile và foundation model của Nova Pro." step="yes" >}}


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


9. **Đặt Policy name là btb-nova-pro-invoke → Create policy.** Hai statement cho phép gọi profile và foundation model tương ứng qua profile đó.

{{< btbfigure file="aws-iam-step-6-review-policy.webp" caption="Đặt Policy name là btb-nova-pro-invoke → Create policy." step="yes" >}}

## Các quyền theo chức năng của backend

Trong **task role → Permissions**, chỉ thêm quyền mà tính năng thực sự dùng. Có thể chọn **Create inline policy → Visual** và nhập action/resource sau.

**Gửi thẳng đến SQS · btb-outbox-sqs-send.** Outbox của ứng dụng dùng `sqs:SendMessage`, `sqs:GetQueueUrl` và `sqs:GetQueueAttributes` trên queue `btb-customer-events`. Nếu ứng dụng đã phát cùng sự kiện qua EventBridge, kiểm tra đường gửi đang dùng để tránh thông báo lặp.

<details>
<summary>Xem ảnh đối chiếu quyền SQS và màn hình Review</summary>

{{< btbfigure file="aws-iam-step-6-create-policy-1.webp" caption="Policy PublishOutbox trong ảnh dành cho đường gửi trực tiếp đến SQS." step="yes" >}}

{{< btbfigure file="aws-iam-step-6-review-policy-1.webp" caption="Xem ảnh đối chiếu quyền SQS và màn hình Review" step="yes" >}}

{{< btbfigure file="aws-iam-step-6-review-policy-2.webp" caption="Xem ảnh đối chiếu quyền SQS và màn hình Review" step="yes" >}}

</details>

**Áp dụng guardrail · btb-bedrock-guardrail.** `bedrock:ApplyGuardrail` cho phép ứng dụng áp dụng bộ lọc đã cấu hình. Chọn ARN của guardrail và guardrail profile mà lab sử dụng.

<details>
<summary>Xem ảnh đối chiếu guardrail và tên policy</summary>

{{< btbfigure file="aws-iam-step-6-create-policy-2.webp" caption="Quyền ApplyGuardrail cho guardrail và profile." step="yes" >}}

{{< btbfigure file="aws-iam-step-6-create-policy-3.webp" caption="Xem ảnh đối chiếu guardrail và tên policy" step="yes" >}}

{{< btbfigure file="aws-iam-step-6-review-policy-3.webp" caption="Xem ảnh đối chiếu guardrail và tên policy" step="yes" >}}

</details>

**Phát sự kiện · btb-eventbridge-put-events.** `events:PutEvents` cho phép Spring Boot gửi sự kiện đơn hàng hoặc thanh toán vào bus `btb-events`. Rule và queue đích được cấu hình ở 5.8.6.

<details>
<summary>Xem ảnh đối chiếu quyền phát sự kiện</summary>

{{< btbfigure file="aws-iam-step-6-create-policy-4.webp" caption="PutEvents vào custom bus của BTB." step="yes" >}}

{{< btbfigure file="aws-iam-step-6-review-policy-4.webp" caption="Xem ảnh đối chiếu quyền phát sự kiện" step="yes" >}}

</details>

**Kiểm tra kết quả.** Trong ECS → task đang chạy → Task definition, kiểm tra Task role và Task execution role. Ảnh ECS dùng execution role tên ecsTaskExecutionRole; chọn role đã cấp quyền trong tài khoản lab.
