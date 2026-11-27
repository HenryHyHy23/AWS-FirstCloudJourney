---
title: "Grant temporary credentials to the RUM client"
menuTitle: "Amazon Cognito Identity Pools"
weight: 5
chapter: false
pre: "<b> 5.8.5. </b> "
---

1. **Open Cognito → Identity pools → the pool used by RUM → User access.** Check Guest access is Active and open the guest role. Verify the trust policy specifies the correct pool aud, unauthenticated amr and cognito-identity.amazonaws.com principal.

{{< btbfigure file="aws-cognito-identity-pools-step-1-guest-access.webp" caption="Guest access and guest role." step="yes" >}}

2. **Open the IAM role → Permissions → putRumEvents → Edit → JSON.** The capture permits writes to all monitors in the account. The lab example below narrows access to the BTB monitor. Replace ACCOUNT_ID and the monitor name, review and save.

{{< btbfigure file="aws-cognito-identity-pools-step-2-policy-check.webp" caption="JSON allowing rum:PutRumEvents." step="yes" >}}


```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "SendFrontendTelemetry",
      "Effect": "Allow",
      "Action": "rum:PutRumEvents",
      "Resource": "arn:aws:rum:ap-southeast-1:ACCOUNT_ID:appmonitor/btb-frontend-performance"
    }
  ]
}
```


**Verify.** Place the pool ID in the client from 5.8.4, open a new browser session and verify PutRumEvents succeeds. This guest role is for browser telemetry.
