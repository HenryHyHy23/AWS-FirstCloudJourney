---
title: "Week 2 Worklog"
date: 2026-09-21
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Week 2 Objectives:

- Compare storage, RDS, Lightsail, and shared file systems.
- Understand Auto Scaling, CloudWatch, and hybrid DNS.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Sources |
| --- | --- | --- | --- | --- |
| 1 | **Storage and S3 static websites** <br> - Review AWS storage and S3/CloudFront website delivery. S3 stores objects, EBS provides block storage, and EFS/FSx provide shared files. <br> - S3 Versioning retains older versions; lifecycle rules manage retention and costs. CloudFront caches content; OAC protects regular S3 origins. CORS does not grant S3 access. | 21/09/2026 | 22/09/2026 | - [Module 1 - 9: Hosting Static Website with Amazon S3](https://000057.awsstudygroup.com/) <br> - [Lab video: Create an S3 Bucket](https://youtu.be/3vSrTeWroSs?si=KmSKod77TeDMpitx) <br> - [Video: AWS Storage Overview](https://youtu.be/hsCfP0IxoaM) <br> - [Video: S3, CORS, Access Control & Glacier](https://youtu.be/mPBjB6Ltl_Q) <br> - [Video: Snow Family, Storage Gateway & Backup](https://youtu.be/YXn8Q_Hpsu4) |
| 2 | **RDS availability and recovery** <br> - Compare database availability, read scaling, and recovery options. Multi-AZ DB instances use a standby for failover; that standby does not serve reads. <br> - Read Replicas scale reads asynchronously and can lag behind the source. Point-in-time recovery creates a new DB instance; applications need the new endpoint. | 23/09/2026 | 23/09/2026 | - [Module 1 - 10: Amazon Relational Database Service](https://000005.awsstudygroup.com/) |
| 3 | **Lightsail, shared storage, and migration** <br> - Review Lightsail, shared file systems, and server migration use cases. Lightsail bundles resources; upsizing uses a larger instance created from a snapshot. <br> - EFS uses NFS; FSx provides workload-specific file systems. MGN rehosts servers; applications and connectivity still need validation after migration. | 24/09/2026 | 24/09/2026 | - [Module 1 - 11: Amazon Lightsail Workshop](https://000045.awsstudygroup.com/) <br> - [Video: Auto Scaling, EFS/FSx, Lightsail & MGN](https://youtu.be/hFVYG8WqfU0) |
| 4 | **Load balancing and Auto Scaling** <br> - Examine ASG capacity/replacement and load-balancer traffic distribution. ASG maintains desired capacity within min/max limits; load balancers route to healthy targets. <br> - Choose suitable scaling metrics and warmup periods. Use launch templates and keep shared sessions and durable data outside instances. | 25/09/2026 | 25/09/2026 | - [Module 1 - 12: EC2 Auto Scaling Group](https://000006.awsstudygroup.com/) <br> - [Video: EC2 Auto Scaling](https://youtu.be/bbLcPitXJSY) |
| 5 | **Monitoring with CloudWatch** <br> - Review metric collection, log analysis, and operational alarms. Check metric namespace, dimensions, statistic, period, and time range. <br> - Guest memory metrics need an agent; Logs Insights helps analyse logs. Configure alarm thresholds, missing-data handling, and appropriate SNS notifications. | 26/09/2026 | 26/09/2026 | - [Module 1 - 13: Amazon CloudWatch Workshop](https://000008.awsstudygroup.com/) |
| 6 | **Hybrid DNS with Route 53 Resolver** <br> - Trace DNS queries between on-premises networks and AWS. Inbound Resolver handles on-premises queries into AWS; outbound forwards VPC queries to on-premises DNS. <br> - VPN/Direct Connect connectivity, routes, and TCP/UDP 53 access are required. Use endpoints across AZs, domain-specific forwarding, and loop-free rules. | 27/09/2026 | 27/09/2026 | - [Module 1 - 14: Set up Hybrid DNS with Route 53 Resolver](https://000010.awsstudygroup.com/) |

### Week 2 Achievements:

- Distinguish database failover, read scaling, and recovery.
- Relate scaling, metrics/alerts, and DNS to application requirements.
