---
title: "Week 3 Worklog"
date: 2026-09-28
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

- Understand AWS CLI configuration and service management.
- Learn the VM Import/Export workflow between VMware and EC2.
- Understand schema conversion with AWS SCT and data migration with AWS DMS.
- Learn EC2 cost optimization with Lambda and EventBridge Scheduler.
- Understand Grafana dashboards and CloudWatch integration.
- Learn resource organization with tags and Resource Groups.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Sources |
| --- | --- | --- | --- | --- |
| 1 | - Learn AWS CLI: Credentials, profiles, Regions, and output formats <br> - Understand CLI operations for S3, SNS, IAM, VPC, and EC2 | 28/09/2026 | 28/09/2026 | - [Module 1 - 15: Getting Started with the AWS CLI](https://000011.awsstudygroup.com/) |
| 2 | - Learn VM Import/Export: Migration, backup, S3, and the `vmimport` role <br> - Understand VMware → VMDK → S3 → AMI → EC2 and the reverse export workflow | 29/09/2026 | 29/09/2026 | - [Module 2 - 1: VM Import/Export](https://000014.awsstudygroup.com/) |
| 3 | - Learn AWS SCT: Schema/code object conversion and manual handling of unsupported objects <br> - Learn AWS DMS: Data migration, transaction synchronization, and minimizing downtime | 30/09/2026 | 30/09/2026 | - [Module 2 - 2: Database Schema Conversion & Migration](https://000043.awsstudygroup.com/) |
| 4 | - Learn Lambda/Boto3, IAM permissions, tag selection, and EventBridge rate/cron schedules <br> - Understand EC2 start/stop automation, Slack notifications, and Savings Plans for continuous workloads | 01/10/2026 | 01/10/2026 | - [Module 3 - 1: Optimize EC2 cost with Lambda](https://000022.awsstudygroup.com/) |
| 5 | - Learn Grafana: Data sources, dashboards, Explore, and alerting <br> - Understand CloudWatch integration and EC2 `CPUUtilization` queries | 02/10/2026 | 02/10/2026 | - [Module 3 - 2: Getting started with Grafana basic](https://000029.awsstudygroup.com/) |
| 6 | - Learn Tags: Key/value metadata for Owner, Service, and Environment <br> - Understand Console/CLI filtering and same-Region Resource Groups using tag or CloudFormation queries; review `EnvTest` | 03/10/2026 | 03/10/2026 | - [Module 3 - 3: Manage Resources Using Tags and Resource Groups](https://000027.awsstudygroup.com/) |

### Week 3 Achievements:

- **AWS CLI:** Understood profiles, credentials, output formats, and service commands.
- **VM Import/Export:** Understood VM import as an AMI, EC2 deployment, and export through S3.
- **AWS SCT & DMS:** Understood schema conversion and data synchronization to minimize downtime.
- **Lambda:** Understood scheduled EC2 start/stop, IAM permissions, and notifications.
- **Grafana:** Understood CloudWatch data sources, CPU queries, and dashboards.
- **Tags & Resource Groups:** Understood tag filtering and grouping resources by query.
