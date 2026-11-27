---
title: "Trace a GitHub commit to the BTB release"
menuTitle: "CI/CD Deployment"
weight: 10
chapter: false
pre: "<b> 5.10. </b> "
---



The project guide describes FE and BE deployment from feat/prod. Inspect the application's .github/workflows and Actions runs for the actual trigger, environment and run ID. The gh-pages workflow in the Hugo repository publishes the workshop.

## 1. Authenticate the runner to AWS

For GitHub OIDC configure token.actions.githubusercontent.com in IAM with audience sts.amazonaws.com. Restrict the trust policy sub to the repository/branch or environment used by the job. The job needs id-token: write and contents: read. Scope frontend deployment to the S3 prefix and lab distribution invalidation. Backend deployment needs ECR push, task revision registration, service update and PassRole for the two ECS roles.

## 2. Trace the frontend release

Compare the frontend run SHA with checkout, npm ci and build. Upload hashed assets before updating index.html. Invalidate /index.html and the root path as needed. In View Source or Network compare the filenames referenced by the new HTML. Roll back using an intact previous build and restore its HTML.

## 3. Trace the backend release

Trace the backend run SHA/tag → ECR digest → task revision with the matching image → ECS service deployment. Wait for service stability and Healthy targets before recording success. If the service returns to an older revision, inspect ECS Events and stopped reason to locate an image pull, startup or health check failure.


```powershell
$region = "ap-southeast-1"
aws ecs describe-services --cluster LAB_CLUSTER --services LAB_SERVICE --region $region --query "services[].{running:runningCount,desired:desiredCount,revision:taskDefinition}"
aws elbv2 describe-target-health --target-group-arn LAB_TARGET_GROUP_ARN --region $region --query "TargetHealthDescriptions[].{port:Target.Port,state:TargetHealth.State,reason:TargetHealth.Reason}"
```


For one deployment record the Actions run URL, commit SHA, digest, revision and target health time. The supplied captures do not include an Actions run.
