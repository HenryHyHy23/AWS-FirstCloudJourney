---
title: "Track cost with AWS Budgets"
menuTitle: "AWS Budgets"
weight: 1
chapter: false
pre: "<b> 5.4.1. </b> "
---

Set a monthly budget before starting Fargate or the ALB. Have a lab mailbox ready for alerts.

1. **Find Billing and Cost Management in the AWS Console.**

![Find Billing and Cost Management in the AWS Console.](/images/5-Workshop/5.4-Foundation/5.4.1-aws-budgets/aws-budgets-step-1.webp)

2. **Open Budgets → Create budget.**

![Open Budgets → Create budget.](/images/5-Workshop/5.4-Foundation/5.4.1-aws-budgets/aws-budgets-step-2-create-budget.webp)

3. **Choose Customize (advanced) → Cost budget → Next.**

![Select Cost budget to track spend in currency.](/images/5-Workshop/5.4-Foundation/5.4.1-aws-budgets/aws-budgets-step-3-customization-setup.webp)

4. **Name the budget and set its period.** In Set your budget enter the following.
   - Budget name — `BTB-Monthly`
   - Period — Monthly
   - Renewal — Recurring
   - Budgeting method — Fixed

![Name and monthly renewal settings.](/images/5-Workshop/5.4-Foundation/5.4.1-aws-budgets/aws-budgets-step-4-set-your-budget.webp)

5. **Set the amount and scope.** The capture uses USD 200 and All AWS services. Use an amount appropriate for your lab, then select Next.

![Fixed amount and service scope.](/images/5-Workshop/5.4-Foundation/5.4.1-aws-budgets/aws-budgets-step-4-set-your-budget-1.webp)

6. **Add an alert threshold.** Choose Actual cost, Greater than and 75% of budgeted amount. For the pictured budget this is USD 150.

![Threshold based on actual spend.](/images/5-Workshop/5.4-Foundation/5.4.1-aws-budgets/aws-budgets-step-5-set-alerts.webp)

7. **Enter the email recipient.** Use a mailbox you can verify, then choose Next.

![Enter the email recipient.](/images/5-Workshop/5.4-Foundation/5.4.1-aws-budgets/aws-budgets-step-5-set-alerts-1.webp)

8. **Check Attach actions.** Leave actions empty for an email-only alert and choose Next.

![Check Attach actions.](/images/5-Workshop/5.4-Foundation/5.4.1-aws-budgets/aws-budgets-step-6-review-alert.webp)

9. **Review Budget details.** Check Cost budget, Monthly and the amount entered.

![Review Budget details.](/images/5-Workshop/5.4-Foundation/5.4.1-aws-budgets/aws-budgets-step-7-review.webp)

10. **Review Alert #1 → Create budget.** Open the verification email if AWS requests it.

![Review shows the 75% alert and no automatic budget actions.](/images/5-Workshop/5.4-Foundation/5.4.1-aws-budgets/aws-budgets-step-7-review-1.webp)

**Verify.** Reopen the budget and check its Actual 75% threshold and recipient status. Billing data can take time to arrive.
