---
title: "Week 5 Worklog"
date: 2026-10-12
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

- Understand IAM roles/conditions and Security Hub CSPM findings.
- Review WAF rules/logs, SSE-KMS permissions, and backup/restore workflows.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Sources |
| --- | --- | --- | --- | --- |
| 1 | **IAM roles and access conditions** <br> - Review EC2/RDS permissions and IP/time restrictions on role assumption. Assign EC2/RDS access through users/groups; separate role trust from permissions policies. <br> - STS issues temporary credentials; assumed-role sessions use role permissions. Trust-policy IP/time conditions gate AssumeRole only. `Z` means UTC; the time window does not terminate existing sessions. | 12/10/2026 | 12/10/2026 | - [Module 3 - 10: IAM Role & Condition](https://000044.awsstudygroup.com/) |
| 2 | **Security assessment with Security Hub CSPM** <br> - Review aggregated findings and configuration checks against security standards. Security Hub CSPM aggregates findings and checks configurations against FSBP, CIS, and PCI DSS. <br> - Check AWS Config account/Region coverage and evaluation data before interpreting scores. Prioritize findings by severity; disabling controls needs a reason and does not fix resources. | 13/10/2026 | 13/10/2026 | - [Module 3 - 11: Getting Started with AWS Security Hub](https://000018.awsfcaj.com/) |
| 3 | **Application protection with AWS WAF** <br> - Review managed/custom rules for inspecting Juice Shop requests. Create a Juice Shop web ACL with managed Core/SQL groups and custom rules. <br> - Lower priorities run first; Allow/Block terminate evaluation, while Count continues. CloudFront web ACLs are managed in `us-east-1`; WCU measures rule capacity. Log redaction does not cover sampled requests. | 14/10/2026 | 14/10/2026 | - [Module 3 - 12: AWS Web Application Firewall](https://000026.awsfcaj.com/) |
| 4 | **Testing and tuning AWS WAF** <br> - Examine matching and nonmatching requests to reduce false positives. Size constraints measure bytes; `username GT 0` only means a nonempty value. <br> - Test the chocolate/nuts header pair OR banana/sauce query pair; both conditions in each pair are required. Use Count and inspect matches/logs and false positives before Block. Firehose needs Direct PUT, `aws-waf-logs-` names, and the correct Region. | 15/10/2026 | 15/10/2026 | - [Module 3 - 13: Using AWS WAF](https://000026.awsfcaj.com/3-useawswaf/) |
| 5 | **S3 encryption with AWS KMS** <br> - Review object encryption, decryption permissions, and access auditing. SSE-KMS encrypts objects with data keys; the KMS key protects those data keys. <br> - Reads need `s3:GetObject` and `kms:Decrypt`; key policies/IAM control key usage. Enable S3 data events and query CloudTrail with Athena. Presigned access is bounded by signer permissions, URL expiry, and credential validity. | 16/10/2026 | 16/10/2026 | - [Module 3 - 14: Encryption at Rest with AWS KMS](https://000033.awsfcaj.com/) |
| 6 | **Backup and restore testing** <br> - Review backup plans and automated restores to assess recoverability. RPO defines the maximum acceptable data-loss interval; RTO defines maximum service interruption. <br> - Backup plans define schedules/retention; resource assignments select resources, and vaults hold recovery points. Backup → SNS → Lambda automates restores. Validate applications/logs, clean up test resources, and handle failures/duplicate notifications. | 17/10/2026 | 17/10/2026 | - [Module 3 - 15: Deploy AWS Backup to the System](https://000013.awsfcaj.com/) |

### Week 5 Achievements:

- Distinguish role-assumption conditions from API permissions, and failed controls from missing evaluations.
- Understand Count/Block, AND/OR logic, and logs for identifying blocking rules.
- Separate S3/KMS permissions; completed backups still require restore and application validation.
