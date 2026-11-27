---
title: "Backend & AI Deployment"
menuTitle: "Backend & AI Deployment"
weight: 6
chapter: false
pre: "<b> 5.6. </b> "
---



Prepare the secret, log group in 5.8.1 and ALB certificate in 5.7.1. Verify a stable task and a Healthy target.

- [5.6.1. AWS Secrets Manager](5.6.1-aws-secrets-manager/) — Protects backend connection parameters, AI configuration, webhook credentials and mail secrets.
- [5.6.2. Application Load Balancer](5.6.2-application-load-balancer/) — HTTPS API entry point forwarding requests to healthy ECS targets.
- [5.6.3. Amazon ECS with AWS Fargate](5.6.3-amazon-ecs-fargate/) — Runs the modular-monolith Spring Boot API as a managed container workload.
- [5.6.4. Amazon Bedrock](5.6.4-amazon-bedrock/) — AI model inference for BTB backend actions/Generate UI where connected; business authorization stays in Spring Boot.
