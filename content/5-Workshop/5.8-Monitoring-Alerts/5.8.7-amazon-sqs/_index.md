---
title: "Inspect SQS messages and distinguish two DLQ uses"
menuTitle: "Amazon SQS and dead-letter queues"
weight: 7
chapter: false
pre: "<b> 5.8.7. </b> "
---

1. **Open SQS → Create queue → Standard.** Create `btb-customer-events` and `btb-customer-events-dlq` in the bus Region. Record the main queue URL and both ARNs. For a custom KMS key check sender and receiver permissions.

2. **Return to EventBridge → rule → Target.** Select the primary queue as target and the error queue as target DLQ. This DLQ receives events EventBridge fails to deliver under the retry policy.

{{< btbfigure file="aws-amazon-eventbridge-step-2-create-rule-4.webp" caption="Retry limits and queue for undelivered events." step="yes" >}}

3. **Send the event in 5.8.6 → open the primary queue → Send and receive messages → Poll for messages.** In an isolated lab pause the consumer if you need to inspect the message first.

4. **Open the message body and verify the eventId.** You can run the following PowerShell instead of polling in the console. Delete the demo message using its latest receipt handle only after processing.


```powershell
$region = "ap-southeast-1"
$queueUrl = aws sqs get-queue-url --queue-name btb-customer-events --region $region --query QueueUrl --output text
aws sqs receive-message --region $region --queue-url $queueUrl --wait-time-seconds 10 --max-number-of-messages 1 --visibility-timeout 30 --query "Messages[].{Body:Body,ReceiptHandle:ReceiptHandle}"
```


**Verify.** Expect lab-001 in the body. A received but undeleted message can reappear after the visibility timeout. For repeated consumer failures use the source queue's redrive policy and maxReceiveCount with a separate **consumer DLQ**.
