---
title: "Bite to Business architecture"
menuTitle: "Architecture & Data Flow"
weight: 3
chapter: false
pre: "<b> 5.3. </b> "
---

Start at **User** on the left. Follow arrow **1** to see how the browser resolves the domain, then arrow **2** for the HTTPS connection to CloudFront. Select the diagram to read its labels at full resolution.

{{< btbfigure file="architecture-btb.png" caption="The Bite to Business team’s AWS architecture. Numbers 1 through 28 identify the flows described below." >}}

| Diagram numbers | Flow | What to trace in the workshop |
|---|---|---|
| 1–3 | Open the storefront | Mắt Bão delegates DNS to Route 53. The browser resolves the domain (1), connects to CloudFront over HTTPS (2), and receives React assets from the S3 origin (3). Check the CloudFront ACM certificate in `us-east-1`. |
| 4–6 | Call the API | Follow the API branch from CloudFront through the internet gateway to the Application Load Balancer (ALB) and on to an ECS Fargate task. In the ALB lab, open the target group to check task health. The diagram shows WAF protection for both CloudFront and the ALB. |
| 7–13 | Use backend features | Trace the branches from ECS to Supabase Auth (7), VietMap (8), Cloudinary (9), Bedrock Nova Pro with Guardrails (10), and SePay (11). For an OTP, the backend uses Gmail SMTP (12) to deliver the message to the user (13). |
| 14–18 | Send event-driven email | ECS publishes an event to EventBridge (14). A rule routes it to SQS (15). Lambda processes the message (16) and calls SES to deliver the email (17–18). In the lab, match the rule target to the queue Lambda reads. |
| 19–23 | Monitor the application | Find the ECS application logs in CloudWatch (19). Trace an alarm through SNS to the operations inbox (20–21). Use Athena to query ALB access logs in S3 (22). CloudWatch RUM records performance and errors in the browser (23). |
| 24–28 | Deploy and start the backend | Follow a developer’s change through GitHub and GitHub Actions. The workflow uses OIDC to obtain AWS credentials and pushes the image to ECR (24–26). ECS pulls the image to run a task (27) and retrieves secrets from Secrets Manager (28). |
