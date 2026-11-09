---
title: "Week 6 Worklog"
date: 2026-10-19
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:

- Understand VPC connectivity through peering and Transit Gateway.
- Check bidirectional routes, DNS, security groups/NACLs, and costs.
- Understand Docker deployment on EC2, RDS connectivity, and image storage in ECR/Docker Hub.
- Understand ECS Fargate tasks/services, ALB/Cloud Map connectivity, and service scaling.
- Understand tag-triggered ECS releases and metric/log monitoring with Container Insights and FireLens.
- Verify releases, diagnose pipeline failures, and check ECS-to-S3 log delivery.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Sources |
| --- | --- | --- | --- | --- |
| 1 | **Connecting two VPCs with peering** <br> - Review private-IP connectivity between EC2 instances in separate VPCs. Peering privately connects two VPCs with nonoverlapping CIDRs; it is non-transitive. <br> - Both sides need routes to peer CIDRs, SG allowances, and NACL return-traffic rules. Enable DNS support/hostnames and peering DNS to resolve peer EC2 public hostnames to private IPs; private hosted zones need separate associations. Test private IPs, then DNS. | 19/10/2026 | 19/10/2026 | - [Module 3 - 16: Setting up VPC Peering](https://000019.awsfcaj.com/) |
| 2 | **Connecting multiple VPCs through Transit Gateway** <br> - Compare four-VPC peering with the Transit Gateway hub model. Four VPCs need six peerings or four TGW attachments; each VPC attachment selects one subnet per enabled AZ. <br> - Association selects the ingress TGW table; propagation adds CIDRs to tables. VPC subnet routes still need configuration. Check forward/return paths, SGs/NACLs, and segmentation. Internet access via `0.0.0.0/0 → TGW` also needs an egress VPC, NAT/IGW, and routes. | 20/10/2026 | 20/10/2026 | - [Module 3 - 17: Set up AWS Transit Gateway](https://000020.awsfcaj.com/) |
| 3 | **Deploy Application on Docker** <br> - Review moving the local application to EC2 containers by building frontend/backend images and running them. Images package code and dependencies to keep deployment environments consistent. <br> - Nginx receives requests and proxies to the backend, which connects to private RDS. The database SG allows MySQL 3306 from the EC2 SG; `3000:80` maps EC2 port 3000 to container port 80. <br> - Use Compose to manage services, networks, and configuration in YAML. Then log in, tag, and push images to ECR/Docker Hub, using an EC2 IAM role for ECR. Registries store images; EC2 runs the application. | 21/10/2026 | 21/10/2026 | - [Module 3 - 18: Deploy Application on Docker](https://000015.awsfcaj.com/) |
| 4 | **Deploying applications on Amazon ECS** <br> - Continue the Docker lab by reviewing frontend/backend deployment on ECS Fargate. Task definitions specify images, CPU/memory, ports, and configuration; services maintain the desired task count and replace failed tasks. <br> - Tasks run in private subnets; the lab uses NAT for image pulls/log delivery. A public ALB uses `ip` targets and routes `/api/*` to the backend. Cloud Map provides `backend.FCAJResbar.internal` so Nginx uses DNS rather than hardcoded task IPs. <br> - Review rolling updates and CPU target tracking, distinguishing task scaling from EC2 cluster scaling. Check health checks, logs, and login/order flows to assess application behaviour beyond a Running status. | 22/10/2026 | 22/10/2026 | - [Module 3 - 19: Deploy applications on Amazon Elastic Container Service](https://000016.awsfcaj.com/) |
| 5 | **CI/CD and monitoring for ECS applications** <br> - Review GitLab, GitHub Actions, and CodeBuild pipelines. Release tags trigger frontend/backend builds → image pushes to ECR → new task definition revisions → ECS service updates. <br> - GitLab uses runners and GitHub uses reusable workflows; CodeBuild executes buildspecs and filters webhooks with `^refs/tags/`. Version images to trace deployed releases back to code. <br> - Check build logs, images, task revisions, and the interface through the ALB to verify releases. Container Insights tracks CPU, memory, and network; a Succeeded job alone does not establish application health. <br> - FireLens uses a Fluent Bit sidecar to send stdout/stderr to S3. The task role permits bucket writes; the execution role supports image pulls/CloudWatch Logs. Verify delivered logs after buffering. | 23/10/2026 | 23/10/2026 | - [Module 3 - 20: Deploying CI/CD with ECS Container](https://000017.awsfcaj.com/) |
| 6 | **ECS CI/CD: release verification and troubleshooting** <br> - Continue the CI/CD lab with separate frontend/backend CodeBuild projects and buildspecs. A `PUSH` webhook with `HEAD_REF: ^refs/tags/` triggers release-tag builds; check buildspecs, environment variables, and IAM permissions when jobs fail. <br> - Trace image tag `v1.0.2` through task revisions, ECS status, target health, and the interface through the ALB. Distinguish a successful build from a working application; inspect the relevant job/service logs to locate failures. <br> - Compare task-family metrics in Container Insights and trace frontend logs → Fluent Bit → S3. Logs are buffered, so missing objects call for checks of destination settings, task-role permissions, and network connectivity. | 24/10/2026 | 24/10/2026 | - [Module 3 - 20 (continued): Deploying CI/CD with ECS Container](https://000017.awsfcaj.com/) |

### Week 6 Achievements:

- Distinguish Active connection status from reachability through routes, firewalls, and DNS.
- TGW reduces connection management but charges for attachments/data processing. Cross-AZ/Region peering can incur transfer charges.
- Distinguish images, containers, and registries; understand port mapping, RDS connectivity, and multi-container management with Compose.
- Distinguish task definitions, tasks, and services; understand ALB/Cloud Map connectivity and Fargate-managed container infrastructure.
- Trace releases through Git tags, image tags, and task revisions; distinguish metrics from logs and task roles from execution roles.
- Locate failures in triggers/builds, ECS deployment, or log delivery; check the running release and stored logs before declaring completion.
