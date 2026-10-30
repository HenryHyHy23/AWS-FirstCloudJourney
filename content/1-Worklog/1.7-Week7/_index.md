---
title: "Week 7 Worklog"
date: 2026-10-26
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

- Understand CodePipeline orchestration of source, build/test, and Node.js deployment to EC2.
- Distinguish buildspecs, AppSpec files, artifacts, and IAM roles during deployment.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Sources |
| --- | --- | --- | --- | --- |
| 1 | **Deploying to EC2 with AWS CodePipeline** <br> - Review Node.js deployment to EC2: CodeCommit commits trigger CodePipeline through EventBridge, followed by build/test and deployment stages. <br> - CodeBuild runs commands from `buildspec.yml` and produces artifacts stored in S3. CodeDeploy uses `appspec.yml` to define installed files and lifecycle scripts such as BeforeInstall and ApplicationStart. <br> - EC2 needs a running CodeDeploy Agent, an instance profile that can read artifacts, and tags matching the deployment group. Distinguish service-role permissions from EC2 permissions; check KMS access for KMS-encrypted artifacts. <br> - Inspect stage logs and lifecycle events to distinguish source, build, artifact-download, and application-start failures. Verify the application over HTTP after deployment; a successful build alone does not establish a working release. | 26/10/2026 | 26/10/2026 | - [Module 3 - 21: Deploy applications to EC2 with AWS CodePipeline](https://000023.awsfcaj.com/) <br> - [AWS: CodePipeline concepts](https://docs.aws.amazon.com/codepipeline/latest/userguide/concepts.html) <br> - [AWS: CodeCommit EventBridge trigger](https://docs.aws.amazon.com/codepipeline/latest/userguide/pipelines-trigger-source-repo-changes-cli.html) <br> - [AWS: CodeDeploy AppSpec](https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file.html) <br> - [AWS: EC2 deployment prerequisites](https://docs.aws.amazon.com/codedeploy/latest/userguide/instances-ec2-configure.html) |

### Week 7 Achievements:

- Distinguish the responsibilities of CodeCommit, CodeBuild, CodeDeploy, and CodePipeline.
- Understand artifact, agent, permission, and lifecycle-hook checks for troubleshooting, followed by application validation after deployment.
