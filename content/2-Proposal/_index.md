---
title: "Proposal"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Bite to Business

### 1. Executive Summary

Our team is building **Bite to Business** as part of the AWS First Cloud Journey internship program: a food e-commerce website for business customers (restaurants, hotels, catering) and individuals. It supports the flow of browsing products → creating a quote → customer confirmation → QR payment through SePay → delivery.

Its key feature is **Generative UI**: the interface adapts to each customer's intent and habits. The backend calls **Amazon Bedrock (Amazon Nova Pro)**, uses **Bedrock Guardrails** to check content, and generates **A2UI** interfaces from the team's component catalog. The application uses React, Spring Boot, and Supabase PostgreSQL, runs on **AWS** in Singapore (`ap-southeast-1`), and is deployed automatically with GitHub Actions.

### 2. Problem Statement

#### What is the problem?

B2B buyers have different tasks: finding ingredients, comparing quantity-based prices, reordering familiar products, or splitting items across delivery addresses.

- **A fixed interface** forces buyers to search, filter, and switch pages repeatedly.
- **AI-generated interfaces** must show correct data, respect permissions, and keep context when the customer changes the request.
- **Slow or failed AI responses** must not interrupt purchasing.

#### The Solution

- **Personalized interface:** The AI picks layouts from the component catalog, such as a table for bulk purchases or a reorder list for returning customers. A habit profile is built from customer interactions.
- **Two-layer validation:** The backend validates A2UI messages before sending them; the frontend only renders components in the catalog.
- **Backend-controlled transactions:** Prices, promotions, VAT, inventory, and permissions are always checked by the backend. After confirmation, stock is held for up to 6 hours, and payment is updated only after a valid SePay webhook.
- **Fallback when AI fails:** Return to the base UI and keep drafts so customers can continue.

#### Business Functions by Role

| Role | Main functions |
| --- | --- |
| **Customer** | Register (approved by Admin), create quotes, use coupons, split packages across delivery addresses, confirm and pay by QR, track orders, reorder, request new products, return packages within 24 hours, request refunds, review products, use the AI assistant |
| **E-commerce Executive** | Manage products, categories, brands, prices, and VAT; create promotions and coupons; monitor quotes; moderate reviews; handle refunds; view the business dashboard |
| **Logistics Staff** | Receive stock, adjust inventory, manage warehouse products and distributors, respond to customers' new product requests |
| **Delivery Staff** | Set availability, claim packages to deliver, view delivery routes on the map, confirm delivery |
| **Admin** | Approve customer registrations, create staff accounts, resolve SePay transactions that need reconciliation, handle package returns, monitor the system |

#### Benefits and Return on Investment

- **Faster purchasing:** Customers say what they need, and the interface arranges the right information instead of making them search and filter.
- **Correct data:** Prices, inventory, and order status come from the backend; the AI never invents numbers.
- **One connected workflow:** Quotes, payment, warehouse, and delivery run on one system for 5 roles.
- **Controlled cost:** About 85 USD per month for infrastructure at MVP scale, with AI call limits and budget alerts.
- **Safe operations:** WAF, Secrets Manager, CI/CD without long-lived access keys, and email alerts when something goes wrong.
- **Scalability:** ECS scales between 1 and 3 tasks; event-driven notifications make new notification types easy to add.

### 3. Solution Architecture

![AWS architecture diagram for Bite to Business](/images/2-Proposal/BTB-AWS-Architecture-Page-1.drawio.png)

**Request flow:** User → CloudFront (+ WAF) → S3 (web) or `/api/*` → ALB (+ WAF) → ECS Fargate. Only the SePay webhook goes directly to the ALB.

#### AWS Services Used

- **Amazon Route 53:** Manages the domain.
- **AWS Certificate Manager:** Issues HTTPS certificates.
- **Amazon CloudFront:** Single entry point for the web and API; delivers the frontend.
- **AWS WAF:** Filters bad requests and rate-limits login, OTP, AI, and the API.
- **Amazon S3:** Stores the frontend build and ALB logs.
- **Elastic Load Balancing (ALB):** Routes API requests to the backend.
- **Amazon ECS (Fargate) + Application Auto Scaling:** Runs the Spring Boot backend and scales between 1 and 3 tasks.
- **Amazon ECR:** Stores the backend Docker image.
- **Amazon VPC:** Network across 2 AZs, S3 Gateway Endpoint, VPC Flow Logs.
- **AWS Secrets Manager:** Stores the database password and integration keys.
- **AWS IAM:** Least-privilege access for tasks, Lambda, and GitHub Actions (OIDC).
- **Amazon Bedrock:** Nova Pro model and Guardrails for Generative UI.
- **Amazon EventBridge, Amazon SQS, AWS Lambda:** Process business events and asynchronous notifications.
- **Amazon SES:** Sends emails to customers and staff.
- **Amazon CloudWatch:** Logs, alarms, RUM, and metrics for the Admin dashboard.
- **Amazon Cognito:** Identity Pool so CloudWatch RUM can send data.
- **Amazon SNS:** Sends system alerts by email.
- **Amazon Athena:** Queries ALB logs.
- **AWS Budgets:** Tracks monthly cost.

Outside AWS: Supabase (PostgreSQL + Auth), SePay (payments), Cloudinary (images), Vietmap (maps), GitHub Actions (CI/CD).

#### Component Design

- **Access and protection layer:** Route 53, CloudFront, WAF, and ACM receive every request and block bad traffic.
- **Frontend layer:** React served from S3 through CloudFront, including the base UI, the AI assistant, and the A2UI runtime.
- **Application layer:** Spring Boot on ECS Fargate behind the ALB, handling business logic, authorization, and AI orchestration.
- **AI layer:** Bedrock (Nova Pro) and Guardrails, with business tools and the A2UI validator.
- **Data layer:** Supabase PostgreSQL and Supabase Auth.
- **Event and notification layer:** Outbox → EventBridge/SQS → Lambda → SES and web notifications.
- **Monitoring layer:** CloudWatch, SNS, Athena, Budgets, and the Admin monitoring dashboard.

#### AI Pipeline

1. **Authentication and quota:** Only signed-in Customers can use AI, up to 20 requests per hour.
2. **Guardrails:** Bedrock Guardrails checks the customer's message; blocked content gets a refusal and the model is not called.
3. **Intent detection:** Nova Pro identifies what the customer wants (search, compare, reorder…) and extracts the needed details.
4. **Real data:** The backend calls the product search, page data, or reorder tool, together with the customer's habit profile. Prices and stock are bound into the UI by the backend; the model only sees names and result counts.
5. **UI generation:** Nova Pro builds an A2UI layout from the component catalog.
6. **Validation:** The backend checks components, properties, and allowed actions, and does not let the model write money amounts. If validation fails, it retries once, then falls back to a default layout.
7. **Rendering:** The result is streamed (SSE) to the frontend and rendered with registered components. Out-of-scope questions get a text answer.

### 4. Technical Implementation

#### Implementation Phases

1. **Requirements and database** – Completed
2. **UI design and repository** – Completed
3. **API, interface, and role-based access** – Completed
4. **AWS infrastructure and CI/CD** – Completed
5. **SePay, Vietmap, Cloudinary, Bedrock, and Generative UI integration** – Completed
6. **Testing, cost optimization, and Generative UI evaluation** – In progress

#### Technical Requirements

| Category | Technologies |
| --- | --- |
| **Frontend** | React 19, TypeScript, Vite, Tailwind CSS, TanStack Query, Zustand, i18next (Vietnamese/English) |
| **Backend** | Java 21, Spring Boot 4.1, Spring Security, JPA, Flyway |
| **Data and authentication** | Supabase PostgreSQL, Supabase Auth (JWT) |
| **AI and Generative UI** | Amazon Bedrock (Nova Pro), Bedrock Guardrails, A2UI |
| **AWS infrastructure** | Route 53, CloudFront, ACM, WAF, S3, ALB, ECS Fargate, ECR, Secrets Manager |
| **Events and email** | EventBridge, SQS, Lambda, SES |
| **Monitoring** | CloudWatch (Logs, Alarms, RUM), SNS, Athena, AWS Budgets |
| **CI/CD** | Docker, GitHub Actions + OIDC |
| **External integrations** | SePay, Cloudinary, Vietmap |

### 5. Timeline & Milestones

| Period | Activities | Deliverables |
| --- | --- | --- |
| **Sep 18–23** | Finalize the idea, write requirements, create repositories | Frontend/backend repositories, requirements document |
| **Sep 25–Oct 7** | Design the database and interface | Schema and first Flyway migration, Figma design |
| **Oct 9–21** | Build business APIs, sign-in, and delivery; try Bedrock and A2UI | Core APIs for each role, Supabase Auth authentication |
| **Oct 23–28** | Quotes, orders, SePay payment | Working quote → QR payment flow |
| **Oct 30–Nov 4** | Localization, Docker packaging | Vietnamese/English UI, backend Docker image |
| **Nov 6–11** | AWS deployment and CI/CD; notifications through SQS, Lambda, SES | Website running on AWS, GitHub Actions pipeline, CloudWatch RUM |
| **Nov 13–18** | Finish Generative UI, SePay reconciliation, monitoring dashboard, put the API behind CloudFront | AI assistant and AI-built pages, UI habit profile |
| **From Nov 20** | Testing, Generative UI evaluation, documentation, and workshop | Comparison results, deployment guide, demo |

### 6. Budget Estimation

Singapore Region prices, November 2026, excluding taxes and services outside AWS.

| Service | 1 task | 3 tasks (maximum) |
| --- | --- | --- |
| ECS Fargate (0.5 vCPU / 2 GB per task) | ~ 26.5 USD | ~ 79.6 USD |
| Application Load Balancer | ~ 24.2 USD | ~ 24.2 USD |
| Public IPv4 | ~ 11.0 USD | ~ 18.3 USD |
| AWS WAF | ~ 21 USD | ~ 21 USD |
| Other services (S3, CloudFront, Route 53, CloudWatch, Secrets Manager…) | a few USD | a few USD |
| **Infrastructure total / month** | **~ 85 USD** | **~ 145 USD** |

**AI cost (Nova Pro):** about 0.0067 USD per call (assuming 3,000 input and 800 output tokens), or about 6.7 USD for 1,000 calls and 67 USD for 10,000 calls per month.

### 7. Risk Assessment

#### Risk Matrix

| Risk | Level | Description |
| --- | --- | --- |
| **Data security** | High | Customer data leaks, spoofed payment webhooks, prompt injection into the AI |
| **Cost overruns** | Medium | AI abuse or attacks increase tasks and model calls |
| **AI disruption** | Medium | Slow model, stream cut after 60 seconds of silence, malformed AI output |
| **External dependencies** | Medium | Outages at Supabase, SePay, Vietmap, or Cloudinary |

#### Mitigation Strategies

- **Security:** All traffic goes through CloudFront and WAF; the ALB only accepts requests from CloudFront; secrets live in Secrets Manager; the SePay webhook is verified; the AI only receives permission-scoped data and is checked by Guardrails.
- **Cost:** A maximum of 3 tasks, 20 AI requests per hour per customer, AWS Budgets, and alarms through SNS.
- **AI disruption:** Heartbeats in the stream, A2UI validation with one retry, and fallback to the base UI.
- **External services:** Timeouts, retries, scheduled payment reconciliation, and clear error messages for users.

### 8. Expected Outcomes

#### Technical Improvements

- **Personalized interface:** Generative UI adapts to each customer's intent and habits, with a safe fallback.
- **Complete workflow:** Quote → payment → warehouse → delivery on one system, with web and email notifications.
- **Automated operations:** CI/CD, auto scaling, monitoring, and cost alerts.

#### Long-term Value

- **Evaluation data:** Comparison of the fixed and personalized interfaces (task completion time, interaction count, error rate).
- **Extensible platform:** New AI tools, notification types, and roles are easy to add.
- **Team knowledge:** Hands-on experience with Bedrock, ECS, CloudFront, WAF, and event-driven architecture on AWS.
