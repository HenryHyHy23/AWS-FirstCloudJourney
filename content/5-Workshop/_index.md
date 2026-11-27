---
title: "Bite to Business on AWS"
menuTitle: "Workshop"
weight: 5
chapter: false
pre: "<b> 5. </b> "
---



Deploy the Bite to Business React storefront and Spring Boot API, from the first VPC to a purchase request passing through CloudFront, ALB and ECS. Then trace errors, cost and order events.

## How to run the workshop

Use a lab account for resource creation. The captured names help identify each step. Substitute your own domain, account ID, ARNs, UUIDs and credentials. Select an image to inspect it at full size. Each step lists the action and fields to complete. The first policy example includes its screenshot and code. Expand the reference captures to inspect additional policy screens.

**Evidence scope.** The team note reports deployed FE/BE services. The captures add an EventBridge bus, SQS targets, WAF rules at different points and Bedrock quota settings. Production AI results, RUM ingestion and event consumption require the tests in 5.11.

## Hands-on labs

- [5.1. Overview](5.1-overview/)
- [5.2. Prerequisites](5.2-prerequisites/)
- [5.3. Architecture & Data Flow](5.3-architecture/)
- [5.4. Network & Foundation](5.4-foundation/)
- [5.5. Container & Storage](5.5-containers-storage/)
- [5.6. Backend & AI Deployment](5.6-backend-ai/)
- [5.7. Web Delivery & Security](5.7-web-security/)
- [5.8. Monitoring, alerts and events](5.8-monitoring-alerts/)
- [5.9. External Integrations](5.9-external-integrations/)
- [5.10. CI/CD Deployment](5.10-ci-cd/)
- [5.11. Testing & Live Demo](5.11-testing/)
- [5.12. Cost Estimation](5.12-cost/)
- [5.13. Cleanup & Troubleshooting](5.13-cleanup/)
