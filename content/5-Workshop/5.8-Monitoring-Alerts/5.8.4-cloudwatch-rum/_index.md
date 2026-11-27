---
title: "Collect React telemetry with CloudWatch RUM"
menuTitle: "CloudWatch RUM"
weight: 4
chapter: false
pre: "<b> 5.8.4. </b> "
---

1. **Open CloudWatch → RUM → Add app monitor.** Enter the following settings.
   - Platform — Web
   - App monitor name — `btb-frontend-performance`
   - Domains — the actual frontend hostnames, such as the apex and www
   - Authorization — select a Cognito Identity Pool and complete the guest role in 5.8.5

{{< btbfigure file="aws-cloudwatch-rum-step-1-add-app-monitor.webp" caption="App monitor name, web platform and domain list." step="yes" >}}

2. **Open Code snippet → TypeScript.** In the frontend repo run `npm install aws-rum-web`. Place the code below in a startup module that runs once before React renders. Replace APP_MONITOR_UUID and IDENTITY_POOL_UUID with the lab IDs.

{{< btbfigure file="aws-cloudwatch-rum-step-2.webp" caption="Monitor-generated TypeScript installation instructions." step="yes" >}}


```typescript
import { AwsRum, type AwsRumConfig } from 'aws-rum-web';

try {
  const config: AwsRumConfig = {
    sessionSampleRate: 1,
    identityPoolId: 'ap-southeast-1:IDENTITY_POOL_UUID',
    endpoint: 'https://dataplane.rum.ap-southeast-1.amazonaws.com',
    telemetries: ['performance', 'errors', 'http'],
    allowCookies: true,
    enableXRay: false,
    signing: true,
  };
  new AwsRum('APP_MONITOR_UUID', '1.0.0', 'ap-southeast-1', config);
} catch {
  // Telemetry initialization must not prevent React from starting.
}
```


3. **Rebuild the frontend → upload to S3 → refresh the CloudFront cache.** Open a new tab on a configured hostname. The capture uses 100% sampling and cookies. Apply the website's consent requirements in use.

**Verify.** In DevTools → Network filter for dataplane.rum and inspect its status. In RUM → monitor select the test time range and look for a page view. For 403 check guest role and signing. The note reports client integration; production ingestion still needs testing.
