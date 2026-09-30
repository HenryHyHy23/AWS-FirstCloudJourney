---
title: "Week 2 Worklog"
date: 2026-09-21
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}

### Week 2 Objectives:

- Learn Amazon S3 and practice hosting a static website.
- Configure CloudFront to distribute content more securely and efficiently.

### Tasks to be carried out this week:

| Day | Task                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Start Date | Completion Date | Reference Sources                                                                                                                                              |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | - Learn Amazon S3: <br> &emsp; + Object storage model, difference between Buckets and Objects <br> &emsp; + Storage Classes: Standard, Intelligent-Tiering, Standard-IA, Glacier <br> &emsp; + Security mechanisms: IAM Policies, Bucket Policies, ACLs, Encryption <br> - Learn Amazon CloudFront and how content is distributed through Edge Locations <br> - Learn about S3 Versioning and Cross-Region Replication (CRR) <br> - **Practice:** <br> &emsp; + Create an S3 Bucket and upload static website source code <br> &emsp; + Enable Static Website Hosting, configure Block Public Access and ACLs <br> &emsp; + Create a CloudFront Distribution and configure Bucket Policy for CloudFront <br> &emsp; + Enable Bucket Versioning, upload a new version, and restore the previous one <br> &emsp; + Move Objects between two Buckets <br> &emsp; + Configure Cross-Region Replication to us-east-1 | 21/09/2026 | 22/09/2026      | - [Lab 57: Hosting Static Website with Amazon S3](https://000057.awsstudygroup.com/) <br> - [Video Tutorial](https://youtu.be/3vSrTeWroSs?si=KmSKod77TeDMpitx) |

### Week 2 Achievements:

- Understood Amazon S3's object storage model:
  - Bucket is a container (globally unique name, max 100 buckets per account)
  - Object is the actual data stored inside (0 bytes to 5 TB per object)
  - Storage Classes: Standard, Intelligent-Tiering, Standard-IA, Glacier
  - Permissions managed via IAM Policies, Bucket Policies, and ACLs

- Successfully hosted a static website on S3 and configured CloudFront to distribute content without making the bucket public:
  - Page load time dropped from ~136ms (direct S3) to ~10ms (via CloudFront)
  - Nearest PoP confirmed as SGN50-P2 (Ho Chi Minh City)

- Practiced Bucket Versioning: uploaded a new version of an object and restored the previous version by deleting the latest one.

- Configured Cross-Region Replication to automatically sync objects from ap-southeast-1 to us-east-1 for Disaster Recovery, following the Reliability pillar of the AWS Well-Architected Framework.
