---
title: "Store the React build in S3"
menuTitle: "Amazon S3"
weight: 2
chapter: false
pre: "<b> 5.5.2. </b> "
---

1. **Search for Amazon S3 → open Buckets.**

{{< btbfigure file="aws-s3-step-1.webp" caption="Search for Amazon S3 → open Buckets." step="yes" >}}

2. **Choose Create bucket → General purpose.**

{{< btbfigure file="aws-s3-step-2-create-bucket.webp" caption="Choose Create bucket → General purpose." step="yes" >}}

3. **Enter the bucket name and AWS Region.** Choose a valid unique name. Record it for the frontend upload and CloudFront origin.

{{< btbfigure file="aws-s3-step-3-general-configuration.webp" caption="Frontend bucket general configuration." step="yes" >}}

4. **Select Bucket owner enforced.** Keep Block all public access enabled, choose default encryption and create the bucket. CloudFront will read through OAC in 5.7.2.

{{< btbfigure file="aws-s3-step-4-setup-ownership.webp" caption="Object Ownership and Block Public Access." step="yes" >}}

5. **Build and upload the storefront.** Set the lab API base URL in the frontend repo. Replace BUCKET_NAME and BUCKET_REGION before running the commands.


```powershell
npm ci
npm run build
Get-ChildItem dist
aws s3 sync dist/ s3://BUCKET_NAME/ --region BUCKET_REGION
```


**Verify.** In Objects verify index.html and assets/ are at the root. Once CloudFront is connected, open a JS asset and check its Content-Type.
