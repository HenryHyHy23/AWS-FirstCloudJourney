---
title : "1. Workshop Overview"
date: 2025-09-09
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Hands-on workshop Overview

In this workshop, we will practice deploying a static website on **Amazon S3** storage service and optimizing content delivery via **Amazon CloudFront** Content Delivery Network (CDN).

#### Key Contents:

1.  **Preparation (5.2)**: Create an S3 Bucket and upload sample website source code to the bucket.
2.  **Configure S3 Static Hosting (5.3)**: Enable static website hosting feature on S3.
3.  **Access Management (5.4 - 5.5)**: Configure *Block Public Access* and *Access Control Lists (ACL)* to allow users to access the website from the internet.
4.  **Verify Website (5.6)**: Verify the website operation via the S3 Endpoint.
5.  **CloudFront Integration (5.7)**:
    -   Initialize a CloudFront Distribution pointing to the S3 origin.
    -   Enhance security by blocking direct public access to S3 (Block Public Access) and routing traffic through CloudFront.
6.  **Cleanup (5.8)**: Instructions on deleting created resources to avoid incurring costs.