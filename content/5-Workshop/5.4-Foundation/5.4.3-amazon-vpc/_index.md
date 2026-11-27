---
title: "Network for the ALB and Fargate tasks"
menuTitle: "Amazon VPC"
weight: 3
chapter: false
pre: "<b> 5.4.3. </b> "
---

Prepare two Singapore AZs for the ALB and a network path to Fargate tasks on port 8080.

## VPC and subnets

1. **Search for VPC in the AWS Console.**

{{< btbfigure file="aws-vpc-step-1.webp" caption="Search for VPC in the AWS Console." step="yes" >}}

2. **Choose Your VPCs → Create VPC.**

{{< btbfigure file="aws-vpc-step-2-create-vpc.webp" caption="Choose Your VPCs → Create VPC." step="yes" >}}

3. **Choose VPC only.** Enter the lab name and IPv4 CIDR `10.0.0.0/16`, then choose Create VPC.

{{< btbfigure file="aws-vpc-step-3-setup-vpc.webp" caption="VPC creation form." step="yes" >}}

4. **Select the VPC → Actions → Edit VPC settings.** Enable DNS resolution and DNS hostnames and save.

{{< btbfigure file="aws-vpc-step-4-enable-dns.webp" caption="DNS options in VPC settings." step="yes" >}}

5. **Open Subnets → Create subnet.** Select the VPC you created and choose an Availability Zone for the first subnet.

{{< btbfigure file="aws-vpc-step-5-create-subnet.webp" caption="Open Subnets → Create subnet." step="yes" >}}

6. **Enter each subnet name and CIDR.** The capture shows private A. Add the remaining subnets to the same VPC. Example lab ranges:
   - Public A/B — `10.0.0.0/24`, `10.0.1.0/24` in two AZs
   - Private A/B — `10.0.128.0/20`, `10.0.144.0/20` in two AZs

{{< btbfigure file="aws-vpc-step-5-setup-subnet.webp" caption="First private subnet and its AZ." step="yes" >}}

## Internet gateway and route tables

7. **Open Internet gateways → Create internet gateway.**

{{< btbfigure file="aws-vpc-step-6-create-internet-gateways.webp" caption="Open Internet gateways → Create internet gateway." step="yes" >}}

8. **Enter its Name tag → Create internet gateway.**

{{< btbfigure file="aws-vpc-step-6-name-internet-gateways.webp" caption="Enter its Name tag → Create internet gateway." step="yes" >}}

9. **Choose Actions → Attach to VPC.** Select your VPC and attach the gateway.

{{< btbfigure file="aws-vpc-step-7-attach.webp" caption="Attach the gateway to the lab VPC." step="yes" >}}

10. **Open Route tables → Create route table.** Name the public table, select the lab VPC and create it.

{{< btbfigure file="aws-vpc-step-8-create-route-table.webp" caption="Open Route tables → Create route table." step="yes" >}}

11. **Choose Routes → Edit routes → Add route.** Set destination `0.0.0.0/0` and select your internet gateway. Save changes, then associate both public subnets.

{{< btbfigure file="aws-vpc-step-9-edit-routes.webp" caption="Public default route points to the internet gateway." step="yes" >}}

12. **Provide egress for private tasks.** For the NAT option create a public NAT gateway with an Elastic IP in a public subnet, wait for Available, then route the **private table's** default route to NAT. If using endpoints, also check connectivity to Supabase and SMTP. NAT incurs hourly and data charges.

13. **Open Security groups → Create security group.** Create both groups in the VPC.
    - ALB-SG — inbound HTTPS 443 from intended sources, outbound TCP 8080 to TASK-SG
    - TASK-SG — inbound TCP 8080 from ALB-SG; allow egress to backend dependencies

{{< btbfigure file="aws-vpc-step-10-create-security-groups.webp" caption="Security group creation in the VPC." step="yes" >}}

**Verify.** Select each subnet → Route table and verify its routes and association. In the ECS lab the target should become Healthy when the ALB checks port 8080.
