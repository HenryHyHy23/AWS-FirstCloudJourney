---
title: "Event 3"
date: 2026-10-24
weight: 1
chapter: false
pre: " <b> 4.3. </b> "
---

# Event Report: MEET UP

The October 24, 2026 meetup brought together Vietnamese Voice AI, multi-account resource management, security testing, and AI agent operations. Its five sessions offered different perspectives on applying AI to practical work, from conversational experience to context management, cost, and execution permissions.

### Event Information

| Item     | Details                                             |
| -------- | --------------------------------------------------- |
| Date     | October 24, 2026                                    |
| Time     | 09:00–12:00 (UTC+7)                                 |
| Location | Floor 26, Bitexco Financial Tower, Ho Chi Minh City |
| Role     | Attendee                                            |

### Purpose of the Event

- Explore how Voice AI and AI agents support practical use cases.
- Connect multi-account resource sharing, security testing, and permission controls with system development and operations.
- Understand Runtime, Memory, context windows, cost, and guardrails when agents support ongoing work.

### Featured Speakers

- **Kiệt Trần** – AI Engineer, Revve AI.
- **Long Huỳnh** – Styl Solution.
- **Thịnh Nguyễn** – Styl Solution.
- **Xuân An** – Leader, AWS Student Builder Group HCMUTE.
- **Khoa Kiều** – Tech Lead, ACB.
- **Nghi Danh** – AWS Community Builder.

### Key Content & Highlights

#### Session 1: Hear, Think, Speak – Building a Vietnamese voice-AI contact center on AWS

**Use case and demo workflow**

The AI initiates a Vietnamese phone call to conduct an approximately **three-minute** Cloud Engineer screening interview. The broader contact-center problem involves peak call volumes, repetitive requests, and call-quality assessment.

The interview flow shown on the slide was:

**Greeting → Experience → Two technical questions → Candidate questions → Goodbye.**

A **Reschedule** node supports arranging another call. Post-call outputs include a transcript, summary, and score. The demo slide highlighted situations to watch: Vietnamese mixed with terms such as “EKS”, “subnet”, and “auto scaling”; candidate interruptions; and attempts to influence the assessment, such as asking the bot to pass the candidate outright.

**Three approaches to voice agents**

| Approach | Processing flow | Trade-off presented |
| --- | --- | --- |
| Cascade | ASR/STT → LLM → TTS | Intermediate text provides a checkpoint before speech output; latency passes through several stages. |
| Turn-based speech-to-speech | Audio → speech model → audio | Fewer intermediate stages and retained vocal information; output controls have different intervention timing from a cascade. |
| Full-duplex speech-to-speech | Listening and speaking simultaneously | Supports backchannels, overlap, and more flexible turn-taking; the speaker's comparison highlighted greater control and deployment complexity. |

**Anatomy of a call**

A SIP trunk brings the call to an EC2 SIP server and a WebRTC media server. Voice workers on EKS handle **noise cancellation → VAD/turn detection → STT → LLM and flow engine → TTS**, then return audio through the media server.

The presented deployment uses one Region and a multi-AZ VPC in the customer's account. STT/TTS include self-hosted GPU components, and the LLM layer has a Bedrock connection through PrivateLink. RDS, ElastiCache, and S3 support data, cache/state, and storage, with CRM/core-banking integration through APIs.

**Latency, Vietnamese, and guardrails**

- **Identify the measurement:** The approximately 200 ms figure on the slide describes human turn-taking. **0.3–1.8 seconds** refers to end-of-turn waiting, not the bot's complete response time. Model selection and conversation design need to account for the whole pipeline.
- **Intentional interruption handling:** The slide described approximately 0.3 seconds of caller speech to trigger barge-in, distinguishing interruptions from brief acknowledgments such as “vâng” and “dạ”.
- **Benchmark in Vietnamese:** Turn detection needs evaluation in the actual language; a padding mismatch can cut callers off. Normalizing numbers and forms such as “500K” before TTS also affects the listening experience.
- **Check before speaking:** In the slide's six-case test, a gateway guard let callers hear the beginning of an unwanted sentence in 5/6 cases; checking each sentence before TTS produced 0/6. These are results from that test set, illustrating why guardrail placement matters.

**Banking POC**

A separate POC used **Greeting → Verification → Intent → API action → Ticket/human handoff**. The speaker reported **799 test calls in one month**, **93% intent match across 871 labeled turns**, and **35/35 outbound outcomes correctly classified**. These figures belong to the banking POC and should be read within its evaluation scope.

#### Session 2: AWS Resource Access Manager

- Resource sharing across accounts in AWS Organizations, combining centralized network administration with cost optimization.
- Notes cover the interaction between IAM policies and SCPs in controlling permissions across multiple accounts.

#### Session 3: AWS Pentest with AWS Continuum / Security Agent

- Early infrastructure security assessment through Terraform reviews and checks integrated into GitLab/GitHub pull requests.
- A Security Agent acts as a user to perform automated pentesting in SIT/UAT before production deployment.
- Tool names and the integration flow will be clarified in the detailed session notes.

#### Session 4: Amazon Bedrock AgentCore

- The two components noted for taking agents into production were Agent Runtime and Memory.
- Runtime notes mention MicroVMs and EC2 Spot alongside cost optimization; their deployment relationship will be checked against the session details.
- Memory notes describe synchronous short-term and asynchronous long-term handling for user context; the specific processing flow will be added later.

#### Session 5: Kiro Crew

Kiro Crew focused on agents running around the clock to support code reviews and system checks, with a Q&A on practical operational concerns.

- **Artifacts:** A demo collected real job-market data and generated statistical charts directly in the chat.
- **Context window:** Notes record Auto Compact at approximately **70–90%**, as recalled from the session, and **Break down context** for inspecting token usage by component. The specific threshold and mechanism will be expanded with the session details.
- **Memory:** Removing old memories and managing their lifecycle to control costs.
- **Guardrails:** Limiting agent actions to prevent unauthorized infrastructure deletion and exposure of server secrets or credentials.

### Knowledge and Key Takeaways

- **Design around the workflow:** A screening call needs clear question stages, a rescheduling path, and post-call outputs that support the next step.
- **Evaluate the whole conversation:** Distinguish end-of-turn waiting from complete response latency; test Vietnamese turn detection, interruptions, and TTS, and check content before audio is played.
- **Connect resource governance with access:** The AWS RAM session linked shared resources with permission scope across multiple accounts.
- **Include security checks throughout development:** Terraform reviews and SIT/UAT pentesting illustrated two different assessment points: reviewing a change and testing a running environment.
- **Consider the agent's whole lifecycle:** Runtime, memory, context windows, and guardrails all shape ongoing agent support. Kiro Crew brought these concerns together in the discussion of efficiency and autonomy.

### Experience at the Event

The live demo of an AI calling a candidate for an approximately three-minute Vietnamese Cloud Engineer screening was a memorable highlight. The flow from greeting and experience to technical questions showed how Voice AI can support a specific recruitment stage. Interruptions, English technical terms, and attempts to influence scoring also drew attention to conversation handling alongside voice quality.

Kiro Crew was the session I found most valuable, especially the closing technical Q&A. Questions about Artifacts, tokens, memory, and execution permissions connected directly with the idea of agents supporting code reviews and system checks around the clock while keeping actions and costs under control.
