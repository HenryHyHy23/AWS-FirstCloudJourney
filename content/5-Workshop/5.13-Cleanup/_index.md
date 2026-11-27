---
title: "Clean up lab resources in dependency order"
menuTitle: "Cleanup & Troubleshooting"
weight: 13
chapter: false
pre: "<b> 5.13. </b> "
---



Apply this only to resources dedicated to the exercise. Check account, Region, name/tag and associations before deleting. Save demo outcomes, revision and cost inputs for the report.

## 1. Stop data producers

Stop lab deployment triggers and remove the RUM client from the lab frontend. Disable the EventBridge rule, remove its targets and delete the rule. Inspect the queues, DLQ and archive. After processing or retaining demo records, delete the archive, custom bus and lab queues. Delete alarms before their SNS topic/subscriptions.

## 2. Remove ingress and compute

Remove lab FE/API aliases after checking for shared names. Disassociate WAF from the distribution/ALB and remove function associations. Disable CloudFront, wait for deployment, then delete the dedicated distribution and OAC. Delete the dedicated function/ACL after removing associations.

Set service desired count to zero, wait for tasks to stop, then delete the lab service and cluster. Delete the ALB (disable protection on this lab ALB if necessary), then its target group. Deregister unused lab task revisions. Delete ACM certificates after removing attachments.

## 3. Remove storage, permissions and network

Empty dedicated buckets including versions and delete markers if versioning was enabled, then delete the FE/log buckets. Delete lab ECR images/repository. Schedule secret deletion with a recovery window. Remove the RUM monitor, identity pool, flow log delivery, log groups and lab roles/policies once producers stop.

If the lab created NAT, delete it, wait for Deleted and release its Elastic IP. Delete dedicated endpoints and check remaining ENIs before removing security groups, subnets and custom route tables. Detach/delete the IGW, then the lab VPC. Delete a public zone only after moving or retiring DNS and removing records other than NS/SOA.

## 4. Verify after the session

Inspect both Singapore and us-east-1. Check running tasks, ALBs, NAT/public IPv4, CloudFront, logs and S3/ECR storage. After billing updates inspect Cost Explorer by service and date. Keep the budget alert if continuing to use the account.
