---
title: "CloudFront with two origins for the BTB storefront"
menuTitle: "Amazon CloudFront"
weight: 2
chapter: false
pre: "<b> 5.7.2. </b> "
---

## Serve the frontend from S3

1. **Open CloudFront → Distributions → Create distribution.** Select Single website or app and enter the lab name and frontend hostnames.

{{< btbfigure file="aws-cloudfront-step-1-create-distribution.webp" caption="Open CloudFront → Distributions → Create distribution." step="yes" >}}

2. **Under Specify origin select Amazon S3.** Choose the frontend bucket's REST endpoint and signed Origin Access Control. Apply the CloudFront-provided bucket policy for GetObject on bucket/* restricted to the distribution ARN. Set index.html as default root object.

{{< btbfigure file="aws-cloudfront-step-2-origin.webp" caption="Choose the frontend build origin." step="yes" >}}

3. **Open Enable security.** Review the distribution's WAF option. If enabling it here, record the web ACL CloudFront creates for lab 5.7.4.

{{< btbfigure file="aws-cloudfront-step-3-enable-security.webp" caption="Open Enable security." step="yes" >}}

4. **Choose Get TLS certificate → select the us-east-1 certificate.** Check apex and www coverage if both are used.

{{< btbfigure file="aws-cloudfront-step-4-setup-certificate.webp" caption="Attach the TLS certificate to the distribution." step="yes" >}}

5. **Choose Review and create → Create distribution.** Wait for Deployed and test the home page at the distribution domain before adding the rewrite.

{{< btbfigure file="aws-cloudfront-step-5-review.webp" caption="Choose Review and create → Create distribution." step="yes" >}}

## Support refreshing React routes

6. **Open CloudFront → Functions → Create function.** Name it `btb-spa-rewrite`, select JavaScript runtime 2.0 and create it.

{{< btbfigure file="aws-cloudfront-step-6-create-function.webp" caption="Open CloudFront → Functions → Create function." step="yes" >}}

7. **Open Build → Development, paste the function and save changes.** The function rewrites to /index.html when the last path segment has no period.

{{< btbfigure file="aws-cloudfront-step-7-build-code.webp" caption="CloudFront Function editor." step="yes" >}}


```javascript
function handler(event) {
    var request = event.request;
    var uri = request.uri;
    var lastSegment = uri.substring(uri.lastIndexOf('/') + 1);
    if (lastSegment.indexOf('.') === -1) {
        request.uri = '/index.html';
    }
    return request;
}
```


8. **Test → Publish → Associate.** Test /customer/profile and /assets/app.js. Associate the Live version with Default (*) at Viewer request.

{{< btbfigure file="aws-cloudfront-step-8-associate-function.webp" caption="Choose a behavior and event for the published function." step="yes" >}}

9. **Open Error pages and inspect any old 403 mapping.** If all 403s return /index.html and 200, test the function first, then remove that legacy mapping so API errors keep their status.

{{< btbfigure file="aws-cloudfront-step-9-delete-403-error-page.webp" caption="Open Error pages and inspect any old 403 mapping." step="yes" >}}

10. **Check the 404 mapping as well.** If there are no custom error responses, continue to the API origin.

{{< btbfigure file="aws-cloudfront-step-9-delete-404-error-page.webp" caption="Check the 404 mapping as well." step="yes" >}}

## Add the API origin and behavior

11. **Prepare a private X-Origin-Verify value.** The capture illustrates generating a string in PowerShell. For a running lab use a fresh value from a secure generator and store it only at the origin configuration and verifier.

{{< btbfigure file="aws-cloudfront-step-10-idk.webp" caption="PowerShell command in the capture, with the generated output removed." step="yes" >}}

12. **Choose Distributions → Origins → Create origin.** Enter the API hostname pointing to the ALB, choose HTTPS only and port 443. The ALB certificate must cover that hostname.

{{< btbfigure file="aws-cloudfront-step-11-create-origin.webp" caption="HTTPS protocol and API origin hostname." step="yes" >}}

13. **Open Add custom header.** Use X-Origin-Verify as the name and enter the value configured on regional WAF. Name the origin btb-backend-alb.

{{< btbfigure file="aws-cloudfront-step-11-create-origin-1.webp" caption="Origin custom header name, with the value column removed." step="yes" >}}

14. **Review Additional settings → Create origin.** Compare connection attempts, connection timeout and response timeout to expected response times.

{{< btbfigure file="aws-cloudfront-step-11-create-origin-2.webp" caption="Review Additional settings → Create origin." step="yes" >}}

15. **Open Behaviors → Create behavior.** Choose /api/*, origin btb-backend-alb, Redirect HTTP to HTTPS and the HTTP methods needed by the API.

{{< btbfigure file="aws-cloudfront-step-12-edit-behavior.webp" caption="API behavior sends requests to the ALB." step="yes" >}}

16. **Set cache policy and origin request policy → Save.** Select CachingDisabled and AllViewerExceptHostHeader. Leave the API behavior's function association empty.

{{< btbfigure file="aws-cloudfront-step-12-create-behavior.webp" caption="Cache and origin request policies for the API." step="yes" >}}

**Verify.** Wait for Deployed. Refresh a deep link, open a JS asset and fetch /api/health on the frontend hostname. Expect a backend response. If you get HTML inspect /api/*, function association and custom error mappings.
