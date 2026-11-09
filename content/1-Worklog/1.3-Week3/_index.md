---
title: "Week 3 Worklog"
date: 2026-09-28
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

- Use AWS CLI and review VM/database migration.
- Understand EC2 scheduling, Grafana dashboards, and resource tagging.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Sources |
| --- | --- | --- | --- | --- |
| 1 | **Working with AWS CLI** <br> - Review CLI commands, output filtering, and error handling. Check profile, account, Region, and caller identity before AWS CLI commands. <br> - Service filters run on AWS; `--query` filters/formats locally. Account for pagination. Use JSON output, check exit codes, and prefer temporary credentials. | 28/09/2026 | 28/09/2026 | - [Module 1 - 15: Getting Started with the AWS CLI](https://000011.awsstudygroup.com/) |
| 2 | **VM Import/Export** <br> - Review VM image import into AWS and export restrictions. Check OS, disk format, boot mode, drivers, and licensing before VM import. <br> - The `vmimport` role needs trust and S3 permissions; monitor the asynchronous task. Validate boot and applications after import; export has separate restrictions. | 29/09/2026 | 29/09/2026 | - [Module 2 - 1: VM Import/Export](https://000014.awsstudygroup.com/) |
| 3 | **Database conversion and migration** <br> - Separate schema conversion from data migration. SCT assesses and converts schema; incompatible objects need manual changes. <br> - DMS full load moves initial data; CDC replicates changes and can lag. Before cutover, validate data, applications, replication lag, and rollback plans. | 30/09/2026 | 30/09/2026 | - [Module 2 - 2: Database Schema Conversion & Migration](https://000043.awsstudygroup.com/) |
| 4 | **Scheduling EC2 start/stop** <br> - Review Scheduler invoking Lambda for business-hour EC2 start/stop. Use cron with an explicit time zone; `rate(8 hours)` does not mean 08:00. <br> - Filter EC2 by tags/state; separate Lambda's EC2 access from Scheduler invocation permissions. Stopped EC2 still incurs EBS costs; an API notification does not confirm the final state. | 01/10/2026 | 01/10/2026 | - [Module 3 - 1: Optimize EC2 cost with Lambda](https://000022.awsstudygroup.com/) |
| 5 | **Grafana dashboards with CloudWatch** <br> - Review Grafana's CloudWatch data source and dashboard configuration. Grafana queries CloudWatch; check Region, InstanceId, statistic, and period for missing data. <br> - Grafana on EC2 can use an instance role with required read permissions. Compare Explore queries with panels; configure alerts and notification destinations. | 02/10/2026 | 02/10/2026 | - [Module 3 - 2: Getting started with Grafana basic](https://000029.awsstudygroup.com/) |
| 6 | **Tags and Resource Groups** <br> - Review tagging for inventory, resource selection, and automation. Standardize Owner, Service, and Environment tags; tags are case-sensitive. <br> - Resource Groups select by tags; tag changes can change membership. Groups are not IAM/network boundaries; protect tags used for authorization. | 03/10/2026 | 03/10/2026 | - [Module 3 - 3: Manage Resources Using Tags and Resource Groups](https://000027.awsstudygroup.com/) |

### Week 3 Achievements:

- Distinguish schema conversion, data replication, and post-migration application validation.
- Understand EC2 schedules, Grafana queries, and tagging/Resource Group limits.
