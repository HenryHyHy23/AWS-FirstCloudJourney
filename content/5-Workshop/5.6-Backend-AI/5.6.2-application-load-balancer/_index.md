---
title: "Route HTTPS from the ALB to Spring Boot"
menuTitle: "Application Load Balancer"
weight: 2
chapter: false
pre: "<b> 5.6.2. </b> "
---

## Create the Fargate target group

1. **Open EC2 → Load Balancing → Target Groups.**

{{< btbfigure file="aws-application-load-balancer-step-1.webp" caption="Open EC2 → Load Balancing → Target Groups." step="yes" >}}

2. **Choose Create target group → IP addresses.** Enter `btb-backend-tg`. For awsvpc tasks ECS registers the task's private IP.

{{< btbfigure file="aws-application-load-balancer-step-2-create.webp" caption="Create an IP target group." step="yes" >}}

3. **Select the protocol, port and VPC.** Set HTTP, port 8080, IPv4 and the VPC hosting the tasks.

{{< btbfigure file="aws-application-load-balancer-step-2.webp" caption="Select the protocol, port and VPC." step="yes" >}}

4. **Open Health checks.** Select HTTP, traffic port and path `/api/health`. The endpoint must return 200 to the health checker.

{{< btbfigure file="aws-application-load-balancer-step-22.webp" caption="BTB health check path." step="yes" >}}

5. **Open Advanced health check settings → Next.** Compare the pictured settings.
   - Unhealthy threshold — 2
   - Timeout — 5 seconds
   - Interval — 30 seconds
   - Success codes — 200

{{< btbfigure file="aws-application-load-balancer-step-23.webp" caption="Timeout, interval and success code." step="yes" >}}

6. **Open Register targets.** The form illustrates selecting a network and entering an IP. For the ECS service in 5.6.3 let ECS register its task IP automatically.

{{< btbfigure file="aws-application-load-balancer-step-3-register-target.webp" caption="Open Register targets." step="yes" >}}

7. **Check the target port and finish creating the group.** The pictured target uses port 8080. Record the group name for the listener and service.

{{< btbfigure file="aws-application-load-balancer-step-3-register-target-1.webp" caption="Target IP and backend port." step="yes" >}}

## Create the HTTPS listener and review attributes

8. **Open Load balancers → Create → Application Load Balancer.** Select internet-facing, the lab VPC, two public subnets in different AZs and ALB-SG. Create HTTPS:443, select the Singapore certificate from 5.7.1 and forward to btb-backend-tg.

9. **Inspect the HTTP:80 listener if present.** The capture shows the old listener's deletion dialog. Keep HTTPS:443. HTTP:80 can redirect to HTTPS; remove an unused lab listener only after checking its replacement.

{{< btbfigure file="aws-application-load-balancer-step-5-delete-listener.webp" caption="Inspect the HTTP:80 listener if present." step="yes" >}}

10. **Open ALB → Attributes → Edit.** Inspect connection idle timeout and X-Forwarded-For processing. Adjust timeouts according to measured application behavior.

{{< btbfigure file="aws-application-load-balancer-step-6-edit-attributes.webp" caption="Open ALB → Attributes → Edit." step="yes" >}}

11. **Under Logging choose a destination if required.** For S3 access logs use a same-Region log bucket with ELB delivery permission. Review and save changes.

{{< btbfigure file="aws-application-load-balancer-step-6-edit-attributes-1.webp" caption="ALB attributes including deletion protection and access logs." step="yes" >}}

**Verify.** After the ECS lab open Targets → Health details. For timeouts inspect SGs and ports. For ResponseCodeMismatch check path and status. Compare Java logs in CloudWatch against ALB and target status in the access log.
