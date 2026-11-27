---
title: "Deliver operations alerts through SNS"
menuTitle: "Amazon SNS"
weight: 3
chapter: false
pre: "<b> 5.8.3. </b> "
---

1. **Find Simple Notification Service → open Topics.**

{{< btbfigure file="aws-sns-step-1.webp" caption="Find Simple Notification Service → open Topics." step="yes" >}}

2. **Choose Create topic → Standard.** Name it `btb-system-alerts` in the alarm Region and create the topic.

{{< btbfigure file="aws-sns-step-2-create-topic.webp" caption="Select a Standard topic for alert emails." step="yes" >}}

3. **Open the topic → Create subscription.** Set Protocol to Email and Endpoint to your lab mailbox. Create the subscription. Its initial status can be Pending confirmation.

{{< btbfigure file="aws-sns-step-3-create-subscription.webp" caption="Open the topic → Create subscription." step="yes" >}}

4. **Open the AWS Notification email → Confirm subscription.** Return to Subscriptions and check for a confirmed subscription ID.

{{< btbfigure file="aws-sns-step-4-confirm.webp" caption="Invitation to confirm the subscription." step="yes" >}}

5. **Open the topic → Publish message.** Enter subject BTB lab and a test message. Check the topic ARN, recipient and email arrival time.

**Verify.** The lab mailbox receives the test message. If it is missing check Pending confirmation, spam and Region.
