---
title: "Week 4 Worklog"
date: 2026-10-05
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives:

- Understand tag-based access, permissions boundaries, and Identity Center.
- Review Systems Manager prerequisites and CloudFormation infrastructure management.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Sources |
| --- | --- | --- | --- | --- |
| 1 | **Tag-based EC2 access** <br> - Examine tag conditions for creating, modifying, and accessing EC2 resources. Distinguish `aws:RequestTag` from existing resource tags; `aws:TagKeys` limits permitted keys. <br> - Tagging EC2 at creation needs `CreateTags`; protect authorization-tag edits. Trust policies control who can assume roles; permissions policies control allowed actions. | 05/10/2026 | 05/10/2026 | - [Module 3 - 4: Managing EC2 Resource Tag Access with AWS IAM](https://000028.awsstudygroup.com/) <br> - [Docs: Tag-on-create authorization](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/supported-iam-actions-tagging.html) <br> - [Docs: IAM tag conditions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html) |
| 2 | **Patching and commands with Systems Manager** <br> - Review Patch Manager and Run Command for multi-server operations. Managed nodes need SSM Agent, an IAM role, and endpoint connectivity. <br> - Patch baselines define approved patches; distinguish `Scan`/`Scan and install` and check compliance/reboots. Run Command targets multiple servers; inspect each node's output and set concurrency/error limits. | 06/10/2026 | 06/10/2026 | - [Module 3 - 5: Manage Patches and Run Commands on Multiple Servers with AWS Systems Manager](https://000031.awsstudygroup.com/) <br> - [Docs: Patch Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager.html) <br> - [Docs: Command concurrency](https://docs.aws.amazon.com/systems-manager/latest/userguide/send-commands-multiple.html) |
| 3 | **Server access through Session Manager** <br> - Review public/private EC2 access, logging, and port forwarding through Session Manager. Session Manager uses outbound HTTPS without inbound SSH/RDP rules. <br> - Private instances need endpoints, private DNS, and suitable TCP 443 rules. S3 logs require permissions and connectivity; session logs do not capture port-forwarding contents. | 07/10/2026 | 07/10/2026 | - [Module 3 - 6: Working with AWS Systems Manager - Session Manager](https://000058.awsstudygroup.com/) <br> - [Session logging limitations](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-logging.html) <br> - [Docs: SSM VPC endpoints](https://docs.aws.amazon.com/systems-manager/latest/userguide/setup-create-vpc.html) |
| 4 | **Infrastructure management with CloudFormation** <br> - Review templates, stack deployment, and infrastructure change controls. YAML/JSON templates declare resources; stacks manage their lifecycle. <br> - Use cfn-lint and change sets; drift detection identifies changes without repairing them. Lambda custom resources handle Create/Update/Delete; StackSets deploy across accounts/Regions. | 08/10/2026 | 08/10/2026 | - [Module 3 - 7: AWS CloudFormation](https://000037.awsstudygroup.com/) <br> - [Docs: Change sets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-changesets.html) <br> - [Docs: Custom resources](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-custom-resources.html) <br> - [Docs: Drift detection](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-stack-drift.html) |
| 5 | **Centralized access with IAM Identity Center** <br> - Review multi-account organization and group-based workforce access. Organizations/OUs organize accounts; SCPs limit permissions without granting access. <br> - Identity Center provisions roles through groups, account assignments, and permission sets. Use `aws configure sso`/`aws sso login`; referenced customer-managed policies need matching target-account names/paths. | 09/10/2026 | 09/10/2026 | - [Module 3 - 8: Using AWS IAM Identity Center for Robust Identity Management](https://000012.awsstudygroup.com/) <br> - [Docs: Permission sets](https://docs.aws.amazon.com/singlesignon/latest/userguide/permissionsetsconcept.html) <br> - [Docs: Customer-managed policies](https://docs.aws.amazon.com/singlesignon/latest/userguide/howtocmp.html) <br> - [Docs: CLI SSO sessions](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html) |
| 6 | **Limiting permissions with boundaries** <br> - Examine effective access when broad EC2 policies are constrained by Region. Boundaries limit user/role identity-based permissions; they neither attach to groups nor grant access. <br> - The lab permits EC2 only in `ap-southeast-1`; Sydney is implicitly denied. Protect boundary edits/removal and evaluate other applicable policies. | 10/10/2026 | 10/10/2026 | - [Module 3 - 9: Limitation of User Rights with IAM Permission Boundary](https://000030.awsstudygroup.com/) <br> - [Permissions boundaries (AWS IAM)](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html) <br> - [Docs: Policy evaluation](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html) |

### Week 4 Achievements:

- Distinguish tag conditions, trust policies, boundaries, and SCPs.
- Understand SSM connectivity/logging requirements, change sets, drift checks, and permission sets.
