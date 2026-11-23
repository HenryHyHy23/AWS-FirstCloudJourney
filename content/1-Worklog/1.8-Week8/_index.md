---
title: "Week 8 Worklog"
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives:

- Start working on the final project.
- Review Java application migration, automated releases, and image processing as a separate microservice.
- Learn to choose an EC2 instance size based on workload metrics.
- Analyze AWS spending with Cost Explorer.
- Query detailed cost and usage data with AWS Glue and Amazon Athena.

### Tasks to be carried out this week:

| Day | Task                               | Start Date | Completion Date | Resources |
| --- | ---------------------------------- | ---------- | --------------- | --------- |
| 1 | **Right-sizing Amazon EC2** <br> - Review the EC2 lab setup, attach an IAM role for Systems Manager and CloudWatch, and verify temporary credentials with STS. <br> - Use Run Command to install and configure the CloudWatch agent for RAM usage alongside standard CPU and network metrics. <br> - Review Compute Optimizer recommendations in Cost Optimization Hub. Use data that captures peak demand before resizing or making a pricing commitment. <br> - **Final project:** Research suitable AWS services in preparation for drawing the project's architecture diagram. | 02/11/2026 | 02/11/2026 | - [Module 3 - 27: Right-sizing Amazon EC2](https://000032.awsfcaj.com/) |
| 2 | **Cost visualization with AWS Cost Explorer** <br> - Read daily and monthly cost charts, filter by service, and group by Region or usage type to identify charges such as NAT Gateway hours. Save and delete a sample report. <br> - Distinguish Savings Plans and Reserved Instances coverage from utilization. Use activated cost allocation tags to break down spending by project. <br> - Trace data transfer and NAT Gateway charges by usage type. <br> - **Final project:** Continue working on the project. | 03/11/2026 | 03/11/2026 | - [Module 3 - 28: Cost visualization with AWS Cost Explorer](https://000034.awsfcaj.com/) |
| 3 | **Cost and performance analysis with AWS Glue and Amazon Athena** <br> - Review how Glue Crawler catalogs sample Cost and Usage Report (CUR) Parquet files in S3 without running an ETL job. <br> - Use Athena SQL to break down costs by account, service, usage type, and `CostCenter` tag and identify unused Reserved Instances. <br> - Select only the columns and partitions needed to reduce data scanned. <br> - **Final project:** Continue working on the project. | 04/11/2026 | 04/11/2026 | - [Module 3 - 29: Cost and performance analysis with AWS Glue and Amazon Athena](https://000040.awsfcaj.com/) |
| 4 | **Migrating the TravelBuddy monolith** <br> - Review how CloudFormation provisions VPC, EC2, and RDS for the lab. Prepare the database and check the Java application locally in Eclipse. <br> - Study how to deploy TravelBuddy on Elastic Beanstalk, update the application with the EB CLI, and verify the API after deployment. | 05/11/2026 | 05/11/2026 | - [Module 4 - 1: Migrating the Monolith](https://000050.awsfcaj.com/) |
| 5 | **Automating application releases** <br> - Trace the TravelBuddy release from a Git push through CodePipeline and CodeBuild to Elastic Beanstalk. Review the Maven cache in S3 and verify the deployed version at the application endpoint. <br> - Review the legacy CodeStar setup and identify the pipeline stages to configure directly today. | 06/11/2026 | 06/11/2026 | - [Module 4 - 2: Configure app auto-release](https://000051.awsfcaj.com/) |
| 6 | **Serverless microservices and image processing** <br> - Review how S3 upload events invoke Lambda to generate thumbnails. Use a separate output bucket or an input prefix filter to prevent recursive invocations. <br> - Review how AWS SAM packages the function and trigger, and how a separate Lambda function stores book metadata in DynamoDB. Check output objects, logs, and table items when testing. | 07/11/2026 | 07/11/2026 | - [Module 4 - 3: Create a Microservice](https://000052.awsfcaj.com/) <br> - [Image Processing and Optimization on AWS](https://000078.awsfcaj.com/) |

### Week 8 Achievements:

- Initiated brainstorming sessions for the Final Project.
- Conducted research on the services the team plans to use in the Final Project.
- Designed the system architecture incorporating the AWS services selected for the Project.
- Understood how to review EC2 sizing recommendations against representative workload metrics before changing instance size.
- Learned to identify what drives AWS spending and interpret commitment coverage and utilization.
- Learned how to use CUR line items to trace AWS costs beyond the summaries in Cost Explorer.
- Understood the TravelBuddy deployment flow on Elastic Beanstalk, the CI/CD stages, and how S3, Lambda, and DynamoDB work together.
