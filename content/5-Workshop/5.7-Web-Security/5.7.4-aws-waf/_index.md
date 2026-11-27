---
title: "Protect both entry points with AWS WAF"
menuTitle: "AWS WAF (Frontend & Backend)"
weight: 4
chapter: false
pre: "<b> 5.7.4. </b> "
---

Prepare the CloudFront distribution, lab ALB and API hostname. CloudFront and the ALB use two web ACLs with different scopes. Check the ACL name in the breadcrumb before saving each rule.

## Create a web ACL and enter the example rule

1. **Find WAF & Shield in the AWS Console.**

{{< btbfigure file="aws-waf-step-1.webp" caption="Find WAF & Shield in the AWS Console." step="yes" >}}

2. **Open Protection packs (web ACLs) → Create protection pack.** Choose Global for CloudFront or Regional/Singapore for the ALB. Select the resource in that scope. If lab 5.7.2 already created a CloudFront ACL, open that ACL.

{{< btbfigure file="aws-waf-step-2-create-protection-pack.webp" caption="Protection pack creation." step="yes" >}}

3. **Open Choose initial protections → Build your own pack → Add rules.** Choose Custom rule → JSON editor.

{{< btbfigure file="aws-waf-step-3-add-rules.webp" caption="Open Choose initial protections → Build your own pack → Add rules." step="yes" >}}

4. **Enter the IP rate rule.** The pictured example uses 500 requests per 300 seconds and Block with response 429. It counts all URIs in the ACL scope. Select an unused priority in an existing ACL. For observation-only labs select Count in the editor.

{{< btbfigure file="aws-waf-step-3-add-rules-1.webp" caption="Rate-limit JSON in the console." step="yes" >}}


```json
{
  "Name": "btb-api-rate-limit-per-ip",
  "Priority": 0,
  "Statement": {
    "RateBasedStatement": {
      "Limit": 500,
      "AggregateKeyType": "IP",
      "EvaluationWindowSec": 300
    }
  },
  "VisibilityConfig": {
    "SampledRequestsEnabled": true,
    "CloudWatchMetricsEnabled": true,
    "MetricName": "btb-api-rate-limit-per-ip"
  },
  "Action": {
    "Block": {
      "CustomResponse": {
        "ResponseCode": 429
      }
    }
  }
}
```


## Review the rule fields in the visual editor

5. **Choose Rules → Add rules → Rate-based rule.** The following captures show the edge setup in the console. If you already added the example, use Edit on the corresponding rule to avoid a duplicate name.

{{< btbfigure file="aws-waf-step-3-add-rules-2.webp" caption="Choose Rules → Add rules → Rate-based rule." step="yes" >}}

6. **In Rule condition set the window and aggregation.** The visual editor shows 2,000 requests, 5 minutes and Source IP. This capture represents a different configuration from the 500-limit JSON.

{{< btbfigure file="aws-waf-step-3-add-rules-3.webp" caption="Five-minute window, limit 2,000 and source IP." step="yes" >}}

7. **Choose Rule action → Block → Customize response.** Set response code 429 so clients can recognize rate limiting.

{{< btbfigure file="aws-waf-step-3-add-rules-4.webp" caption="Choose Rule action → Block → Customize response." step="yes" >}}

8. **Name the rule btb-api-rate-limit-per-ip → Add rule.** Enable CloudWatch metrics and sampled requests to inspect traffic.

{{< btbfigure file="aws-waf-step-3-add-rules-5.webp" caption="Name the rule btb-api-rate-limit-per-ip → Add rule." step="yes" >}}

9. **Open the API rule → Edit → Scope of inspection.** For API-only limiting choose Only consider requests that match → URI path → Starts with string → /api/. Save and verify the resulting scope.

{{< btbfigure file="aws-waf-step-4-edit-rules.webp" caption="Limit scope to URI paths starting with /api/." step="yes" >}}

## Separate rules for AI, login and OTP

10. **Add the AI rate rule.** Under Rule condition select 30 requests, a five-minute window and Source IP as pictured.

{{< btbfigure file="aws-waf-step-3-add-rules-6.webp" caption="Limit 30 in the AI-rule setup sequence." step="yes" >}}

11. **Choose Only consider requests that match.** Under Inspect select URI path and enter the lab backend's AI route, for example /api/ai/agent. The capture shows the condition selector; verify the deployed URI before saving.

{{< btbfigure file="aws-waf-step-3-add-rules-9.webp" caption="Choose Only consider requests that match." step="yes" >}}

12. **Select the AI action and custom response.** For Block enter 429 as the response code.

{{< btbfigure file="aws-waf-step-3-add-rules-7.webp" caption="Select the AI action and custom response." step="yes" >}}

13. **Name the rule btb-ai-rate-limit-per-ip.** Check the metric name and priority, then save.

{{< btbfigure file="aws-waf-step-3-add-rules-8.webp" caption="Name the rule btb-ai-rate-limit-per-ip." step="yes" >}}

14. **Create the login rule → URI path → Exactly matches string.** Enter `/api/auth/login`. Choose the limit based on legitimate login traffic in your lab.

{{< btbfigure file="aws-waf-step-3-add-rules-10.webp" caption="Create the login rule → URI path → Exactly matches string." step="yes" >}}

15. **Name the rule btb-login-rate-limit-per-ip → Add rule.**

{{< btbfigure file="aws-waf-step-3-add-rules-11.webp" caption="Name the rule btb-login-rate-limit-per-ip → Add rule." step="yes" >}}

16. **Create the verify OTP rule → Rate-based rule.** The capture uses 10 requests per five minutes per source IP.

{{< btbfigure file="aws-waf-step-3-add-rules-12.webp" caption="Create the verify OTP rule → Rate-based rule." step="yes" >}}

17. **Scope the verify OTP rule.** Choose URI path → Exactly matches string → `/api/auth/verify-otp`.

{{< btbfigure file="aws-waf-step-3-add-rules-13.webp" caption="Scope the verify OTP rule." step="yes" >}}

18. **Name the rule btb-verify-otp-rate-limit-per-ip → Add rule.**

{{< btbfigure file="aws-waf-step-3-add-rules-14.webp" caption="Name the rule btb-verify-otp-rate-limit-per-ip → Add rule." step="yes" >}}

19. **Create the resend OTP rule.** Select the appropriate threshold. The capture shows 10 requests in five minutes.

{{< btbfigure file="aws-waf-step-3-add-rules-15.webp" caption="Create the resend OTP rule." step="yes" >}}

20. **Select Source IP → Only consider requests that match.** Open the scope-down condition to restrict the rule to the actual resend route.

{{< btbfigure file="aws-waf-step-3-add-rules-16.webp" caption="Select Source IP → Only consider requests that match." step="yes" >}}

21. **Inspect the prefix condition.** The capture uses URI path starting with `/api/admin/registration-requests/`. This prefix can cover multiple admin operations. Check the backend's resend route and method before finalizing the scope.

{{< btbfigure file="aws-waf-step-3-add-rules-17.webp" caption="Inspect the prefix condition." step="yes" >}}

22. **Name the rule btb-resend-otp-rate-limit-per-ip.** Review metric, action and priority and save.

{{< btbfigure file="aws-waf-step-3-add-rules-18.webp" caption="Name the rule btb-resend-otp-rate-limit-per-ip." step="yes" >}}

23. **Open Rules and inspect the five rate rules.** All five show Block in this capture. Open a rule to check its limit and URI.

{{< btbfigure file="aws-waf-step-3-display-rules.webp" caption="Five rate rules shown with Block in one captured configuration." step="yes" >}}

24. **Review the complete CloudFront ACL rule list.** Lower priority numbers run first. The two captured managed groups check IP reputation and known risky inputs. Override action None preserves the component rule actions.

{{< btbfigure file="aws-waf-step-3-display-rules-1.webp" caption="Review the complete CloudFront ACL rule list." step="yes" >}}

<details>
<summary>View the two AWS managed rule group captures</summary>

{{< btbfigure file="aws-waf-step-4-edit-rules-1.webp" caption="AWS managed IP reputation list in JSON view." step="yes" >}}

{{< btbfigure file="aws-waf-step-4-edit-rules-2.webp" caption="AWS managed Known Bad Inputs in the JSON editor." step="yes" >}}

</details>

## Protect the ALB and verify the SePay webhook

25. **Switch to Regional/Singapore → btb-backend-waf → Add rate-based rule.** The capture shows a threshold of 100 for the webhook. Set the window and action to match legitimate callback traffic.

{{< btbfigure file="aws-waf-step-3-add-rules-19.webp" caption="Switch to Regional/Singapore → btb-backend-waf → Add rate-based rule." step="yes" >}}

26. **Select URI path → Exactly matches string.** Enter `/api/payments/sepay/webhook` for the webhook rule.

{{< btbfigure file="aws-waf-step-3-add-rules-20.webp" caption="Select URI path → Exactly matches string." step="yes" >}}

27. **Name the rule btb-sepay-webhook-rate-limit → Add rule.**

{{< btbfigure file="aws-waf-step-3-add-rules-21.webp" caption="Name the rule btb-sepay-webhook-rate-limit → Add rule." step="yes" >}}

28. **Choose Add rule → Custom rule.** Create the rule that checks requests to the ALB origin. Use the same X-Origin-Verify value configured on CloudFront.

{{< btbfigure file="aws-waf-step-3-add-rules-22.webp" caption="Choose Add rule → Custom rule." step="yes" >}}

29. **Select Match all of the following.** Block when **both** conditions hold: the origin header does not match **and** the URI is not the designated SePay callback. Use this exception only for a lab accepting direct SePay callbacks. The backend still authenticates the callback. The private value column has been cropped.

{{< btbfigure file="aws-waf-step-3-add-rules-23.webp" caption="Select Match all of the following." step="yes" >}}

30. **Name the rule btb-allow-only-cloudfront → Save.** Check its evaluation order with the webhook rate rule before testing.

{{< btbfigure file="aws-waf-step-3-add-rules-24.webp" caption="Name the rule btb-allow-only-cloudfront → Save." step="yes" >}}

31. **Inspect older rules still on the regional ACL.** The Delete 5 selected rules dialog records a cleanup operation. Skip deletion on a new ACL. For an older lab ACL verify the association and names before retiring unused rules.

{{< btbfigure file="aws-waf-step-5-delete-rules.webp" caption="Inspect older rules still on the regional ACL." step="yes" >}}

32. **Open btb-backend-waf → Rules and Associated resources.** Verify the ALB, rule names, actions and priorities. The captured list shows Block for both regional rules; the team note recorded Count. Record the current settings and inspection time.

{{< btbfigure file="aws-waf-step-3-display-rules-2.webp" caption="The two actions and priorities in the backend ACL capture." step="yes" >}}

**Verify.** Make one valid request from the storefront and inspect its response. For the webhook use the demo transaction in 5.9.4. Match metrics, logs and sampled requests for each ACL by timestamp.
