---
title: "Proposal"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# SnapResume Platform

### 1. Project Summary

**SnapResume** is an intelligent resume/CV building platform powered by AI, helping job seekers create professional, ATS-friendly (Applicant Tracking System) resumes quickly and efficiently. The platform provides modern design templates, an intuitive editor, and an AI assistant to optimize content, fix grammar, and suggest keywords relevant to job descriptions.

Its key differentiator is the integration of AI content generation (`GenAI`) using `AWS Bedrock` (`Claude 3 Sonnet`), allowing users to produce professional summaries and impressive experience descriptions from only basic bullet points. The system is fully built on a `Serverless` architecture, ensuring infinite scalability and optimized cost.

### 2. Problem Statement

#### What is the problem?

- **Difficult formatting**: Job seekers spend hours formatting text in `Word` or complex design tools but still fail to achieve a professional look.
- **Weak content**: Candidates struggle to write concise content, use industry terminology, and highlight achievements. “Writer’s block” is extremely common.
- **Fails ATS screening**: Manually designed resumes often contain graphics or table structures that employer ATS systems cannot parse.

#### The Solution

SnapResume solves these problems using a modern `React` web application built on `AWS Serverless`.

- **Visual editor**: Drag-and-drop UI or simple form-based input with real-time preview.
- **AI Copilot**: Uses `AWS Bedrock` to rewrite sentences and generate content tailored to the job role.
- **ATS-Friendly**: Templates designed to remain machine-readable while still visually appealing.
- **Centralized management**: Store multiple resume versions and export high-quality `PDF`.

### 3. Solution Architecture

The system uses a `Serverless` and `Event-driven` architecture on AWS.
![AWS Architecture](/images/2-Proposal/SnapResume.png)

#### Architecture Overview

- **Frontend**: `React 19` + `Vite` (hosted on `S3` + `CloudFront` or `AWS Amplify`).
- **API Layer**: `Amazon API Gateway` + `AWS Lambda` (`Node.js`/`Express`).
- **Database**: `Amazon DynamoDB` (Single Table Design) storing `User`, `Resume`, `Section`, `Template`.
- **Authentication**: `Amazon Cognito` (User Pools & Identity Pools).
- **AI Engine**: `Amazon Bedrock` (`Claude 3 Sonnet`) for content generation.
- **Storage**: `Amazon S3` for storing profile images and exported `PDF` files.

### 4. Technical Implementation

#### Technology Stack

- **Frontend**: `ReactJS`, `TailwindCSS`, `Ant Design`, `html2pdf.js` for PDF export.
- **Backend**: `NodeJS`, `TypeScript`, `Express` (running inside `Lambda`).
- **IaC**: `Terraform` managing all infrastructure.

#### Development Phases

- **Phase 1: Core Foundation (January)**
  - Set up AWS infrastructure (`Terraform`).
  - Build Authentication (`Cognito`).
  - Basic Resume `CRUD` (`DynamoDB` + `Lambda`).
- **Phase 2: Editor & Templates (February)**
  - Develop drag-and-drop Editor UI.
  - Build dynamic Template system.
  - PDF export (`html2pdf.js` / `Puppeteer`).
- **Phase 3: AI Integration & Polish (March)**
  - Integrate `Amazon Bedrock` for “AI Writer”.
  - UX/UI optimization.
  - Load testing & security hardening.

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
