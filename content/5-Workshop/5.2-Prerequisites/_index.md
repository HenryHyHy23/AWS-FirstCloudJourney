---
title: "Prepare the lab environment"
menuTitle: "Prerequisites"
weight: 2
chapter: false
pre: "<b> 5.2. </b> "
---



Use an AWS lab account, clone the team's React and Spring Boot repositories and select the revision for the exercise. Prepare deployment permissions, a test mailbox and DNS access if exercising custom domains. Install Docker, Git, Node/npm, AWS CLI, PowerShell and the JDK required by the backend build.


```powershell
git --version
docker version
aws --version
aws sts get-caller-identity --query Arn --output text
node --version
npm --version
```


## Values to determine before creating resources

**Region.** The backend, Bedrock quota and EventBridge captures use Singapore. The CloudFront certificate is in N. Virginia. For frontend S3 use the Region selected at bucket creation.

**Names and IDs.** The captures show generate-ui-be (ECR), btb-backend (secret), btb-backend-tg (target group), btb-events (bus), btb-customer-events (queue) and btb-system-alerts (SNS). In a shared environment use a lab suffix and update ARNs in all examples.

## Execution order for a fresh account

1. Budget → IAM → VPC and private-task egress. Complete IAM scoping after the secret, bus and queue ARNs exist.

2. ECR/image and S3/build → hosted zone/delegation → two ACM certificates → secret and log group → target group, ALB → ECS service.

3. Create the api alias to ALB → CloudFront/OAC and both behaviors → frontend alias → WAF. Check frontend, deep link and API health.

4. SNS → alarm; RUM → Cognito → rebuild FE; SQS → EventBridge → test message. Finish demo, cost estimate and cleanup in 5.11–5.13.
