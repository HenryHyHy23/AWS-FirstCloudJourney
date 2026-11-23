---
title: "Proposal"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Bite to Business

### 1. Project Summary

**Bite to Business** is a project to build a food sales website, focusing on business customers such as restaurants, hotels, and catering providers while also supporting individuals and households. The system supports the workflow from browsing products and creating a Quote, through backend validation of prices, promotions, VAT, delivery fees, and inventory, the customer confirming the final quote, QR payment through SePay, to preparation and delivery.

Its key feature is **Generative UI**, which adapts the interface to each user's intent, preferences, and purchasing context. The backend calls **Amazon Bedrock** with the **Amazon Nova Pro** model to recommend products and generate JSON UI configurations using a component library built by the team; **Amazon Bedrock Guardrails** checks user input sent to the AI. React renders validated configurations, while the backend manages authorization, pricing, inventory, and transactions.

The application uses **React/TypeScript**, **Spring Boot (Java 21)**, and **Supabase PostgreSQL**, and has been deployed on **AWS** in the Singapore Region (`ap-southeast-1`). The project aims to simplify purchasing, support coordination between sales, warehouse, and delivery teams, and evaluate the effectiveness of personalized interfaces.

### 2. Problem Statement

#### What is the problem?

In B2B food purchasing, customers may need to discover ingredients, compare quantity-based prices, reorder familiar products, or allocate items across delivery addresses. Each task requires different information and interactions to take priority.

- **A fixed interface may not fit each purchasing need:** A shared layout can require repeated searches, filtering, and page navigation, even when buyers can clearly express their goals.
- **Changing interfaces can lose task context:** Selected products, quantities, sales units, Quotes, and delivery groups need to remain consistent as customers ask follow-up questions or switch layouts.
- **Generated interfaces need correct data and workflows:** Quantity changes can affect price tiers, totals, and availability. The UI needs current backend data and actions appropriate to the user's permissions and order status.
- **Slow or failed responses can interrupt purchasing:** An outdated response can overwrite newer interactions, while interrupted UI generation can lose progress or leave stale information on screen.

The challenge for **Bite to Business** is to adapt the interface to each user's intent and context while preserving purchasing continuity and valid transactions.

#### The Solution

**Bite to Business** integrates **Generative UI** through **Amazon Bedrock (Amazon Nova Pro)**. The solution includes:

- **Task-based personalization:** The backend supplies relevant context and preferences; AI proposes layouts using the team's component catalog. For example, it can prioritize a table for bulk purchasing or a reorder list for returning customers.
- **A validated A2UI runtime:** Receive JSON messages using the selected protocol version, validate schemas, components, data bindings, and permitted actions, then render them through the website's React component library.
- **Continuous interaction context:** Send product identifiers, quantities, the Quote, and the state revision with each action to the backend. Apply results that remain relevant to the current task and preserve valid selections across layout changes.
- **Backend-controlled transactions:** Validate pricing, promotions, VAT, delivery fees, inventory, and authorization on the backend. When the customer confirms the final quote, the system creates an Order, holds stock for up to six hours, and offers QR payment through SePay; payment status changes only after a valid SePay webhook.
- **Fallback when AI fails:** Retain valid interface elements or return to the base UI, preserve drafts where possible, and explain the current state so users can continue.

Effectiveness will be compared with the fixed interface using task completion rate and time, interaction count, rendering and data-binding correctness, and recovery from failures.

### 3. Solution Architecture

The system runs on AWS in the Singapore Region (`ap-southeast-1`); edge resources (CloudFront, its ACM certificate, and its Web ACL) are in `us-east-1` as CloudFront requires. The status below is current as of November 2026.

| Component | Service / configuration | Status |
| --- | --- | --- |
| **Domain and HTTPS** | Route 53 manages the hosted zone; AWS Certificate Manager issues HTTPS certificates. | Deployed |
| **Single entry point** | All user traffic enters through CloudFront: `/*` → React/Vite build in a private S3 bucket (Origin Access Control; a CloudFront Function rewrites SPA routes to `index.html`); `/api/*` → Internet Gateway → ALB with caching disabled, all viewer headers and cookies forwarded, and a 60-second origin timeout for AI streaming. The frontend and API share one domain, so no CORS is needed. | Deployed |
| **Protection** | AWS WAF with 2 Web ACLs. **CloudFront:** 5 per-IP rate-limit rules on `/api/*` (API 2,000, AI 30, login 30, OTP verify/resend 10 requests per 5 minutes, returning 429) plus the IP reputation and known-bad-inputs managed groups in Block; the common rule set and Anti-DDoS group stay in Count while logs are reviewed. There is no country or VPN blocking, so international and VPN users are served. **ALB:** accepts only requests carrying a secret header added by CloudFront, plus the rate-limited SePay webhook path, so the ALB cannot be called directly. | Deployed |
| **Load balancer** | Application Load Balancer (HTTPS only, `/api/health` health check) behind CloudFront; access logs are stored in S3 (30-day lifecycle) and queried with Amazon Athena. | Deployed |
| **Backend** | Spring Boot on ECS Fargate, 0.5 vCPU / 2 GB per task; Application Auto Scaling keeps 1–3 tasks with a 60% CPU target. | Deployed |
| **Networking** | VPC across 2 AZs with an Internet Gateway; tasks run in public subnets with public IPs, and the security group only allows inbound traffic from the ALB; no NAT Gateway. S3 Gateway Endpoint and VPC Flow Logs. | Deployed |
| **Data and authentication** | Supabase PostgreSQL (schema managed by Flyway); Supabase Auth issues JWTs validated by Spring Security; refresh tokens stored in an HttpOnly cookie. | Deployed |
| **AI** | The backend calls Amazon Bedrock (Converse API) with Amazon Nova Pro through the APAC cross-region inference profile; Bedrock Guardrails checks content; AI calls are limited per user. The `/api/ai/agent` endpoint streams results (SSE). | Deployed |
| **Event bus and notifications** | The backend writes domain events (`order.*`, `payment.*`) to an outbox table in the same transaction; a relay publishes them to the Amazon EventBridge bus `btb-events`. A rule routes customer events to Amazon SQS (`btb-customer-events`); a Lambda function reads the queue, stores web notifications, and sends emails through the Amazon SES API. Failed deliveries are retried and then moved to a DLQ; the Lambda function deduplicates by event ID. | Deployed |
| **More notification consumers** | New rules on the same bus: staff notifications (out-of-stock alerts for logistics, new product requests for e-commerce and admin) and marketing emails (flash sales and Black Friday for opted-in customers, scheduled with EventBridge Scheduler), each with its own SQS queue and Lambda function. | Planned |
| **Email** | Amazon SES with a verified domain: Lambda sends through the SES API, and the backend sends through SES SMTP. | Deployed |
| **Secrets and IAM** | Secrets Manager stores the DB password and integration keys and injects them into the container; IAM separates the task role, Lambda role, GitHub OIDC provider, and SMTP user. | Deployed |
| **Monitoring and alerts** | CloudWatch Logs and Alarms; alarms notify the team by email through Amazon SNS; CloudWatch RUM measures frontend performance (a Cognito Identity Pool is used only so RUM can send data, not for sign-in); AWS Budgets tracks monthly cost. | Deployed |
| **CI/CD** | GitHub Actions uses OIDC to obtain temporary AWS credentials: the backend builds an image → ECR → updates the ECS service; the frontend builds → syncs to S3 → invalidates CloudFront. | Deployed |
| **Generative UI** | `@a2ui/react` and `@json-render` have been added to the FE; the component catalog and renderer are being built. | In development |
| **Outside AWS** | Supabase (PostgreSQL + Auth), Cloudinary (images), SePay (QR bank transfer + webhook), Vietmap (maps, delivery routes), GitHub Actions. | Integrated |

**Request flow:** User → CloudFront (+ WAF) → `/*` S3 (web) or `/api/*` → Internet Gateway → ALB (+ WAF) → ECS. The only direct path to the ALB is the SePay payment webhook.

**Generate UI flow:** Request and context → backend authentication and permission-scoped data retrieval → Guardrails content check → Bedrock (Nova Pro) UI proposal → backend validation and A2UI conversion → streaming to the FE → runtime validation and React rendering.

Actions return to the backend for authorization, pricing, inventory, and transaction checks. The runtime preserves context, rejects outdated responses, and falls back to the base UI when AI fails.

### 4. Technical Implementation

#### Technology Stack

| Category | Technologies |
| --- | --- |
| **Frontend** | React 19, TypeScript, Vite, Tailwind CSS, React Router |
| **State and API data** | TanStack Query, Zustand, Axios |
| **Forms and validation** | React Hook Form, Zod |
| **Backend** | Java 21, Spring Boot 4.1, Spring Security (OAuth2 Resource Server), Spring Data JPA/Hibernate |
| **Database / Migration** | Supabase PostgreSQL, Flyway |
| **Authentication** | Supabase Auth, JWT, refresh token in an HttpOnly cookie |
| **Generative UI** | A2UI, json-render, JSON Schema, React component catalog and runtime (in development) |
| **AI** | Amazon Bedrock (Converse API), Amazon Nova Pro, Bedrock Guardrails |
| **Edge and security** | Route 53, CloudFront (+ CloudFront Functions), AWS Certificate Manager, AWS WAF, IAM, Secrets Manager |
| **Compute and networking** | ECS Fargate, Application Auto Scaling, ALB, ECR, VPC (Internet Gateway, S3 Gateway Endpoint) |
| **Event-driven and email** | Amazon EventBridge, Amazon SQS (with DLQ), AWS Lambda, Amazon SES |
| **Monitoring and cost** | CloudWatch Logs, Alarms, RUM; Amazon SNS; VPC Flow Logs; Amazon Athena; AWS Budgets |
| **CI/CD** | Docker, GitHub Actions + OIDC |
| **External integrations** | Supabase, SePay, Cloudinary, Vietmap |
| **Localization** | i18next, react-i18next |

#### Development Phases

1. **Requirements and database:** Write requirements, define business workflows, and design the schema and create the database on Supabase PostgreSQL. *(Completed)*
2. **UI design and repository:** Design the interface in Figma; create the GitHub repository and frontend/backend project structure. *(Completed)*
3. **Development and authentication:** Implement the interface and APIs, build authentication with Supabase Auth, and enforce role-based access. *(Core features completed)*
4. **AWS setup and deployment:** Build the VPC, ALB, ECS Fargate with auto scaling, S3/CloudFront, WAF, Route 53, SES, EventBridge/SQS/Lambda, and the GitHub Actions + OIDC pipeline; move the API behind CloudFront as a single entry point. *(Completed)*
5. **Service integration:** SePay, Vietmap, Cloudinary, Bedrock/Nova Pro + Guardrails; finish the A2UI runtime. *(Generative UI in progress)*
6. **UX/UI, performance, and cost optimization:** Monitor RUM, ALB logs through Athena, alarms, and Budgets; tune task size, auto scaling, and WAF rules. *(In progress)*

### 5. Budget Estimation (AWS Cost)

Prices come from the AWS Price List for the Singapore Region (`ap-southeast-1`), November 2026; one month is 730 hours. The table reflects the running configuration and excludes taxes and services outside AWS (Supabase, Cloudinary, Vietmap, SePay).

| Service | Pricing | 1 task (usual) | 3 tasks (maximum) |
| --- | --- | --- | --- |
| **ECS Fargate** (0.5 vCPU / 2 GB per task) | 0.05056 USD per vCPU-hour + 0.00553 USD per GB-hour ≈ 26.5 USD per task | ~ 26.5 USD | ~ 79.6 USD |
| **Application Load Balancer** | 0.0252 USD per hour + 0.008 USD per LCU-hour (assuming 1 LCU) | ~ 24.2 USD | ~ 24.2 USD |
| **Public IPv4** | 0.005 USD per hour per address (2 for the ALB + 1 per task) | ~ 11.0 USD | ~ 18.3 USD |
| **AWS WAF** | 5 USD per Web ACL × 2 + 1 USD per rule or managed group (9 on CloudFront + 2 on the ALB) + 0.60 USD per million requests | ~ 21 USD | ~ 21 USD |
| **Secrets Manager** | 0.40 USD per secret (1 JSON secret) | ~ 0.4 USD | ~ 0.4 USD |
| **S3, CloudFront, Route 53, ECR, CloudWatch, Athena, SNS, EventBridge** | Based on storage, traffic, requests, log volume, data scanned, and events; small at MVP scale | a few USD | a few USD |
| **Infrastructure total (excluding AI)** | | **~ 85 USD** | **~ 145 USD** |

Removing the NAT Gateway saves about 43 USD per month in hourly charges (0.059 USD per hour × 730 hours), plus 0.059 USD for every GB that would have passed through it.

**AI cost (Amazon Nova Pro, Singapore):** 0.00108 USD per 1,000 input tokens and 0.00432 USD per 1,000 output tokens. Assuming 3,000 input tokens and 800 output tokens per call: 3 × 0.00108 + 0.8 × 0.00432 ≈ 0.0067 USD. Guardrails is charged separately based on the amount of text checked.

| Scale | AI calls / month | Model cost / month |
| --- | --- | --- |
| MVP / demo | 1,000 | ~ 6.7 USD |
| Medium | 10,000 | ~ 67 USD |
| High | 100,000 | ~ 670 USD |

At MVP scale, the SQS → Lambda → SES notification flow mostly stays within the SQS and Lambda free tiers; SES costs 0.10 USD per 1,000 emails on pay-as-you-go pricing (since July 21, 2026, new accounts default to the Essentials plan at 0.16 USD per 1,000 emails).

**Optimization options:**

- Measure real CPU and memory usage to tune the task size and auto scaling threshold.
- ALB logs expire after 30 days and CloudWatch Logs keep 14 days (both in place).
- Use AWS Budgets (already in place) and the per-user AI call limit (already in the backend).

### 6. Risk Assessment

#### Security & Privacy Risks (High)

- **Issue:** Business customer data (contacts, delivery addresses, orders) and payment transactions; risk of spoofed webhooks or prompt injection into the AI.
- **Mitigation:**
  - All traffic enters through CloudFront; the ALB only accepts requests carrying the CloudFront secret header (plus the SePay webhook), and ECS tasks only accept traffic from the ALB security group.
  - Per-IP rate limits protect login, OTP, AI, and the API; the IP reputation and known-bad-inputs rules block known attackers. The common rule set stays in Count until logs confirm it does not block real API requests.
  - Secrets live in Secrets Manager, not in the image or source code; CI/CD uses OIDC instead of long-lived access keys.
  - Verify the SePay webhook key and the amount before updating an order.
  - The AI only receives data within the user's permissions; prices, inventory, and order status always come from the backend; Bedrock Guardrails filters input content.

#### Cost Risk (Medium)

- **Issue:** Fixed costs of the ALB, WAF, and public IPv4 addresses; auto scaling adding tasks during an attack; abuse of AI features.
- **Mitigation:** A maximum of 3 tasks, WAF blocking bad requests, per-user AI call limits, AWS Budgets, and alarms through SNS.

#### Architecture and Operations Risks

- **Tasks in public subnets:** No NAT is needed, but each task has a public IP; every port must be locked down by the security group, with nothing open except the application port for the ALB.
- **AI streaming through CloudFront and the ALB:** CloudFront waits up to 60 seconds for the origin and the ALB closes connections that stay idle for 60 seconds; compression is disabled for `/api/*`, and the SSE stream should send periodic heartbeats so long model calls are not cut off.
- **Secret header rotation:** The CloudFront → ALB header value must match in both places; rotating it means updating the CloudFront origin and the ALB WAF rule together.
- **Dependencies outside AWS:** Supabase, SePay, Vietmap, and Cloudinary run outside AWS; timeouts, retries, and clear error messages are needed.
- **Secret changes:** Secrets are injected when the container starts; changing a secret requires redeploying the ECS service.
- **Duplicate emails:** EventBridge and SQS deliver at least once; the Lambda function records processed events to avoid sending duplicate emails.

### 7. Expected Outcomes

- A B2B food e-commerce website running on AWS that supports the full quote → confirmation → QR payment → delivery workflow, with event-driven web and email notifications that can be extended to staff and marketing notifications.
- A Generative UI that adapts to buyer intent, with validation and a safe fallback to the base UI.
- Comparison data between the fixed and personalized interfaces: task completion time, interaction count, and rendering error rate.
- Infrastructure with auto scaling, automated CI/CD, centralized secrets management, monitoring and cost alerts, and a clear cost estimate for optimization after the MVP.
