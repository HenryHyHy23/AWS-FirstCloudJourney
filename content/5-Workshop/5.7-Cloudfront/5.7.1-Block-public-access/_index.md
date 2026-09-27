---
title : "Block Public Access"
date: 2025-09-09
weight : 1
chapter : false
pre : " <b> 5.7.1 </b> "
---

1. In S3 Bucket Interface
    - Select **Permissions**
    - Currently, the **Block all public access** function is **Off** because we disabled it in step 4
    - Select **Edit**
![Block public access](/images/5-Workshop/5.7-Cloudfront/5.7.1-Block-public-access/1.png)
    -   Select **Block all public access**
    -   Select **Save changes**

![Block public access](/images/5-Workshop/5.7-Cloudfront/5.7.1-Block-public-access/2.png)
    - Another window appears to confirm the **edit**, type `confirm`
    - Select **Confirm**

![Block public access](/images/5-Workshop/5.7-Cloudfront/5.7.1-Block-public-access/3.png)
    -   **Block all public access** is now enabled, at this point no public access can connect to your S3 Bucket.

![Block public access](/images/5-Workshop/5.7-Cloudfront/5.7.1-Block-public-access/4.png)

2. Check Block all public access function
    -   In S3 bucket interface
    - Select **Properties**

![Block public access](/images/5-Workshop/5.7-Cloudfront/5.7.1-Block-public-access/5.png)
    -   Scroll to the bottom of the page, at the **Static website hosting** section
    -   Select the **square** icon to copy URL

![Block public access](/images/5-Workshop/5.7-Cloudfront/5.7.1-Block-public-access/6.png)
    -   Open URL in a new browser tab

![Block public access](/images/5-Workshop/5.7-Cloudfront/5.7.1-Block-public-access/7.png)

-> Congratulations, you have successfully Blocked public access
