---
title: "Week 5 Worklog"
date: 2026-10-12
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

- Distinguish user/group permissions, role trust, and the permissions of an assumed-role session.
- Explain temporary STS credentials and evaluate role access using request context.
- Interpret IP/time conditions, time zones, and the difference between role assumption and subsequent service access.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Sources |
| --- | --- | --- | --- | --- |
| 1 | - **Identity and permission scope:** Compare direct EC2/RDS policies with permissions inherited through a group. A group organizes users and is not itself a request principal. Service-wide FullAccess policies illustrate separation of duties but still exceed many routine tasks. <br> - **AssumeRole flow:** A trust policy defines who may assume a role; its permissions policy defines allowed service actions. STS returns an access key ID, secret access key, session token, and expiration. The role session uses role permissions rather than adding them to the user's existing permissions. <br> - **Request conditions:** `IpAddress` with `aws:SourceIp` checks the public source address seen by AWS, which may be a NAT/VPN address. `DateGreaterThan`/`DateLessThan` with `aws:CurrentTime` define a fixed interval; `Z` denotes UTC, not Vietnam local time. <br> - **Control scope:** IP/time conditions in the trust policy gate `sts:AssumeRole`. They do not automatically constrain later EC2/RDS calls or invalidate issued credentials; continued-use restrictions need appropriate permissions-policy or organization controls. | 12/10/2026 | 12/10/2026 | - [Module 3 - 10: IAM Role & Condition](https://000044.awsstudygroup.com/) <br> - [AWS Docs: AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) <br> - [AWS Docs: Condition keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) <br> - [AWS Security Blog: Role trust policies](https://aws.amazon.com/blogs/security/how-to-use-trust-policies-with-iam-roles/) |

### Week 5 Achievements:

- **Least privilege:** Can distinguish separating EC2/RDS administration from granting only the actions needed. Broad managed policies remain a starting point for analysis rather than proof of least privilege.
- **Trust reasoning:** A same-account user explicitly trusted by a role can be allowed to assume it without a separate identity-policy allow. No direct service permissions is different from an applicable explicit deny.
- **Conditional access:** Can separate an assumption failure from a later API denial. A trust-policy time window is not a daily schedule or a guarantee that an existing session ends at that boundary.
