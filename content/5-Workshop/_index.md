---
title: "Workshop"
date: 2025-09-09
weight: 5
chapter: true
pre: " <b> 5. </b> "
---

# S3 Static Website & CloudFront Hosting System

In this workshop, we will build a static website hosting and delivery system using **Amazon S3** and **Amazon CloudFront**.

#### System Architecture
The system consists of the following key components:
1.  **Amazon S3 Bucket**: Stores website source code (HTML, CSS, JS, Images). Configured with **Static Website Hosting** feature.
2.  **Amazon CloudFront**: Content Delivery Network (CDN) to accelerate website access for global users and reduce load on S3.
3.  **Security**: Access management via **Block Public Access** and **Bucket Policies/ACLs**.

#### Workshop Contents
The practical labs are divided into sections for easy following:

1.  **[Overview (5.1)](./5.1-workshop-overview)**: Introduction to the workshop and services used.
2.  **[Preparation (5.2)](./5.2-prerequiste)**: Create S3 Bucket and upload sample travel website source code.
3.  **[S3 Hosting Config (5.3)](./5.3-s3-static-web)**: Enable Static Website Hosting on the bucket.
4.  **[Security Config (5.4 - 5.5)](./5.4-config-block-public)**: Configure `Block Public Access` and `ACL` to make the website public.
5.  **[Verify Website (5.6)](./5.6-check-website)**: Access the website via S3 Endpoint.
6.  **[CloudFront Integration (5.7)](./5.7-cloudfront)**: Deploy CloudFront CDN to distribute website content.
7.  **[Cleanup (5.8)](./5.8-cleanup)**: Delete resources to avoid incurring costs.
