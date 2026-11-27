---
title: "Alert when the ALB returns 5xx"
menuTitle: "CloudWatch Metrics & Alarms"
weight: 2
chapter: false
pre: "<b> 5.8.2. </b> "
---

Create the SNS topic and confirm the email subscription in 5.8.3 before attaching the alarm action.

1. **Open CloudWatch → Alarms → All alarms → Create alarm → Select metric.** Select CloudWatch metrics as the data source.

{{< btbfigure file="aws-cloudwatch-metrics--alarms-step-1-create-alarm.webp" caption="Open CloudWatch → Alarms → All alarms → Create alarm → Select metric." step="yes" >}}

2. **Choose ApplicationELB → Per AppELB Metrics.** Find your lab load balancer and select HTTPCode_ELB_5XX_Count. Use HTTPCode_Target_5XX_Count to monitor errors returned by the application.

{{< btbfigure file="aws-cloudwatch-metrics--alarms-step-2-select-metric.webp" caption="Select the LoadBalancer dimension." step="yes" >}}

3. **Configure the alarm condition → Next.** Example for a low-traffic lab:
   - Statistic — Sum; period — 5 minutes
   - Threshold type — Static; Greater than — enter 0
   - Datapoints — 1 of 1; missing data — not breaching

{{< btbfigure file="aws-cloudwatch-metrics--alarms-step-3-setup-metric.webp" caption="Alarm metric, statistic, period and condition." step="yes" >}}

4. **Under Configure actions choose In alarm → Select an existing SNS topic.** Select btb-system-alerts and choose Next.

{{< btbfigure file="aws-cloudwatch-metrics--alarms-step-4-configure.webp" caption="Attach SNS to the In alarm state." step="yes" >}}

5. **Enter the alarm name → Review → Create alarm.** Use `btb-backend-alb-5xx` as pictured.

{{< btbfigure file="aws-cloudwatch-metrics--alarms-step-5-name.webp" caption="Backend ALB alarm name." step="yes" >}}

**Verify.** Open the alarm → History. When a transition occurs, compare the action timestamp with its SNS email. Publishing a test in SNS checks the SNS-to-email part of the path.
