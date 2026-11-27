---
title: "Trace a SePay payment from QR to webhook"
menuTitle: "SePay QR & Payment Webhook"
weight: 4
chapter: false
pre: "<b> 5.9.4. </b> "
---

1. **Open the test SePay dashboard → webhook configuration.** Enter the lab HTTPS callback and compare it with the backend route. The WAF capture uses /api/payments/sepay/webhook.

2. **Select the authentication method supported by the webhook controller.** Store the credential in the backend secret. Match the callback's CloudFront or direct ALB path to the rule in 5.7.4.

3. **Open BTB and create a demo order awaiting payment.** Compare the QR reference and amount with backend data. Use the account's test/sandbox facility if available or an approved fixture in a test environment.

4. **Inspect the webhook result.** Record event ID, status and time. Check the corresponding order after a valid callback. Replay the same demo event and inspect the payment record count.

**Verify.** A valid transaction is applied once. For 403 compare WAF sampled requests and Spring logs at the same time to locate the rejection.
