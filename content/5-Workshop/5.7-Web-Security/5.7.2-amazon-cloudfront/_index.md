---
title: "CloudFront with two origins for the BTB storefront"
menuTitle: "Amazon CloudFront"
weight: 2
chapter: false
pre: "<b> 5.7.2. </b> "
---

## Serve the frontend from S3

1. **Open CloudFront → Distributions → Create distribution.** Select Single website or app and enter the lab name and frontend hostnames.

![Open CloudFront → Distributions → Create distribution.](/images/5-Workshop/5.7-Web-Security/5.7.2-amazon-cloudfront/aws-cloudfront-step-1-create-distribution.webp)

2. **Under Specify origin select Amazon S3.** Choose the frontend bucket's REST endpoint and signed Origin Access Control. Apply the CloudFront-provided bucket policy for GetObject on bucket/* restricted to the distribution ARN. Set index.html as default root object.

![Choose the frontend build origin.](/images/5-Workshop/5.7-Web-Security/5.7.2-amazon-cloudfront/aws-cloudfront-step-2-origin.webp)

3. **Open Enable security.** Review the distribution's WAF option. If enabling it here, record the web ACL CloudFront creates for lab 5.7.4.

![Open Enable security.](/images/5-Workshop/5.7-Web-Security/5.7.2-amazon-cloudfront/aws-cloudfront-step-3-enable-security.webp)

4. **Choose Get TLS certificate → select the us-east-1 certificate.** Check apex and www coverage if both are used.

![Attach the TLS certificate to the distribution.](/images/5-Workshop/5.7-Web-Security/5.7.2-amazon-cloudfront/aws-cloudfront-step-4-setup-certificate.webp)

5. **Choose Review and create → Create distribution.** Wait for Deployed and test the home page at the distribution domain before adding the rewrite.

![Choose Review and create → Create distribution.](/images/5-Workshop/5.7-Web-Security/5.7.2-amazon-cloudfront/aws-cloudfront-step-5-review.webp)

## Support refreshing React routes

6. **Open CloudFront → Functions → Create function.** Name it `btb-spa-rewrite`, select JavaScript runtime 2.0 and create it.

![Open CloudFront → Functions → Create function.](/images/5-Workshop/5.7-Web-Security/5.7.2-amazon-cloudfront/aws-cloudfront-step-6-create-function.webp)

7. **Open Build → Development, paste the function and save changes.** The function rewrites to /index.html when the last path segment has no period.

![CloudFront Function editor.](/images/5-Workshop/5.7-Web-Security/5.7.2-amazon-cloudfront/aws-cloudfront-step-7-build-code.webp)


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

![Choose a behavior and event for the published function.](/images/5-Workshop/5.7-Web-Security/5.7.2-amazon-cloudfront/aws-cloudfront-step-8-associate-function.webp)

9. **Open Error pages and inspect any old 403 mapping.** If all 403s return /index.html and 200, test the function first, then remove that legacy mapping so API errors keep their status.

![Open Error pages and inspect any old 403 mapping.](/images/5-Workshop/5.7-Web-Security/5.7.2-amazon-cloudfront/aws-cloudfront-step-9-delete-403-error-page.webp)

10. **Check the 404 mapping as well.** If there are no custom error responses, continue to the API origin.

![Check the 404 mapping as well.](/images/5-Workshop/5.7-Web-Security/5.7.2-amazon-cloudfront/aws-cloudfront-step-9-delete-404-error-page.webp)

## Add the API origin and behavior

11. **Prepare a private X-Origin-Verify value.** The capture illustrates generating a string in PowerShell. For a running lab use a fresh value from a secure generator and store it only at the origin configuration and verifier.

![PowerShell command in the capture, with the generated output removed.](/images/5-Workshop/5.7-Web-Security/5.7.2-amazon-cloudfront/aws-cloudfront-step-10-idk.webp)

12. **Choose Distributions → Origins → Create origin.** Enter the API hostname pointing to the ALB, choose HTTPS only and port 443. The ALB certificate must cover that hostname.

![HTTPS protocol and API origin hostname.](/images/5-Workshop/5.7-Web-Security/5.7.2-amazon-cloudfront/aws-cloudfront-step-11-create-origin.webp)

13. **Open Add custom header.** Use X-Origin-Verify as the name and enter the value configured on regional WAF. Name the origin btb-backend-alb.

![Origin custom header name, with the value column removed.](/images/5-Workshop/5.7-Web-Security/5.7.2-amazon-cloudfront/aws-cloudfront-step-11-create-origin-1.webp)

14. **Review Additional settings → Create origin.** Compare connection attempts, connection timeout and response timeout to expected response times.

![Review Additional settings → Create origin.](/images/5-Workshop/5.7-Web-Security/5.7.2-amazon-cloudfront/aws-cloudfront-step-11-create-origin-2.webp)

15. **Open Behaviors → Create behavior.** Choose /api/*, origin btb-backend-alb, Redirect HTTP to HTTPS and the HTTP methods needed by the API.

![API behavior sends requests to the ALB.](/images/5-Workshop/5.7-Web-Security/5.7.2-amazon-cloudfront/aws-cloudfront-step-12-edit-behavior.webp)

16. **Set cache policy and origin request policy → Save.** Select CachingDisabled and AllViewerExceptHostHeader. Leave the API behavior's function association empty.

![Cache and origin request policies for the API.](/images/5-Workshop/5.7-Web-Security/5.7.2-amazon-cloudfront/aws-cloudfront-step-12-create-behavior.webp)

**Verify.** Wait for Deployed. Refresh a deep link, open a JS asset and fetch /api/health on the frontend hostname. Expect a backend response. If you get HTML inspect /api/*, function association and custom error mappings.
