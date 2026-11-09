---
title: "Proposal"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Bite to Business

### 1. Project Summary

**Bite to Business** is a project to build a food sales website, focusing on business customers such as restaurants, hotels, and catering providers while also supporting individuals and households. The system supports the workflow from browsing products and creating purchase requests (Quotes) through staff review, order confirmation when all requested items are available, preparation, and delivery.

Its key feature is **Generative UI**, which adapts the interface to each user's intent, preferences, and purchasing context. The system plans to integrate **Amazon Bedrock**, with **Claude Haiku 5.5** proposed as the primary model for the MVP, to recommend products and generate JSON UI configurations using a component library built by the team. React renders validated configurations, while the backend manages authorization, pricing, inventory, and transactions. AI also supports role-scoped chatbot assistance and drafts poster content and layouts for E-commerce staff to preview, edit, and approve.

The application uses **React/TypeScript**, **Spring Boot**, and **Supabase PostgreSQL**, with deployment planned on **AWS**. The project aims to simplify purchasing, support coordination between sales, warehouse, and delivery teams, and evaluate the effectiveness of personalized interfaces.

### 2. Problem Statement

#### What is the problem?

In B2B food purchasing, customers may need to discover ingredients, compare quantity-based prices, reorder familiar products, or allocate items across delivery addresses. Each task requires different information and interactions to take priority.

- **A fixed interface may not fit each purchasing need:** A shared layout can require repeated searches, filtering, and page navigation, even when buyers can clearly express their goals.
- **Changing interfaces can lose task context:** Selected products, quantities, sales units, Quotes, and delivery groups need to remain consistent as customers ask follow-up questions or switch layouts.
- **Generated interfaces need correct data and workflows:** Quantity changes can affect price tiers, totals, and availability. The UI needs current backend data and actions appropriate to the user's permissions and approval status.
- **Slow or failed responses can interrupt purchasing:** An outdated response can overwrite newer interactions, while interrupted UI generation can lose progress or leave stale information on screen.

The challenge for **Bite to Business** is to adapt the interface to each user's intent and context while preserving purchasing continuity and valid transactions.

#### The Solution

**Bite to Business** proposes integrating **Generative UI** through **Amazon Bedrock**, with **Claude Haiku 5.5** proposed as the primary model for the MVP. The solution includes:

- **Task-based personalization:** The backend supplies relevant context and preferences; AI proposes layouts using the team's component catalog. For example, it can prioritize a table for bulk purchasing or a reorder list for returning customers.
- **A validated A2UI runtime:** Receive JSON messages using the selected protocol version, validate schemas, components, data bindings, and permitted actions, then render them through the website's React component library.
- **Continuous interaction context:** Send product identifiers, quantities, the Quote, and the state revision with each action to the backend. Apply results that remain relevant to the current task and preserve valid selections across layout changes.
- **Backend-controlled transactions:** Validate pricing, promotions, inventory, authorization, and approval steps on the backend. Order confirmation succeeds only when all requested items are available, staff review is complete, and the buyer confirms.
- **Fallback when AI fails:** Retain valid interface elements or return to the base UI, preserve drafts where possible, and explain the current state so users can continue.

Effectiveness will be compared with the fixed interface using task completion rate and time, interaction count, rendering and data-binding correctness, and recovery from failures.

### 3. Solution Architecture

**Bite to Business** plans to deploy on AWS with the following components:

| Component | Responsibility |
| --- | --- |
| **Frontend** | React/TypeScript on private S3 + CloudFront, including the base UI and A2UI runtime. |
| **Backend** | Spring Boot on private ECS Fargate tasks behind an ALB; handles APIs, authorization, and business rules. |
| **Data / Authentication** | Supabase PostgreSQL stores data; Supabase Auth manages authentication. |
| **AI** | The backend calls Amazon Bedrock — Claude Haiku 5.5 for product recommendations and proposed UI configurations. |
| **External integrations** | Cloudinary for images, Vietmap for maps, SePay for payments, and SMTP for email. |
| **Operations** | ECR stores images, Secrets Manager manages secrets, and CloudWatch collects logs and metrics. |

**Generate UI flow:** Request and context → backend authentication/data retrieval → Bedrock UI proposal → backend validation/A2UI conversion → runtime validation and React rendering.

Actions return to the backend for authorization, pricing, inventory, and transaction checks. The runtime preserves context, rejects outdated responses, and falls back to the base UI when AI fails.

### 4. Technical Implementation

#### Technology Stack

| Category | Technologies |
| --- | --- |
| **Frontend** | React, TypeScript, Vite, Tailwind CSS, React Router |
| **State and API data** | TanStack Query, Zustand, Axios |
| **Forms and validation** | React Hook Form, Zod |
| **Backend** | Java 21, Spring Boot, Spring Security, Spring Data JPA/Hibernate |
| **Database / Migration** | Supabase PostgreSQL, Flyway |
| **Authentication** | Supabase Auth, JWT |
| **Generative UI — planned** | A2UI, JSON Schema, React component catalog and runtime |
| **AI — planned** | Amazon Bedrock, Claude Haiku 5.5 |
| **Deployment — planned** | Docker, ECR, ECS Fargate, ALB, S3, CloudFront |
| **Monitoring and secrets — planned** | CloudWatch, Secrets Manager |
| **External integrations** | Cloudinary (planned), Vietmap, SePay, SMTP |
| **Localization** | i18next, react-i18next |

#### Development Phases

1. **Requirements and database:** Write requirements, define business workflows, and design the schema and create the database on Supabase PostgreSQL.
2. **UI design and repository:** Design the interface in Figma; create the GitHub repository and frontend/backend project structure.
3. **Development and authentication:** Implement the interface and APIs, build authentication with Supabase Auth, and enforce role-based access for core features.
4. **Service integration:** Connect SePay, Amazon Bedrock/Claude Haiku 5.5, the A2UI runtime, and Vietmap API.
5. **UX/UI and performance optimization:** Refine the experience and optimize page loading, API response times, and database queries.
6. **AWS setup and deployment:** Provision infrastructure following the AWS architecture, configure the environment, deploy, and verify the website.

### 5. Budget Estimation (AWS Cost)

The `Serverless` pricing model scales with usage. AI cost (`Bedrock`) becomes a major factor compared to traditional apps.

#### Assumptions

- 1 Resume = 5 AI calls (optimization, rewriting descriptions).
- 1 AI call = 1,000 input tokens + 500 output tokens.
- `Bedrock` pricing (`Claude 3 Sonnet`): $3.00 / 1M input, $15.00 / 1M output.
- AI cost per Resume: ~ $0.05 (≈ 1,200 VND).
- PDF file size: 2MB.

| Service             | Pricing Model | Low Traffic (MVP/Test) | Medium Traffic | High Traffic          |
| :------------------ | :------------ | :--------------------- | :------------- | :-------------------- |
| **Scale**           |               | **< 500 users/month**  | **~5,000/mo**  | **~50,000/mo**        |
| Resumes generated   |               | 200 CV                 | 2,000 CV       | 20,000 CV             |
| Amazon S3           | Storage       | $0.10                  | $1.00          | $15.00                |
| CloudFront          | CDN           | $0.50                  | $5.00          | $50.00                |
| API GW + Lambda     | Compute       | Free Tier              | $5.00          | $40.00                |
| DynamoDB            | Database      | Free Tier              | $2.00          | $20.00                |
| Cognito             | Auth          | Free Tier              | Free Tier      | Free Tier (< 50k MAU) |
| Amazon Bedrock (AI) | Tokens        | $10.00                 | $100.00        | $1,000.00             |
| WAF + Route53       | Security      | $7.00                  | $7.00          | $15.00                |
| **Total / Month**   |               | **~ $18**              | **~ $120**     | **~ $1,140**          |

> Note: AI cost (`Bedrock`) becomes dominant at scale. Costs can be optimized by using smaller models such as `Claude 3 Haiku` for simple tasks, reducing AI cost to about 1/5.

### 6. Risk Assessment

#### Security & Privacy Risks (High)

- **Issue**: Resumes contain sensitive PII — phone number, email, address.
- **Mitigation**:
  - Encrypt data at rest (`DynamoDB`, `S3`).
  - Enforce strict access permissions (`IAM Roles`).
  - User data is not used for AI training (`AWS Bedrock` does not train on customer inputs).

#### Cost Risk (Medium)

- **Issue**: API `DDoS` attacks or AI abuse can incur high cost.
- **Mitigation**:
  - Apply throttling on `API Gateway`.
  - Limit daily AI calls per free-tier account.
  - Set up `AWS Budget Alerts`.

#### Architectural Risk

- **Issue**: Lambda cold start may slow first-time user experience.
- **Mitigation**: Use `Lambda SnapStart` (for Java) or `Provisioned Concurrency` if needed (Node.js usually has fast cold start).

### 7. Expected Outcomes

- A complete `SaaS` platform targeting the recruitment market.
- Highly scalable system supporting tens of thousands of users without server management.
- AI integration provides a significant competitive advantage over traditional CV builders.
