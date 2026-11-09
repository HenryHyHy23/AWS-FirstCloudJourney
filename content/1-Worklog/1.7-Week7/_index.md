---
title: "Week 7 Worklog"
date: 2026-10-26
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

- Understand CodePipeline orchestration of source, build/test, and Node.js deployment to EC2.
- Distinguish buildspecs, AppSpec files, artifacts, and IAM roles during deployment.
- Understand how S3 File Gateway connects SMB/NFS shares to S3 and the role of local caching.
- Understand FSx for Windows, Multi-AZ, SMB permissions, performance monitoring, storage savings, and file recovery.
- Understand ingestion, cataloging, ETL, SQL queries, and visualization in an S3-based data lake.
- Understand access-pattern-based DynamoDB modeling, safe updates, CDC, and semantic search integrations.
- Understand Savings Plans, Reserved Instances, and how to align commitments with usage needs.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Sources |
| --- | --- | --- | --- | --- |
| 1 | **Deploying to EC2 with AWS CodePipeline** <br> - Review Node.js deployment to EC2: CodeCommit commits trigger CodePipeline through EventBridge, followed by build/test and deployment stages. <br> - CodeBuild runs commands from `buildspec.yml` and produces artifacts stored in S3. CodeDeploy uses `appspec.yml` to define installed files and lifecycle scripts such as BeforeInstall and ApplicationStart. <br> - EC2 needs a running CodeDeploy Agent, an instance profile that can read artifacts, and tags matching the deployment group. Distinguish service-role permissions from EC2 permissions; check KMS access for KMS-encrypted artifacts. <br> - Inspect stage logs and lifecycle events to distinguish source, build, artifact-download, and application-start failures. Verify the application over HTTP after deployment; a successful build alone does not establish a working release. | 26/10/2026 | 26/10/2026 | - [Module 3 - 21: Deploy applications to EC2 with AWS CodePipeline](https://000023.awsfcaj.com/) |
| 2 | **Deploy File Storage Gateway** <br> - Explore S3 File Gateway for accessing S3 objects as files through SMB/NFS. The lab uses an appliance AMI on EC2, gateway activation, and a separate cache disk. <br> - Create an SMB share backed by a bucket, configure guest access for the lab, and map a Windows drive with `net use`. Distinguish SMB authentication from IAM access to S3; check client connectivity and security groups. <br> - Writes enter the cache before asynchronous upload to S3. Use a sample file to check the corresponding bucket object; a successful mount does not confirm upload completion. Direct S3 changes require a cached inventory refresh to appear on the share. | 27/10/2026 | 27/10/2026 | - [Module 3 - 22: Deploy File Storage Gateway](https://000024.awsfcaj.com/) |
| 3 | **Amazon FSx for Windows File Server** <br> - Explore FSx for Windows as a managed Windows file system using SMB and Active Directory. The lab prepares a VPC and clients through CloudFormation; Multi-AZ synchronously replicates across two AZs and provides automatic failover. <br> - Map shares using DNS/UNC and manage them through Shared Folders/PowerShell; distinguish share/NTFS permissions from IAM resource-management permissions. <br> - Compare SSD/HDD storage and explore read/write measurements with DiskSpd/fio, correlating throughput, IOPS, and latency in CloudWatch. Distinguish storage-capacity increases from throughput-capacity changes. <br> - Deduplication reduces duplicate data; shadow copies restore earlier file versions and consume file-system capacity. Quota tracking monitors usage; enforcement blocks writes when a user exceeds the limit. | 28/10/2026 | 28/10/2026 | - [Module 3 - 23: Amazon FSx for Windows File Server](https://000025.awsfcaj.com/) |
| 4 | **Data Lake on AWS** <br> - Explore a data lake for music-playback analysis: KDG generates JSON → Firehose batches records → S3. Separate raw, reference, and processed data; buffering adds delivery latency. <br> - Glue Crawler infers schemas/partitions and stores metadata in the Data Catalog; the data remains in S3. Distinguish cataloging from ETL. <br> - Review a PySpark notebook that filters activity, joins track reference data on `track_id`, removes unnecessary columns, and writes Parquet; update the catalog for the processed data. <br> - Query with Athena SQL; Parquet helps avoid scanning unnecessary columns. Connect Athena to Quick Sight (Amazon Quick), distinguish datasets, analyses, and dashboards, and check schemas/record counts before visualization. | 29/10/2026 | 29/10/2026 | - [Module 3 - 24: Data Lake on AWS](https://000035.awsfcaj.com/) |
| 5 | **Amazon DynamoDB Immersion Day** <br> - Explore CRUD, Query/Scan, and access-pattern-based PK/SK and GSI design; filters do not reduce read capacity. <br> - Distinguish transactions, backup/PITR, and CDC through Lambda: atomic updates, data recovery, and change processing with duplicate handling. <br> - Review DynamoDB → OpenSearch → Bedrock: embeddings support semantic search, and an LLM generates recommendations from retrieved data; DynamoDB remains the source of truth. | 30/10/2026 | 30/10/2026 | - [Module 3 - 25: Amazon DynamoDB Immersion Day](https://000039.awsfcaj.com/) |
| 6 | **Introduction Savings Plan** <br> - Explore Savings Plans: a USD/hour commitment for one or three years, with three payment options; unused commitment is still charged. <br> - Compare Compute SP flexibility across EC2/Fargate/Lambda with EC2 Instance SP limits by family/Region; review recommendations based on historical usage. <br> - Compare EC2 RIs and Reserved DB Instances; check eligibility, utilization, and coverage to align commitments with actual needs. | 31/10/2026 | 31/10/2026 | - [Module 3 - 26: Introduction Savings Plan](https://000042.awsfcaj.com/) |

### Week 7 Achievements:

- Distinguish the responsibilities of CodeCommit, CodeBuild, CodeDeploy, and CodePipeline.
- Understand artifact, agent, permission, and lifecycle-hook checks for troubleshooting, followed by application validation after deployment.
- Distinguish SMB authentication, IAM access to S3, and asynchronous uploads; understand how to verify the share and destination object.
- Distinguish storage capacity from throughput capacity and quota tracking from enforcement; understand file recovery with shadow copies and the continuing need for backups.
- Distinguish S3 data from catalog metadata and crawlers from ETL; understand enrichment, Parquet, and result checks before building charts.
- Understand how keys/GSIs and filters affect queries and capacity; distinguish transactions, backups, CDC, and search over replicated data.
- Distinguish discount eligibility from spending commitments; compare recommendations, utilization, and coverage with actual needs.
