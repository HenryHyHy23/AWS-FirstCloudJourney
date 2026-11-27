---
title: "Run the backend on ECS Fargate"
menuTitle: "Amazon ECS with AWS Fargate"
weight: 3
chapter: false
pre: "<b> 5.6.3. </b> "
---

1. **Open ECS → Clusters → Create cluster.** Name the cluster and choose Fargate. Prepare the ECR URI, two IAM roles, secret, target group and log group before creating the service.

2. **Open Task definitions → Create new task definition.** Enter a family name and select AWS Fargate.

{{< btbfigure file="aws-application-load-balancer-step-4-create-task-definition.webp" caption="Family and Fargate launch requirements." step="yes" >}}

3. **Choose the environment and both IAM roles.** Compare the fields in the capture.
   - OS/architecture — Linux/X86_64; network mode — awsvpc
   - Task size — 0.5 vCPU, 2 GB
   - Task role — backend role from 5.4.2; Task execution role — ECR, secrets and logging role

{{< btbfigure file="aws-application-load-balancer-step-4-create-task-definition-1.webp" caption="CPU, RAM and the two roles in one revision." step="yes" >}}

4. **Add the backend container.** Enter `btb-backend`, select the image you pushed, enable Essential and map TCP 8080. Spring Boot must listen on the container interface.

{{< btbfigure file="aws-application-load-balancer-step-4-create-task-definition-2.webp" caption="Container port mapping." step="yes" >}}

5. **Review the container resource allocation limits.** Keep CPU and memory limits compatible with the task size and allow for JVM non-heap memory.

{{< btbfigure file="aws-application-load-balancer-step-4-create-task-definition-3.webp" caption="Review the container resource allocation limits." step="yes" >}}

6. **Open Environment variables → Add environment variable.** For Cloudinary, SePay, SMTP and database credentials select ValueFrom and reference the ARN from the Secrets Manager lab.

{{< btbfigure file="aws-application-load-balancer-step-4-create-task-definition-4.webp" caption="Cloudinary, SMTP, SePay and database keys in the task." step="yes" >}}

7. **Complete the Supabase and Vietmap fields.** Verify the names read by Spring. Use Value for ordinary settings such as JAVA_TOOL_OPTIONS. Under Logging select awslogs, the Region, log group and ecs stream prefix. Register the revision.

{{< btbfigure file="aws-application-load-balancer-step-4-create-task-definition-5.webp" caption="Supabase and Vietmap variables in the task." step="yes" >}}

8. **Select Cluster → Services → Create.** Choose the registered family:revision.
   - Compute — Fargate; desired tasks — 1 for the lab
   - Networking — private subnets with egress, TASK-SG, public IP off
   - Load balancing — lab ALB, btb-backend:8080 and IP target group; allow enough startup grace for Spring

**Verify.** Wait for Running to equal Desired, verify a Healthy target and fetch GET /api/health over HTTPS. For a failed rollout inspect ECS Events and the new task's stopped reason.
