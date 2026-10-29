---
title: "Event 2"
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

# Event Report: AWS Cloud Day and AI

**Location:** Floor 26, Bitexco Financial Tower, Ho Chi Minh City.

**Role:** Attendee.

AWS Cloud Day and AI connected cloud architecture, data modernization, and agentic AI with practical enterprise use cases. The report brings together the keynote's engineering principles and selected program highlights, focusing on what makes an AI-enabled system reliable, governable, and useful in production.

### Event Objectives

- Understand the relationship between workload characteristics, systems thinking, and operational excellence.
- Explore how data foundations, agent architecture, and AI-assisted development fit together across the software lifecycle.
- Connect enterprise examples with decisions about reliability, security, cost, and human ownership.

### Featured Speakers

Speakers from the keynote and the 39 program sessions, grouped by organization.

| Organization / Group | Speakers and Roles |
| --- | --- |
| Amazon / Keynote | **Dr. Werner Vogels** – Chief Technology Officer and Vice President, Amazon (Keynote) |
| AWS – Solutions Architects | **Phuc Nguyen**, **Diep Nguyen Hoang**, **Dzung Ha**, **Khoi Phan**, **Quan Phuong**, **Trung Nguyen**, **Thanh Dang**, **Taiki Dang**, **Hien** |
| AWS – Senior / Specialist Solutions Architects | **Thi Nguyen**, **Lai Trung Hieu**, **Huy Tran** – Senior Solutions Architect; **Trung Tri Nguyen** – Analytics Specialist Solutions Architect; **Toan Huynh** – Specialist Solutions Architect |
| AWS – Customer / Account Management | **Hung Hoang**, **Linh Pham** – Senior Customer Solutions Manager; **Luu Thuy Linh** – Account Manager; **Tan Huynh**, **Trung Nguyen** – Technical Account Manager |
| AWS Professional Services | **Thanh Nguyen** – Senior Delivery Consultant |
| VPBank | **Thai Hong Pham** – Head of Enterprise Data Architect; **Yen Trinh** – Application Platform Engineer |
| Techcom Securities (TCBS) | **Đoàn Thế Long** – Senior Manager, Software Engineering |
| Vietnam International Bank (VIB) | **Tuan Le** – Head of AI Center; **Hoang Nguyen** – AWS Cloud Engineer |
| TechX Corporation | **Ngô Mạnh Hà** – CEO |
| Techcom Life | **Pham Tan Anh Khoa** – Head of Technology Governance & Delivery; **Vu Quang Tam** – Project Manager, AI Initiatives |
| G-AsiaPacific Vietnam | **Vo Xuan Minh Tuan** – Senior Solutions Architect; **Phạm Thị Hồng Hạnh** – Cloud Consultant |
| FCAJ | **Phuong Nguyen Huu** – Cloud Engineering; **Bui Huy Hoang** – Data Engineer |
| VinUni | **Nguyen Duc Manh** – AI Researcher |
| Canon Vietnam | **Dinh Ngoc Hieu** – System Administrator |
| SoftwareOne Việt Nam | **Nguyen Huu Toan** – Solution Architect |
| eCloudvalley | **Vu Gia Khanh** – Solution Architect Lead |
| Cloud Kinetics | **Dung Luong Tien** – Cloud Engineer |
| Katalon | **Vu Bui** – Technical Director |
| NTQ Solution | **Phạm Minh Quang** – DevOps Engineer |
| Renova Cloud | **Nguyễn Thái Bình** – Head of DevOps |
| Datadog | **Bang Truong** – Sales Engineer |
| Vingroup | **Kha Văn** – Cloud Infrastructure Security Specialist |
| GoTymeX | **Nguyễn Trung Trực** – Senior Platform Engineer; **Huỳnh Văn Toán** – Platform Lead |

### Keynote: Systems Thinking and the Renaissance Developer

#### 1. Choose Architecture from Actual Workloads

- The account of a major database outage at Amazon illustrated the limits of applying one technology to every access pattern. Simple key-value lookups and workloads requiring relational operations have different needs; examining those needs informed the development of Dynamo.
- The lesson is to justify a data-store choice through query patterns, consistency requirements, scale, and failure behavior.

#### 2. Design and Measure for Failure

- Tail latency reveals slow requests that averages or medians can hide. P99.9 describes a latency threshold covering approximately 99.9% of observed requests, not a percentage of service availability.
- Failure isolation, tested failover, recovery procedures, and GameDay exercises turn resilience from an architectural assumption into something teams can validate. Operational readiness includes understanding how dependencies behave during an outage.

#### 3. Develop Beyond Code Generation

- The Renaissance Developer combines curiosity, systems thinking, ownership, T-shaped expertise, and communication. Understanding business goals and explaining architectural trade-offs remain necessary even when AI accelerates implementation.
- AI-generated output still needs review for correctness, security, and edge cases. Engineers retain responsibility for what reaches production and should evaluate the effect of a change across the whole system.

### Main Content and Key Highlights

#### 1. AI-Ready Data Foundations and Enterprise Analytics

- The VPBank case described consolidating more than 600 TB from legacy warehouses into a Redshift analytics platform, with lakehouse integration and data sharing across teams. The architectural issue was enabling shared analytics while managing duplication, performance, and governance.
- Other data sessions emphasized Amazon S3, Apache Iceberg, open catalogs, and consistent governance. Data freshness, lineage, quality, and permissions determine whether retrieved information is suitable evidence for an agent's answer or action.

#### 2. AI-Native SDLC and Independent Quality Checks

- TCBS presented its adoption of Kiro and AI-DLC, reporting a 26% reduction in development time in its own case. The broader theme was applying AI to requirements, design, testing, documentation, and delivery rather than measuring progress only by generated code.
- Spec-driven development makes requirements and acceptance criteria explicit. Katalon's software-quality example reinforced the need to validate AI-generated work independently and retain traceable evidence of checks and decisions.

#### 3. Agent Architecture, Frameworks, and Production Services

- Sessions on agent harnesses, Strands, LangGraph, and Bedrock AgentCore examined orchestration, tool execution, state, memory, and observability. Framework-level control flow and managed operating services address different parts of the system.
- ReAct loops, MCP tools, retrieval, and multi-agent specialization connect reasoning to actions and external context. Extra agents or tools also introduce coordination and latency trade-offs, so their value should follow the task's requirements.
- MCP and codebase Knowledge Servers supplied AWS guidance and dependency graphs to coding assistants, helping them assess how changes affect connected services.

#### 4. GitOps, Platform Engineering, and Modernization

- Vinamilk's example linked AI-assisted infrastructure work with GitOps, EKS, Argo CD, human approval gates, and temporary access. Related IaC sessions used pull requests to review generated Terraform/OpenTofu changes and address configuration drift.
- The NestJS/Angular case moved from EC2/PM2 to EKS with FluxCD, making Git the source of truth and rollback a Git revert. The EKS IDP example used Backstage, Crossplane, and Terragrunt to standardize multi-account provisioning and reduce TicketOps.
- EKS Hybrid Nodes and AWS Transform sessions addressed containerization, SQL Server/.NET modernization, and migration-wave planning, with dependency, compatibility, and cutover checks.

#### 5. Security, Workload Identity, and Controlled Autonomy

- Workload-identity sessions connected IAM, STS, short-lived sessions, and identity context with least-privilege access across services and accounts. Removing static keys is only one part of establishing which workload may perform an action.
- Defense-in-depth and governance sessions combined data protection, evaluations, policy controls, and human approval. Agent access should expand according to verified evidence and business authority, with clear ownership of consequential changes.
- The DevOps Agent landing-zone session examined infrastructure read access, compliance, and hidden costs. AWS Security Agent covered organization-specific security reviews and on-demand application penetration testing.

#### 6. Cost Control and Observability for GenAI

- Bedrock cost-control and model-routing sessions addressed long prompts, repeated agent loops, traffic spikes, and choosing models appropriate to task complexity. Usage visibility needs actionable limits and near-real-time kill switches; a strong model is not automatically the most efficient choice for every request.
- Monitoring discussions added cost, safety, and answer quality to latency, errors, traffic, and saturation. S3 storage-tiering sessions made a related point: a cheaper storage class must also satisfy retrieval, access-frequency, and recovery requirements.

#### 7. Business Applications and Evidence-Based Operations

- TechcomLife illustrated cloud-native insurance. Amazon Quick Suite and Enterprise Knowledge Hub sessions connected fragmented data to governed business actions; Quick workshops explored custom agents, knowledge bases, prompts, skills, and deep research.
- Fraud detection combined Iceberg/Athena, Neptune knowledge graphs, XGBoost, and GraphRAG to investigate connected fraud. Manufacturing sessions linked SCADA, Historian, and MES data through an Industrial Data Fabric for contextualized analytics.
- GoTymeX's AWS DevOps Agent workflow correlated APM traces, Datadog metrics, and infrastructure signals across services and accounts; TechX's Xora material connected incident context with human-controlled recovery and verification. A plausible root-cause explanation still needs supporting evidence and a checked recovery outcome.

### Knowledge and Lessons Learned

- **Start with the workload:** Define access patterns, quality expectations, and business constraints before choosing a database, model, or agent architecture.
- **Keep ownership with engineers:** Review requirements, proposed changes, test evidence, and rollback plans even when AI performs much of the implementation.
- **Treat production readiness as a complete system:** Data governance, identity, resilience, observability, and cost controls need to work together.
- **Measure business outcomes:** Evaluate completed tasks, reliable recovery, delivery quality, and cost per useful result, alongside coding speed.

---

![Checked in at the event](/images/4-EventParticipated/4.2-Event2/check-in.png)

_Checked in at the event_
