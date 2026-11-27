---
title: "Test and demonstrate the deployment"
menuTitle: "Testing & Live Demo"
weight: 11
chapter: false
pre: "<b> 5.11. </b> "
---



Use a connected lab with demo accounts and orders. For each case record time, SHA/revision, observed result and one redacted piece of evidence. The supplied configuration captures do not establish production test results.

## 1. Test from edge to container

Replace the hostname and React path with a valid route in the deployed build. A protected route may return the React shell before the app redirects. Inspect response body and Content-Type to distinguish API output from HTML.


```powershell
$site = "https://YOUR_LAB_FRONTEND"
curl.exe -I "$site/"
curl.exe -i "$site/customer/profile"
curl.exe -i "$site/api/health"
curl.exe -i "$site/api/workshop-missing"
```


1. **Home and deep link:** expect valid HTTPS, working JS/CSS and correct page or access redirect after refresh. API health returns 200 according to the target group. A missing API path returns its expected error rather than the SPA home page.

2. **Permissions and purchase:** inspect the guest catalog and run the quote flow with demo Customer and Staff accounts. Check volume pricing, a missing SKU, stock and order state before/after. A Customer calling a Staff approval route must be denied by the backend.

3. **Providers:** use a demo product with Cloudinary media, Vietmap address and OTP mailbox. Match a test SePay transaction to its order and check replay in 5.9.4. Record trip state following the delivery confirmation.

## 2. Verify runtime outcomes absent from the captures

**AI:** send a prompt from BTB and match the task log time with the actual model/profile. **RUM:** load the frontend and compare the dataplane request with an event/page view in the monitor. **Queue:** send the order.workshop event from 5.8.6 and inspect SQS body for `eventId=lab-001`. Where a consumer exists, trace one notification record.

**WAF:** record each rule action and associated resource by scope at test time. **Alarm:** match the alarm history and SNS email for the same transition. **CI/CD:** correlate the run with the task revision and frontend asset from 5.10.

## 3. Record demo results

For each case record time with zone, lab hostname, expected and observed result, a redacted capture/log link and pass/fail status. For an unexecuted case identify the missing prerequisite, such as InvokeModel access or a sampled RUM session.
