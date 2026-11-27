---
title: "Issue certificates for the website and API"
menuTitle: "AWS Certificate Manager (ACM)"
weight: 1
chapter: false
pre: "<b> 5.7.1. </b> "
---

1. **Select N. Virginia (us-east-1) → ACM → Request a certificate → Request a public certificate.**

![Select N. Virginia (us-east-1) → ACM → Request a certificate → Request a public certificate.](/images/5-Workshop/5.7-Web-Security/5.7.1-aws-certificate-manager/aws-certificate-manager-step-1-request-a-public-certificate.webp)

2. **Enter the storefront names → DNS validation → Request.** Include apex and www if serving both. Under Domains create the validation CNAME in the authoritative zone.

![Domain names and DNS validation for the frontend certificate.](/images/5-Workshop/5.7-Web-Security/5.7.1-aws-certificate-manager/aws-certificate-manager-step-2-setup-certificate.webp)

3. **Open Certificates in N. Virginia.** Wait for Issued and verify coverage of the CloudFront alternate domain names.

![N. Virginia certificate in Issued state.](/images/5-Workshop/5.7-Web-Security/5.7.1-aws-certificate-manager/aws-certificate-manager-step-3-issued-virginia.webp)

4. **Switch to Singapore → Request a public certificate for the API hostname.** Repeat DNS validation using the name served by the ALB listener.

![Certificate request in Singapore.](/images/5-Workshop/5.7-Web-Security/5.7.1-aws-certificate-manager/aws-certificate-manager-step-2-setup-certificate-singapore.webp)

5. **Open Certificates in Singapore and verify Issued.** Select this certificate for HTTPS:443 on the ALB. Retain the validation CNAME for renewal.

![Singapore certificate in Issued state.](/images/5-Workshop/5.7-Web-Security/5.7.1-aws-certificate-manager/aws-certificate-manager-step-3-issued-singapore.webp)

**Verify.** For Pending validation check domain delegation and the DNS CNAME. If an issued certificate is missing from a selector, check Region and account.
