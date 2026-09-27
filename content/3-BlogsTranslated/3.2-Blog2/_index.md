---
title: "Blog 2"
date: 2025-09-23
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Artificial Intelligence: Running Deep Research AI Agents on Amazon Bedrock AgentCore

Amazon Bedrock Blog
Written by **Vadim Omeltchenko**, **Eashan Kaushik**, **Mark Roy**, and **Shreyas Subramanian** | **September 23, 2025** | Category: **Amazon Bedrock, Amazon Bedrock Agents, Generative AI, Technical How-to**

---

AI Agents are evolving beyond basic assistants performing single tasks to become more powerful systems capable of planning, critiquing, and collaborating with other agents to solve complex problems. **Deep Agents**—a new framework built on LangGraph—bring these capabilities to life, allowing multi-agent workflows to simulate real-world team dynamics.

However, the challenge is not just building such agents but running them reliably and securely in a **production environment**. This is where **Amazon Bedrock AgentCore Runtime** comes into play. By providing a secure, purpose-built **serverless environment** for AI Agents and tools, the Runtime helps deploy Deep Agents at enterprise scale without the heavy lifting of infrastructure management.

In this post, we will demonstrate how to deploy Deep Agents on AgentCore Runtime. As shown in the diagram below, AgentCore Runtime scales any agent and provides **session isolation** by provisioning a new microVM for each new session.


---

## What is Amazon Bedrock AgentCore?

Amazon Bedrock AgentCore is both **framework-agnostic** and **model-agnostic**, giving you the flexibility to deploy and operate advanced AI agents securely and at scale. Whether you are building with **Strands Agents**, **CrewAI**, **LangGraph**, **LlamaIndex**, or another framework—and running them on any large language model (LLM)—AgentCore provides the infrastructure to support them.

Its modular services are purpose-built for **dynamic agent workloads**, with tools to extend agent capabilities and the controls necessary for production use. By removing the undifferentiated heavy lifting of building and managing specialized agent infrastructure, AgentCore allows you to bring your preferred framework and model and deploy without rewriting code.

Amazon Bedrock AgentCore offers a comprehensive suite of capabilities designed to transform **local agent prototypes** into production-ready systems. These include:

* **Persistent memory** to maintain context within and across conversations.
* Access to existing APIs using the **Model Context Protocol (MCP)**.
* Seamless integration with corporate authentication systems.
* Specialized tools for web browsing and code execution.
* **Deep observability** into agent reasoning processes.

In this post, we focus specifically on the **AgentCore Runtime** component.

---

## Core Capabilities of AgentCore Runtime

AgentCore Runtime provides a **secure, serverless hosting environment** designed specifically for agentic workloads. It packages code into a **lightweight container** with a simple, consistent interface, making it equally suitable for running agents, tools, MCP servers, or other workloads that benefit from seamless scaling and built-in identity management.

AgentCore Runtime offers:

* **Extended execution time** of up to 8 hours for complex reasoning tasks.
* Handling of large payloads for **multimodal content**.
* **Consumption-based pricing** that charges only during active processing—not while waiting for LLM or tool responses.
* **Dedicated micro virtual machines (microVMs)** for each user session, ensuring complete isolation and preventing cross-session contamination.

The Runtime works with multiple frameworks (e.g., LangGraph, CrewAI, Strands) and foundation model providers, while offering **built-in corporate authentication**, specialized agent observability, and unified access to the broader AgentCore environment via a single SDK.

---

## Real-World Example: Integrating Deep Agents

In this post, we will deploy a newly released Deep Agents example implementation on AgentCore Runtime—showing how little effort is required to put the latest agent innovations into operation.

The example implementation includes:

1.  A **research agent** performing deep internet searches using the Tavily API.
2.  A **critique agent** reviewing and providing feedback on generated reports.
3.  A **main orchestrator** managing the workflow and handling file operations.

Deep Agents utilize LangGraph's **state management** to create a Multi-Agent system with:

* **Built-in task planning** via `write_todos` tool helping agents break down complex requests.
* **Virtual file system** where agents can read/write files to persist context between interactions.
* **Sub-agent architecture** allowing specialized agents to be invoked for specific tasks while maintaining **context isolation**.
* **Recursive reasoning** with a high recursion limit (over 1,000) to handle complex, multi-step workflows.

This architecture allows Deep Agents to handle research tasks requiring multiple rounds of information gathering, synthesis, and refinement.

### Code Integration

The beauty lies in its simplicity—we only need to add a few lines of code to make an agent compatible with AgentCore:

```python
# 1. Import the AgentCore runtime
from bedrock_agentcore.runtime import BedrockAgentCoreApp

app = BedrockAgentCoreApp()

# 2. Decorate your agent function with @app.entrypoint
@app.entrypoint
async def langgraph_bedrock(payload):
    # Your existing agent logic remains unchanged
    user_input = payload.get("prompt")
    
    # Call your agent as before
    stream = agent.astream(
        {"messages": [HumanMessage(content=user_input)]},
        stream_mode="values"
    )
    
    # Stream responses back
    async for chunk in stream:
        yield(chunk)

# 3. Add the runtime starter at the bottom
if __name__ == "__main__":
    app.run()
```

That's it! The rest of the code—model initialization, API integration, and agent logic—remains exactly the same. AgentCore handles the infrastructure while your agent handles the intelligence.

---

## Step-by-Step Implementation on AgentCore Runtime

Let's look at the actual deployment process using the **AgentCore Starter ToolKit**, which significantly simplifies the deployment workflow.

### Prerequisites

Before starting, ensure you have:
* Python 3.10 or higher.
* AWS credentials configured.
* Amazon Bedrock AgentCore SDK installed.

### Step 1: IAM Permissions

There are two different AWS Identity and Access Management (IAM) permissions to consider:
1.  **Developer Role:** Used by you to create AgentCore resources.
2.  **Execution Role:** Needed by the agent to run within the AgentCore Runtime.

*Note: The execution role can now be automatically created by the AgentCore Starter Toolkit (`auto_create_execution_role=True`).*

### Step 2: Add a Wrapper to Your Agent

As shown in the Deep Agents code snippet above, add the AgentCore imports and decorators to your existing agent code.

### Step 3: Deploy Using the AgentCore Starter Toolkit

The starter toolkit provides a three-step deployment process:

```python
from bedrock_agentcore_starter_toolkit import Runtime

# Step 1: Configure
agentcore_runtime = Runtime()
config_response = agentcore_runtime.configure(
    entrypoint="hello.py", # contains the code we showed earlier in the post
    execution_role=role_arn, # or auto-create
    auto_create_ecr=True,
    requirements_file="requirements.txt",
    region="us-west-2",
    agent_name="deepagents-research"
)

# Step 2: Launch
launch_result = agentcore_runtime.launch()
print(f"Agent deployed! ARN: {launch_result['agent_arn']}")

# Step 3: Invoke
response = agentcore_runtime.invoke({
    "prompt": "Research the latest developments in quantum computing"
})
```

### Step 4: What Happens Behind the Scenes

When you run the deployment, the toolkit automatically:
1.  Creates an optimized **Docker file** with a Python 3.13-slim base image and OpenTelemetry instrumentation.
2.  Builds your container with dependencies from `requirements.txt`.
3.  Creates an **Amazon Elastic Container Registry (Amazon ECR)** repository and pushes your image.
4.  Deploys to AgentCore Runtime and monitors deployment status.
5.  Configures networking and **observability** with Amazon CloudWatch and AWS X-Ray integration.

The entire process typically takes 2–3 minutes. Each new session is launched in its own fresh AgentCore Runtime microVM.

---

## Invoking Your Deployed Agent

Once deployed, you have two options to call your agent:

**Option 1: Using the Starter Toolkit**

```python
response = agentcore_runtime.invoke({
    "prompt": "Research the latest developments in quantum computing"
})
```

**Option 2: Using the Boto3 SDK Directly**

```python
import boto3
import json

agentcore_client = boto3.client('bedrock-agentcore', region_name='us-west-2')

response = agentcore_client.invoke_agent_runtime(
    agentRuntimeArn=agent_arn,
    qualifier="DEFAULT",
    payload=json.dumps({
        "prompt": "Analyze the impact of AI on healthcare in 2024"
    })
)

# Handle streaming response
for event in response['completion']:
    if 'chunk' in event:
        print(event['chunk']['bytes'].decode('utf-8'))
```

---

## Deep Agents in Action

When executing in Bedrock AgentCore Runtime, the main agent **orchestrates** specialized sub-agents. In this case, the orchestrator's prompt sets the plan:

1.  Write the question to `question.txt`.
2.  Scatter into one or more research agent calls (each for a unique sub-topic) using the `internet_search` tool.
3.  Synthesize findings into `final_report.md`.
4.  Call the **critique-agent** to evaluate gaps and structure.
5.  Optionally loop back for further research/editing.


*(Click on the drawing to play video)*

---

## Cleanup

When finished, don't forget to de-allocate the provisioned AgentCore Runtime and the container repository:

```python
agentcore_control_client = boto3.client(
    'bedrock-agentcore-control', region_name=region
)
ecr_client = boto3.client('ecr', region_name=region)

runtime_delete_response = agentcore_control_client.delete_agent_runtime(
    agentRuntimeId=launch_result.agent_id
)

response = ecr_client.delete_repository(
    repositoryName=launch_result.ecr_uri.split('/')[1],
    force=True
)
```

---

## Conclusion

Amazon Bedrock AgentCore represents a **paradigm shift** in how we deploy AI agents. By abstracting infrastructure complexity while maintaining framework flexibility, it allows developers to focus on building sophisticated agent logic rather than managing deployment pipelines.

Our Deep Agents implementation proves that even complex multi-agent systems can be deployed with minimal code changes. The combination of enterprise-grade security, **built-in observability**, and **serverless scaling** makes AgentCore the premier choice for production AI agent deployment.

Specifically for deep research agents, AgentCore offers unique capabilities:
* **Asynchronous Processing:** Handle long-running agents (up to 8 hours) without blocking responses.
* **AgentCore Memory:** Maintain complex investigation context without losing progress between sessions.
* **AgentCore Gateway:** Extend research to include proprietary insights from enterprise data sources via MCP tools.

**Ready to deploy?**
1.  Install the toolkit: `pip install bedrock-agentcore-starter-toolkit`
2.  Experiment: Deploy your code by following the step-by-step guide.

The era of production-ready AI agents has arrived. With AgentCore, the journey from prototype to production has never been shorter.

---

### About the Authors

**Vadim Omeltchenko** is a Sr. AI/ML Solutions Architect, passionate about helping AWS customers innovate in the cloud.

**Eashan Kaushik** is a Specialist Solutions Architect for AI/ML at Amazon Web Services. He is driven by creating advanced generative AI solutions while prioritizing a customer-centric approach.

**Shreyas Subramanian** is a Principal Data Scientist, helping customers solve business challenges using Machine Learning on the AWS platform.

**Mark Roy** is a Principal Machine Learning Architect for AWS, helping customers design and build generative AI solutions. He leads solution architecture efforts for Amazon Bedrock launches.
