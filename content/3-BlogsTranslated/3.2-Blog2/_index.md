---
title: "Blog 2"
date: 2026-11-19
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# WHEN AI DOES MORE THAN ANSWER: EXPLORING AI AGENTS WITH AMAZON BEDROCK AGENTCORE

**What if AI could break a request into steps, retrieve information, and check the result before responding?**

As I explore AI, I notice that some problems need more than a single answer. For example, recommending food supplies can involve understanding a customer's needs, finding suitable products, and checking information before suggesting anything.

This led me to **AI agents**: programs that combine AI models with tools and orchestration logic to carry out multiple steps toward a goal.

**How might an agent work?**

- A customer asks: “I need ingredients for 20 people with a budget of 500,000 VND.”
- The agent interprets the request and identifies missing details, such as the menu or number of meals.
- When data is needed, it can propose using a product search tool supplied by the application.
- The application checks permissions and calls the API; the agent uses the results to form an answer.
- If information is missing, the agent asks a follow-up instead of inventing products or prices.

An agent system can use one agent or several collaborating agents. For example, one can gather information while another reviews it. More agents also mean more model calls, latency, and cost; not every problem needs a multi-agent design.

**How can Amazon Bedrock AgentCore help?**

[Amazon Bedrock AgentCore](https://docs.aws.amazon.com/prescriptive-guidance/latest/agentic-ai-frameworks/amazon-bedrock-agent-core.html) offers capabilities for building and operating agents:

- **Runtime:** A serverless environment for running agents and tools, reducing infrastructure-management work.
- **Gateway:** Helps expose existing APIs and tools to agent workflows through MCP.
- **Memory:** Helps store and retrieve context when an application needs continuity across turns or sessions.
- **Identity:** Helps manage authentication and access to other services.
- **Observability:** Helps track activity, latency, and errors so I can understand what an agent is doing.

I do not have to use every component. For instance, an agent handling one simple request may not need long-term Memory.

**Bedrock and AgentCore serve different purposes**

I think of **Amazon Bedrock** as a way to access AI models, while **AgentCore** provides additional capabilities to run and connect agents. I still need to design the agent logic, declare tools, validate results, and decide which data can be used. AgentCore Runtime does not turn a model into a complete application automatically.

**How are APIs and data protected?**

- I expose only the tools needed for a task instead of giving an agent unrestricted access.
- The backend still authenticates users and checks authorization on every API call.
- Prices, stock, and order status must come from actual system data.
- Important actions such as ordering or payment require business checks and appropriate confirmation.
- Sensitive data and logs should be limited to what is needed.

I find agents useful when a request requires multiple steps or sources. The key is choosing the right level of complexity, testing results, and controlling access from the beginning.

**References:**

- [Original AWS post on Deep Agents and AgentCore](https://aws.amazon.com/blogs/machine-learning/running-deep-research-ai-agents-on-amazon-bedrock-agentcore/)
- [AgentCore Runtime and Harness](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/harness-vs-runtime.html)
- [Runtime security practices](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/runtime-security-best-practices.html)
