---
title: "Monitoring, alerts and events"
menuTitle: "Monitoring, alerts and events"
weight: 8
chapter: false
pre: "<b> 5.8. </b> "
---



Exercise three paths: backend logs and metrics, browser telemetry, and order events sent to a queue. Create SNS before the alarm and SQS before attaching the EventBridge target.

- [5.8.1. Amazon CloudWatch Logs](5.8.1-cloudwatch-logs/) — Centralized Spring Boot stdout/stderr logs for task startup, API issues and integrations.
- [5.8.2. CloudWatch Metrics & Alarms](5.8.2-cloudwatch-metrics-alarms/) — Track ECS CPU/memory, ALB errors/latency/healthy targets and trigger alert actions.
- [5.8.3. Amazon SNS](5.8.3-amazon-sns/) — Sends alarm notifications to verified email subscriptions.
- [5.8.4. CloudWatch RUM](5.8.4-cloudwatch-rum/) — Browser-side monitoring for React performance and JavaScript errors.
- [5.8.5. Amazon Cognito Identity Pools](5.8.5-cognito-identity-pools/) — Issues AWS credentials for the browser RUM client as configured.
- [5.8.6. Amazon EventBridge](5.8.6-amazon-eventbridge/)
- [5.8.7. Amazon SQS & DLQ](5.8.7-amazon-sqs/)
