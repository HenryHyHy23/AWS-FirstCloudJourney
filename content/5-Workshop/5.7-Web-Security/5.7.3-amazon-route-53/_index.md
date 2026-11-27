---
title: "Delegate DNS from Mắt Bão to Route 53"
menuTitle: "Amazon Route 53"
weight: 3
chapter: false
pre: "<b> 5.7.3. </b> "
---

1. **Choose Route 53 → Hosted zones → Create hosted zone.** Enter your registered domain, choose Public hosted zone and create it. Copy all four name servers from its NS record.

{{< btbfigure file="aws-route-53-step-1-create-hosted-zone.webp" caption="Public hosted zone for the team domain." step="yes" >}}

2. **Open Mắt Bão → Domain management → Name Server.** For an existing domain copy the required MX/TXT/ACM records into the new zone first. Select custom name servers, enter all four zone values and save.

{{< btbfigure file="aws-route-53-step-2-setup-route.webp" caption="Name server configuration at Mắt Bão." step="yes" >}}

3. **Return to Route 53 → Create record.** Create apex and www A aliases to CloudFront. Add AAAA aliases if IPv6 is enabled. Create an api A alias to the Singapore ALB. The API origin hostname must resolve to the ALB.

4. **Verify DNS and HTTPS.** Replace YOUR_LAB_DOMAIN with the lab domain.


```powershell
$domain = "YOUR_LAB_DOMAIN"
Resolve-DnsName $domain -Type NS
Resolve-DnsName $domain -Type A
Resolve-DnsName "api.$domain" -Type A
curl.exe -I "https://$domain/"
curl.exe -i "https://$domain/api/health"
```


**Verify.** Verify the expected delegation, valid frontend HTTPS and a backend response for /api/health on the frontend hostname.