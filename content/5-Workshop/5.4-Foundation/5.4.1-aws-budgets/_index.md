---
title: "Track cost with AWS Budgets"
menuTitle: "AWS Budgets"
weight: 1
chapter: false
pre: "<b> 5.4.1. </b> "
---

Set a monthly budget before starting Fargate or the ALB. Have a lab mailbox ready for alerts.

1. **Find Billing and Cost Management in the AWS Console.**

{{< btbfigure file="aws-budgets-step-1.webp" caption="Find Billing and Cost Management in the AWS Console." step="yes" >}}

2. **Open Budgets → Create budget.**

{{< btbfigure file="aws-budgets-step-2-create-budget.webp" caption="Open Budgets → Create budget." step="yes" >}}

3. **Choose Customize (advanced) → Cost budget → Next.**

{{< btbfigure file="aws-budgets-step-3-customization-setup.webp" caption="Select Cost budget to track spend in currency." step="yes" >}}

4. **Name the budget and set its period.** In Set your budget enter the following.
   - Budget name — `BTB-Monthly`
   - Period — Monthly
   - Renewal — Recurring
   - Budgeting method — Fixed

{{< btbfigure file="aws-budgets-step-4-set-your-budget.webp" caption="Name and monthly renewal settings." step="yes" >}}

5. **Set the amount and scope.** The capture uses USD 200 and All AWS services. Use an amount appropriate for your lab, then select Next.

{{< btbfigure file="aws-budgets-step-4-set-your-budget-1.webp" caption="Fixed amount and service scope." step="yes" >}}

6. **Add an alert threshold.** Choose Actual cost, Greater than and 75% of budgeted amount. For the pictured budget this is USD 150.

{{< btbfigure file="aws-budgets-step-5-set-alerts.webp" caption="Threshold based on actual spend." step="yes" >}}

7. **Enter the email recipient.** Use a mailbox you can verify, then choose Next.

{{< btbfigure file="aws-budgets-step-5-set-alerts-1.webp" caption="Enter the email recipient." step="yes" >}}

8. **Check Attach actions.** Leave actions empty for an email-only alert and choose Next.

{{< btbfigure file="aws-budgets-step-6-review-alert.webp" caption="Check Attach actions." step="yes" >}}

9. **Review Budget details.** Check Cost budget, Monthly and the amount entered.

{{< btbfigure file="aws-budgets-step-7-review.webp" caption="Review Budget details." step="yes" >}}

10. **Review Alert #1 → Create budget.** Open the verification email if AWS requests it.

{{< btbfigure file="aws-budgets-step-7-review-1.webp" caption="Review shows the 75% alert and no automatic budget actions." step="yes" >}}

**Verify.** Reopen the budget and check its Actual 75% threshold and recipient status. Billing data can take time to arrive.
