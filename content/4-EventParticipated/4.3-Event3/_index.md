---
title: "Event 3"
date: 2025-09-09
weight: 1
chapter: false
pre: " <b> 4.3. </b> "
---

# Event Report: AWS Cloud Mastery Series #2 : DevOps on AWS Workshop

### Purpose of the Event
- **Instill the DevOps Mindset**: To move beyond tools and establish a deep understanding of the DevOps culture, principles, and key metrics (like DORA) that drive high-performing technology teams.
- **Provide an End-to-End Technical Blueprint**: To offer a comprehensive, full-day deep dive into the AWS DevOps toolchain, covering the entire software delivery lifecycle from source code to production monitoring.
- **Master Automation Through Live Demos**: To provide practical, hands-on demonstrations of core DevOps workflows, including building a full CI/CD pipeline, deploying infrastructure as code (IaC), and managing containerized applications.
- **Build Resilient and Observable Systems**: To equip attendees with the knowledge to not only deploy applications efficiently but also to monitor, trace, and manage them effectively in a complex, distributed environment.

### List of Speakers
-   **Truong Quang Tinh** - DevOps Engineer, TymeX
-   **Van Hoang Kha** - Cloud Security Engineer, AWS Vietnam
-   **Quoc Bao** - AWS Vietnam
-   **Phuc Thinh** - AWS Vietnam
-   **Dai Vi** - AWS Vietnam
-   **Long Huynh** - AWS Vietnam
-   **Quy Pham** - AWS Vietnam
-   **Nghiem Le** - AWS Vietnam

### Key Content & Highlights

#### From Code to Cloud: The Automated CI/CD Pipeline
- **Focus**: The morning's core session was dedicated to building a fully automated software delivery pipeline from scratch.
- **Content**: The instructors provided a deep dive into the AWS CodeSuite, covering:
    - **Source**: AWS CodeCommit and best practices for Git strategies (GitFlow vs. Trunk-based).
    - **Build & Test**: Configuring automated builds and integrating testing stages with CodeBuild.
    - **Deploy**: A detailed look at CodeDeploy, comparing modern deployment strategies like Blue/Green, Canary, and Rolling updates.
    - **Orchestrate**: Tying everything together into a seamless workflow with CodePipeline.
- **Key Moment**: The live demonstration of a complete CI/CD pipeline in action was a standout. It connected all the theoretical pieces into a tangible, working system, showing how a single code commit could automatically trigger the entire build, test, and deployment process.
- **Related Session**: "AWS DevOps Services – CI/CD Pipeline".

#### Building the Foundation: Infrastructure as Code (IaC)
- **Focus**: This session shifted from application code to infrastructure, teaching attendees how to manage cloud resources programmatically.
- **Content**: The workshop explored the two primary IaC tools on AWS: the declarative power of AWS CloudFormation (templates, stacks, drift detection) and the imperative flexibility of the AWS CDK, which allows developers to use familiar programming languages.
- **Key Moment**: The side-by-side demo of deploying infrastructure with both CloudFormation and the CDK provided immense clarity. The following discussion on when to choose each tool gave attendees a practical framework for making informed architectural decisions.
- **Related Session**: "Infrastructure as Code (IaC)".

#### Running Modern Applications: The World of Containers
- **Focus**: The afternoon began with a deep dive into containerization, the backbone of modern microservices architecture.
- **Content**: Attendees were taken on a journey from Docker fundamentals to managing containers at scale on AWS. This included securing images in Amazon ECR, comparing the orchestration power of Amazon ECS and EKS, and exploring the simplicity of AWS App Runner for simple deployments.
- **Key Moment**: The case study and demo comparing different microservices deployment strategies was incredibly valuable. It illustrated the real-world trade-offs between simplicity (App Runner), managed control (ECS), and ultimate flexibility (EKS).
- **Related Session**: "Container Services on AWS".

#### Closing the Loop: Full-Stack Observability
- **Focus**: This critical session taught attendees how to see inside their applications and infrastructure once they are live.
- **Content**: The presentation went beyond basic monitoring, covering the three pillars of observability: logs and metrics with Amazon CloudWatch, and distributed tracing with AWS X-Ray. Best practices for setting up meaningful alarms, dashboards, and on-call processes were also shared.
- **Key Moment**: The full-stack observability demo was like turning on the lights in a dark room. It showed how to trace a single user request as it traveled through multiple microservices, pinpointing bottlenecks and errors with precision.
- **Related Session**: "Monitoring & Observability".

### What You Learned

#### On Automation & Pipelines
- **Build a Complete CI/CD Workflow**: You learned how to construct an automated pipeline on AWS that takes code from a Git repository and safely deploys it to production using modern strategies like Blue/Green.
- **Manage Infrastructure as Code**: You now understand how to define and manage your entire cloud infrastructure programmatically, enabling version control, peer reviews, and repeatable deployments for your environments. You also know the key differences between CloudFormation and the CDK.

#### On Modern Application Deployment
- **Master Containerization on AWS**: You gained a clear framework for choosing the right container service (ECS, EKS, or App Runner) based on your team's skills, application complexity, and operational needs.
- **Implement Advanced Deployment Strategies**: You moved beyond simple deployments and learned how to use techniques like feature flags and A/B testing to release new features to users with minimal risk and maximum feedback.

#### On System Reliability and Operations
- **Build Observable Systems**: You learned to create systems that are not just monitored, but truly observable. You can now use CloudWatch and X-Ray together to gain deep insights into application performance and quickly diagnose issues in a distributed architecture.
- **Adopt DevOps Best Practices**: You gained insights into proven practices for incident management, writing effective postmortems, and fostering a culture of continuous improvement within your team.

### Experience at the Event
The full-day "DevOps on AWS" workshop was an intensive and incredibly rewarding masterclass. From the moment it began, there was a clear sense that this was not just a series of lectures, but a cohesive journey through the entire lifecycle of a modern application.

#### The Morning: Building the Blueprint
The morning sessions felt like being handed the architectural blueprints for building a world-class software factory. The CI/CD pipeline demo wasn't just a technical walkthrough; it was a moment of clarity where the abstract concept of automation became a tangible, powerful reality. The deep dive into IaC felt equally empowering, as if we were being given the tools to sculpt the cloud with code, ensuring consistency and eliminating manual errors forever.

#### The Afternoon: Navigating Real-World Complexity
After lunch, the workshop shifted into the complex realities of running modern applications. The session on containers was a masterclass in navigating choices, clarifying the often-confusing landscape of ECS, EKS, and App Runner. The observability session was perhaps the most eye-opening part of the day. The demo showing how to trace a single request across a dozen services was a powerful illustration of how to find a needle in a haystack, transforming the daunting task of debugging into a systematic process.
