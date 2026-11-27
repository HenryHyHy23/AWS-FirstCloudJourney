---
title: "Route order and payment events"
menuTitle: "Amazon EventBridge"
weight: 6
chapter: false
pre: "<b> 5.8.6. </b> "
---

First create the two Standard queues btb-customer-events and btb-customer-events-dlq in 5.8.7. Use the same Region for the bus, rule and queues.

1. **Open EventBridge → Event buses → Create custom event bus.** Choose the classic bus shown, enter `btb-events` and choose the AWS owned key.

![Bus name and encryption settings.](/images/5-Workshop/5.8-Monitoring-Alerts/5.8.6-amazon-eventbridge/aws-amazon-eventbridge-step-1-create-custom-event-bus.webp)

2. **Review Logs, Resource-based policy and Archives.** Choose a log destination if needed. For a same-account producer grant the task role PutEvents on this bus as in 5.4.2.

![Review Logs, Resource-based policy and Archives.](/images/5-Workshop/5.8-Monitoring-Alerts/5.8.6-amazon-eventbridge/aws-amazon-eventbridge-step-1-create-custom-event-bus-1.webp)

3. **Review the optional archive and choose Create.** If enabling replay name it btb-events-archive and set retention. Keep event IDs so consumers can handle replay.

![Optional archive configuration.](/images/5-Workshop/5.8-Monitoring-Alerts/5.8.6-amazon-eventbridge/aws-amazon-eventbridge-step-1-create-custom-event-bus-2.webp)

4. **Choose Rules → Create rule.** Name it `btb-customer-notifications`, select bus `btb-events` and choose a rule with an event pattern.

![Rule name and selected bus.](/images/5-Workshop/5.8-Monitoring-Alerts/5.8.6-amazon-eventbridge/aws-amazon-eventbridge-step-2-create-rule.webp)

5. **Under Event pattern select Custom pattern → JSON editor.** Paste the sample below. Source must match exactly and detail-type must start with order. or payment. to match.

![Source and the two detail-type prefixes.](/images/5-Workshop/5.8-Monitoring-Alerts/5.8.6-amazon-eventbridge/aws-amazon-eventbridge-step-2-create-rule-2.webp)


```json
{
  "source": [
    "bitetobusiness.backend"
  ],
  "detail-type": [
    {
      "prefix": "order."
    },
    {
      "prefix": "payment."
    }
  ]
}
```


6. **Select Target → SQS queue → btb-customer-events.** For a console-created execution role verify EventBridge trust and SendMessage on the queue ARN.

![Select the SQS target in the lab account.](/images/5-Workshop/5.8-Monitoring-Alerts/5.8.6-amazon-eventbridge/aws-amazon-eventbridge-step-2-create-rule-3.webp)

7. **Open Retry policy and dead-letter queue.** Configure as pictured.
   - Maximum event age — 1 hour
   - Retry attempts — 20
   - Dead-letter queue — btb-customer-events-dlq; verify its queue policy allows the rule ARN to deliver

![Retry limits and queue for undelivered events.](/images/5-Workshop/5.8-Monitoring-Alerts/5.8.6-amazon-eventbridge/aws-amazon-eventbridge-step-2-create-rule-4.webp)

8. **Open Input transformation → Part of the matched event.** Enter `$.detail`, review and create the rule. Verify Enabled. The SQS body contains only detail.

![JSONPath selecting the detail sent to the queue.](/images/5-Workshop/5.8-Monitoring-Alerts/5.8.6-amazon-eventbridge/aws-amazon-eventbridge-step-2-create-rule-5.webp)

9. **Send a test event from the lab bus.** Open the bus → Send events. Enter source `bitetobusiness.backend`, detail type `order.workshop` and detail `{"eventId":"lab-001","orderRef":"DEMO-01"}`. Send and inspect the queue body in 5.8.7. Try inventory.updated in the Event pattern sandbox to check a non-match.

**Verify.** SQS receives eventId lab-001 and orderRef DEMO-01. For an empty queue check bus, pattern, target role and FailedInvocations.