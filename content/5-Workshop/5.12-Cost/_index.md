---
title: "Cost of a sample BTB environment"
menuTitle: "Cost Estimation"
weight: 12
chapter: false
pre: "<b> 5.12. </b> "
---


The task-definition capture shows Linux/x86, 0.5 vCPU and 2 GB. This Singapore example assumes one task and one ALB for 730 hours per month, averaging one LCU. The USD list prices were retrieved on 25 November 2026 and exclude tax, credits and Free Tier. Task count, hours and LCU are exercise assumptions.

| Charge | Quantity × rate | USD/month |
|---|---|---:|
| Fargate vCPU | 0.5 × 730 × 0.05056 | 18.4544 |
| Fargate RAM | 2 × 730 × 0.00553 | 8.0738 |
| ALB hours | 1 × 730 × 0.0252 | 18.3960 |
| ALB LCU | 1 × 730 × 0.008 | 5.8400 |

**Fargate + ALB subtotal: USD 50.76/month.** With the same assumptions, a four-hour lab costs about USD 0.28 for these items. The complete bill also depends on the services below. The USD 200 in lab 5.4.1 is an alert budget.

## Complete the estimate for the demo

1. Create an AWS Pricing Calculator estimate, select Singapore for compute and use LCU usage measured in CloudWatch. Multiply CPU/RAM by total task-hours. Include overlapping task revisions during rollout.
2. Add ECR (image GB), S3 (GB, requests, versions), CloudFront (viewer-region transfer and requests), Route 53 (zones/queries), WAF (ACLs, rules, requests), Secrets Manager (secrets and calls). Check the ACM certificate type.
3. Add public IPv4, hourly and per-GB NAT or per-AZ interface endpoints, and data transfer as applicable. For CloudWatch include log ingestion/retention, alarms and RUM events. Add SNS delivery, EventBridge custom events/archives, SQS requests/DLQ and tokens for the actual Bedrock model.
4. Compare Calculator totals with Cost Explorer for the same period, account and Region. Keep external provider costs separate. Save the estimate link/export with its date and assumptions after entering real usage.

**Tune from measurements.** Remove lab resources after the exercise, measure CPU and memory before resizing tasks, reduce RUM sampling after testing and set log/archive retention. Check the egress needed by Supabase and SMTP before choosing NAT or endpoints.

[Amazon ECS price list · Singapore](https://pricing.us-east-1.amazonaws.com/offers/v1.0/aws/AmazonECS/current/ap-southeast-1/index.json) · [ELB price list · Singapore](https://pricing.us-east-1.amazonaws.com/offers/v1.0/aws/AWSELB/current/ap-southeast-1/index.json) · [AWS Pricing Calculator](https://calculator.aws/)
