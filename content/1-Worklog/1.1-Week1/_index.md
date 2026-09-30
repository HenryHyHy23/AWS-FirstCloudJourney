---
title: "Week 1 Worklog"
date: 2026-09-14
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}

### Week 1 Objectives:

- Get familiar with the AWS Cloud platform and the AWS Management Console.
- Explore core AWS services and complete foundational labs within the AWS Free Tier.

### Tasks to be carried out this week:

| Day | Task                                                                                                                                                                                                                                                                                                                                                                     | Start Date | Completion Date | Reference Sources                                                                                                                                                                                                                                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1   | - Get familiar with AWS Management Console <br> - Learn about AWS Free Tier, promotional credits, and pricing models <br> - Explore AWS Billing & Cost Management <br> - **Practice:** <br> &emsp; + Create and configure an AWS Free Tier account <br> &emsp; + Set up AWS Budgets to monitor spending                                                                  | 14/09/2026 | 15/09/2026      | - [Module 1 - 1: Create new AWS Account](https://000001.awsstudygroup.com/) <br> - [Module 1 - 2: Manage costs with AWS Budgets](https://000007.awsstudygroup.com/) <br> - [Module 1 - 3: Request support with AWS Support](https://aws.amazon.com/support/) |
| 2   | - Learn AWS Identity and Access Management (IAM): <br> &emsp; + Users, Groups, Roles, Policies <br> &emsp; + Least Privilege principle <br> - **Practice:** <br> &emsp; + Create IAM users, groups, and attach policies <br> &emsp; + Basic AWS CLI usage                                                                                                                | 15/09/2026 | 16/09/2026      | - [Module 1 - 4: Access Management with AWS IAM](https://000002.awsstudygroup.com/)                                                                                                                                                                          |
| 3   | - Learn Amazon Virtual Private Cloud (VPC): <br> &emsp; + CIDR, Subnets (public/private) <br> &emsp; + Route Tables, Internet Gateway, NAT Gateway <br> &emsp; + Security Groups, Network ACLs <br> &emsp; + Site-to-Site VPN <br> - **Practice:** <br> &emsp; + Create a VPC and configure networking components                                                        | 16/09/2026 | 17/09/2026      | - [Module 1 - 5: Deploy network infrastructure with Amazon VPC](https://000003.awsstudygroup.com/)                                                                                                                                                           |
| 4   | - Learn Amazon EC2: <br> &emsp; + Instance Types, AMI, EBS <br> &emsp; + Key Pairs, Security Groups <br> - **Practice:** <br> &emsp; + Launch and configure an EC2 instance <br> &emsp; + Connect to EC2 via SSH <br> &emsp; + Manage instance lifecycle                                                                                                                 | 17/09/2026 | 18/09/2026      | - [Module 1 - 6: Deploy applications on Amazon EC2](https://000004.awsstudygroup.com/)                                                                                                                                                                       |
| 5   | - Learn how to grant application permissions to access AWS services <br> - Understand the security risk of hardcoding Access Keys in source code <br> - Learn about Instance Metadata and AWS STS <br> - **Practice:** <br> &emsp; + Connect a Python app on EC2 to S3 using Access Keys <br> &emsp; + Connect using an IAM Role attached to the EC2 instance            | 18/09/2026 | 19/09/2026      | - [Module 1 - 7: Grant application permissions via IAM Role](https://000048.awsstudygroup.com/)                                                                                                                                                              |
| 6   | - Learn about AWS CloudShell and the browser-based command-line environment <br> - Understand Linux `rwx` file permission format <br> - **Practice:** <br> &emsp; + Basic Linux commands: `pwd`, `cd`, `mkdir`, `touch`, `cat`, `vi`, `ls -la` <br> &emsp; + Run AWS CLI commands in CloudShell <br> &emsp; + Upload/download files between local machine and CloudShell | 19/09/2026 | 20/09/2026      | - [Module 1 - 8: Getting Started with AWS CloudShell](https://000006.awsstudygroup.com/)                                                                                                                                                                     |

### Week 1 Achievements:

- Successfully created an AWS Free Tier account, got familiar with the AWS Management Console, and set up AWS Budgets to keep track of spending.

- Understood the core components of IAM:
  - Users, Groups, Policies, Roles
  - Least Privilege principle
  - Practiced creating IAM users, groups, and assigning permissions

- Understood basic networking with Amazon VPC:
  - Subnets (public/private), Route Tables, Internet Gateway, NAT Gateway
  - Security Groups and Network ACLs
  - Successfully created a VPC and connected to an EC2 instance via SSH

- Recognized the security risk of hardcoding Access Keys in source code, and learned how to attach an IAM Role to an EC2 instance so the application can upload files to S3 using temporary credentials via Instance Metadata and AWS STS.

- Got comfortable with AWS CloudShell, practiced basic Linux commands, and ran AWS CLI commands directly in the browser without needing any local setup.
