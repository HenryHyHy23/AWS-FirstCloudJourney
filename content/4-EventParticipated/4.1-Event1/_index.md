---
title: "Event 1"
date: 2026-09-26
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Summary Report: The Thinking Behind Building the Bot

The talkshow **The Thinking Behind Building the Bot**, organized by **CMC Global and VietAWS**, explored the thinking behind building AI Agents, from software development problems to business applications and the level of autonomy given to a bot.

### Event Information

| Field      | Details                                                                                |
| ---------- | -------------------------------------------------------------------------------------- |
| Date       | Saturday, September 26, 2026                                                           |
| Time       | 09:00–12:00 (UTC+7)                                                                    |
| Location   | 26th Floor, Bitexco Financial Tower, 2 Hai Trieu Street, Saigon Ward, Ho Chi Minh City |
| Organizers | CMC Global × VietAWS                                                                   |
| Role       | Attendee                                                                               |

### Event Objectives

- Understand how an AI Agent moves from generating answers to selecting tools and taking actions toward a defined goal.
- Relate agent design to business–development collaboration across time zones, including ticket context, progress notes, issue notifications, and controlled autonomy.
- Explore the AI Agent Challenge and exchange perspectives on Cloud & AI skills, practical projects, and career development.

### Featured Speakers

- **Bùi Nhật Trường** – Technical Leader, CMC Global.
- **Nguyễn Gia Hưng** – Head of Solutions Architect, AWS.

### Key Highlights

#### 1. Published Program

| Time (UTC+7) | Session                                   | Focus                                                                               |
| ------------ | ----------------------------------------- | ----------------------------------------------------------------------------------- |
| 09:00–09:30  | Buildathon Kickoff                        | Challenge introduction, objectives, judging criteria, AWS Cloud and GenAI overview. |
| 09:30–09:45  | Ice-breaking                              | Meet other participants and prepare for collaboration.                              |
| 09:45–10:30  | Talkshow: Building a Career in Cloud & AI | Industry perspectives, practical projects, and career opportunities.                |
| 10:30–10:45  | Q&A                                       | AWS, GenAI, careers, certifications, and challenge questions.                       |
| 10:45–11:15  | AI Agent Challenge                        | Prompt engineering, RAG, knowledge bases, and agent concepts.                       |
| 11:15–11:20  | Closing Remarks                           | Learning resources and community activities.                                        |

#### 2. The Thinking Behind an Agent

The central topic was how an AI Agent can understand context, use memory, decide a next step, and act as a developer's teammate. This raises design questions about which tasks to delegate and how much authority the system should have.

The discussions covered software development problems, enterprise applications, and a challenge focused on understanding how a bot works. Alongside the technical content, there were opportunities to connect with companies, exchange ideas, and take part in interviews.

#### 3. Demo: Business–Dev Coordination Across Time Zones and QA

- The demo presented a scenario in which the business and development teams worked in different time zones. The business team sent tickets to an AI Agent while developers were offline.
- The agent handled the requests, recorded notes and issues, and provided notifications so developers could review the work when they started their working day. This illustrated how an agent can support asynchronous collaboration and preserve context between teams.

### Key Takeaways

#### 1. Design the Goal and Execution Loop

- Define a specific task, success criteria, permitted tools, and a stopping condition. In an LLM-based agent, the model proposes a next step; application code executes the allowed tool and returns its result for the next iteration. See the [AWS explanation of agent orchestration](https://docs.aws.amazon.com/bedrock/latest/userguide/agents-how.html).
- The ticket demo illustrates why progress notes should distinguish work completed, blockers, and the next action for the developer. For QA-related work, a completion message should be supported by check results or a clear record of what still needs verification.

#### 2. Distinguish Context, Memory, and RAG

- Conversation history helps preserve context within an interaction; memory requires a deliberate strategy for storing and retrieving information. RAG retrieves relevant documents to ground a response and serves a different role from remembering a user's previous requests. See [AgentCore Memory](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/memory.html) and [Bedrock Knowledge Bases](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base.html).
- In a handoff across time zones, the returning developer needs the original request, a record of actions taken, and open questions. Preserving this ticket state serves a different purpose from retrieving reference documents; source quality and access permissions still need separate checks.

#### 3. Match Autonomy to Business Risk

- Limit tool permissions and validate parameters. Operations that change important data or systems should have suitable approval and recovery paths; the model's decision is not a substitute for authorization. AWS documents [user confirmation for agent actions](https://docs.aws.amazon.com/bedrock/latest/userguide/agents-userconfirmation.html) as one such mechanism.
- Evaluate task completion, failure cases, latency, and cost, and inspect execution traces when behavior differs from expectations. These checks connect an impressive demonstration to an application that can be operated and improved. See [AgentCore Observability](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/observability.html).

### References

- [Event program and poster](https://luma.com/8fe9vvps)
- [CMC Global Careers: event recap](https://vn.linkedin.com/company/cmc-global-careers)
- [AWS: What are AI Agents?](https://aws.amazon.com/what-is/ai-agents/)
- [AWS: AgentCore Memory](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/memory.html)
- [AWS: Amazon Bedrock Knowledge Bases](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base.html)
- [AWS: Agent action confirmation](https://docs.aws.amazon.com/bedrock/latest/userguide/agents-userconfirmation.html)
- [AWS: AgentCore Observability](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/observability.html)

---

![Checked in at the event](/images/4-EventParticipated/4.1-Event1/check-in.png)

*Checked in at the event*
