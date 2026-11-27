---
title: "Inspect application logs and network flow logs"
menuTitle: "Amazon CloudWatch Logs"
weight: 1
chapter: false
pre: "<b> 5.8.1. </b> "
---

1. **Open CloudWatch Console → Logs → Log management → Create log group.** Configure the following fields.
   - Log group name — `/aws/vpc/flowlogs` as shown
   - Log class — Standard
   - Retention — the example shows Never expire; for a lab choose a period such as 7 days and create the group

{{< btbfigure file="aws-cloudwatch-step-1.webp" caption="The pictured log group is /aws/vpc/flowlogs." step="yes" >}}

2. **Open VPC → Your VPCs → select the VPC → Flow logs → Create flow log.** Choose All or Reject, destination CloudWatch Logs, the new log group and a delivery role authorized to write. Create the flow log.

3. **Create the backend log group.** Return to CloudWatch → Create log group → enter `/ecs/btb-backend`. In the ECS task definition select awslogs, this group, Singapore Region and prefix ecs. Deploy the new revision.

4. **Open the log stream for the task ID.** Note the time you call /api/health and compare it with application logs, or read the group with the following command.


```powershell
aws logs tail /ecs/btb-backend --since 10m --region ap-southeast-1
```


**Verify.** Check for Spring startup logs in the ECS group and flow records for matching traffic in the VPC group. To find ALB HTTP status inspect the S3 access logs from 5.6.2.
