---
title: "Week 4 Worklog"
date: 2026-10-05
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives:

- Understand IAM least privilege, tag/Region conditions, and secure role assumption for EC2.
- Learn centralized node management, patch compliance, and bulk commands with Systems Manager.
- Understand Session Manager private access, session logging, and RDP port forwarding.
- Learn CloudFormation templates, stack lifecycle, validation, Custom Resources, StackSets, and Drift Detection.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Sources |
| --- | --- | --- | --- | --- |
| 1 | - Learn IAM least privilege and EC2 tag-based access: `Team=Alpha`, allowed Regions (`us-east-1`, `us-west-1`) <br> - Review five policy functions: read, tag at creation, tag existing resources, launch, and manage instances; distinguish request tags, resource tags, and allowed tag keys <br> - Understand trust policies, MFA, AssumeRole, and conditional allow/deny scenarios | 05/10/2026 | 05/10/2026 | - [Module 3 - 4: Managing EC2 Resource Tag Access with AWS IAM](https://000028.awsstudygroup.com/) |
| 2 | - Learn Systems Manager: Centralized operations, Fleet Manager, managed nodes, and `AmazonSSMManagedInstanceCore` permissions <br> - Understand Patch Manager: Patch scanning/installation, compliance, and reboot options <br> - Learn Run Command: `AWS-RunPowerShellScript` (`net user`) across multiple Windows nodes, per-node output/errors, and optional S3 output | 06/10/2026 | 06/10/2026 | - [Module 3 - 5: Manage Patches and Run Commands on Multiple Servers with AWS Systems Manager](https://000031.awsstudygroup.com/) |
| 3 | - Learn Session Manager: IAM/SSM role permissions and public/private EC2 access without opening inbound SSH/RDP ports <br> - Understand Interface Endpoints, private DNS, HTTPS 443, and S3 Gateway Endpoint routing for private log delivery <br> - Distinguish Session history from S3 shell logs; learn CLI/Session Manager Plugin and RDP forwarding (`localhost:9999` → `3389`) | 07/10/2026 | 07/10/2026 | - [Module 3 - 6: Working with AWS Systems Manager - Session Manager](https://000058.awsstudygroup.com/) <br> - [Session logging limitations](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-logging.html) |
| 4 | - Learn CloudFormation IaC: YAML/JSON templates, Parameters/Resources/Outputs, intrinsic functions (`Ref`, `GetAtt`, `Sub`), and dependencies <br> - Understand stack validation/deployment through CloudShell/CLI with `cfn-lint` and `taskcat`; Lambda Custom Resources and SSM Parameter Store <br> - Review Mappings, StackSets across accounts/Regions, and Drift Detection (`IN_SYNC`/`DRIFTED`) | 08/10/2026 | 08/10/2026 | - [Module 3 - 7: AWS CloudFormation](https://000037.awsstudygroup.com/) |

### Week 4 Achievements:

- **IAM:** Distinguished request/resource tags and understood Region/tag-key restrictions, trust policies, MFA, and role assumption.
- **Systems Manager:** Understood managed-node IAM permissions, patch scan/install/compliance, and Run Command documents with per-node results.
- **Session Manager:** Understood private endpoint access, shell logs in S3, and RDP tunneling; port-forwarding content is not logged.
- **CloudFormation:** Understood reusable templates, intrinsic functions, dependency ordering, Lambda Custom Resources with SSM Parameter Store, Mappings, multi-account/Region StackSets, and Drift Detection.
