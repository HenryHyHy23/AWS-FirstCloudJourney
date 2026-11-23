---
title: "Blog 3"
date: 2026-11-19
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# SENDING ORDER CONFIRMATION EMAILS WITHOUT SLOWING DOWN THE WEBSITE: EXPLORING AMAZON SQS, AWS LAMBDA, AND AMAZON SES

**If the email service is a few seconds slow, does the customer have to watch the "Place order" button spin?**

While learning how an online store works, I noticed that the most obvious approach is: the API saves the order, sends a confirmation email, and only then responds to the customer. This works when I test it on my machine. But if the email service is slow, the customer waits too. If the email service fails, the API may report "order failed" even though the order was saved, or the email may simply be lost.

That led me to explore how to separate sending emails from the ordering flow using **Amazon SQS**, **AWS Lambda**, and **Amazon SES**. This post is what I have understood so far, so feel free to correct me if something is wrong.

**The idea: put a queue in the middle**

I picture it like the order counter in a restaurant. The cashier writes a ticket, puts it on the counter, and serves the next customer; the kitchen picks up the ticket and cooks later. The cashier does not wait for the food before taking the next order.

In this example, the backend is the cashier, the queue is the ticket counter, and the email sender is the kitchen.

- **Amazon SQS:** a queue service. The backend puts a "new order" message in the queue and responds to the customer immediately.
- **AWS Lambda:** runs code without managing servers. Lambda picks up messages from the queue in batches and processes them.
- **Amazon SES:** an email service. Lambda calls SES to send the confirmation email.
- **Dead-letter queue (DLQ):** a separate queue for messages that failed too many times, so I can review them later instead of letting them retry forever.

**How it works (see the diagram)**

![Sending order confirmation emails with Amazon SQS, AWS Lambda, and Amazon SES](/images/3-BlogsTranslated/3.3-Blog3/architecture.png)

- The customer clicks "Place order". The backend saves the order in a database (for example, Amazon RDS) and responds right away.
- The backend sends a "new order" message to the SQS queue.
- Lambda receives the message. Before sending, it checks Amazon DynamoDB to see whether the email for this order has already been sent.
- If not, Lambda calls SES to send the email to the customer's inbox and records that it was sent.
- If a message fails several times, SQS moves it to the DLQ. A CloudWatch alarm tells me when the DLQ is not empty.

One detail is easy to miss: if the backend saves the order but fails to send the message to SQS, the email is lost. The approach I found is the **transactional outbox**: the backend writes the order and the "event to send" in the same database transaction, and a separate process then forwards that event to SQS.

**Traps I found while reading the documentation**

- **A message can be delivered more than once.** SQS standard queues guarantee at-least-once delivery, so the same message occasionally arrives twice. Without duplicate protection, a customer could get two emails for one order. That is why I store the IDs of orders whose emails were sent in DynamoDB and check them first.
- **The visibility timeout is too short.** While Lambda processes a message, the message is hidden for the visibility timeout. If that period is shorter than the Lambda run time, the message reappears and is processed again. AWS recommends a visibility timeout of at least six times the Lambda function timeout.
- **One failed message makes the whole batch retry.** By default, Lambda reads up to 10 messages at once. If one fails and Lambda reports the whole batch as failed, messages that were already emailed also return to the queue. Turning on "ReportBatchItemFailures" lets Lambda return only the failed messages.
- **A DLQ exists, but nobody looks at it.** The documentation suggests setting "maxReceiveCount" (how many times a message can be received before moving to the DLQ) to at least 5. Without an alarm, though, messages can sit in the DLQ unnoticed.
- **SES starts in sandbox mode.** In the sandbox, SES only sends to verified addresses, up to 200 emails per 24 hours and 1 email per second. To email real customers, I need to request production access in each Region.

**What about cost?**

I estimated an example of 30,000 orders per month, based on the pricing pages I checked in November 2026:

- **SQS:** the first 1 million requests each month are free. Each message takes a few requests (send, receive, delete), about 90,000 requests in total. Lambda also polls the queue continuously, and those polls count as requests, but at low traffic they usually stay within the free amount.
- **Lambda:** 1 million requests and 400,000 GB-seconds per month are free, so 30,000 short email jobs cost almost nothing.
- **SES:** the older pay-as-you-go price is 0.10 USD per 1,000 emails, about 3 USD for 30,000 emails. According to the SES pricing page, since July 21, 2026 new accounts default to the Essentials plan at 0.16 USD per 1,000 emails, about 4.8 USD.

In my view, money is not the main benefit of this architecture. The benefits are a faster API, no lost emails when the email service has a temporary problem, and automatic retries.

**How would I test it?**

- Place a test order and check that the API responds without waiting for the email.
- Send the same message to the queue twice; the customer should receive only one email.
- Make Lambda fail on purpose and check that the message moves to the DLQ after five attempts and the alarm fires.
- Use the SES mailbox simulator to test sending without real email addresses.

I think a queue is one of the simplest yet most useful ideas when learning AWS: anything that does not need to happen right away can go into a queue, so important steps like placing an order stay fast and reliable.

**References:**

- [Amazon SQS standard queues](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/standard-queues.html)
- [Using Lambda with Amazon SQS](https://docs.aws.amazon.com/lambda/latest/dg/with-sqs.html)
- [Amazon SQS dead-letter queues](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html)
- [Request production access for Amazon SES](https://docs.aws.amazon.com/ses/latest/dg/request-production-access.html)
- [Transactional outbox pattern](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/transactional-outbox.html)
