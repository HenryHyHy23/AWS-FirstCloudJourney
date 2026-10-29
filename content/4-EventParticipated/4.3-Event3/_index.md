---
title: "Event 3"
date: 2026-10-24
weight: 1
chapter: false
pre: " <b> 4.3. </b> "
---

# Event Report: MEET UP

### Event Information

| Item     | Details                                             |
| -------- | --------------------------------------------------- |
| Date     | October 24, 2026                                    |
| Time     | 09:00–12:00 (UTC+7)                                 |
| Location | Floor 26, Bitexco Financial Tower, Ho Chi Minh City |
| Role     | Attendee                                            |

### Purpose

Learn about AI agents, resource sharing, and security checks on AWS.

### Featured Speakers

- **Kiệt Trần** – AI Engineer, Revve AI.
- **Long Huỳnh** – DevSecOps, Styl Solution.
- **Thịnh Nguyễn** – Styl Solution.
- **Xuân An** – Leader, AWS Student Builder Group HCMUTE.
- **Khoa Kiều** – Tech Lead, ACB.
- **Nghi Danh** – AWS Community Builder.

### Sessions

#### Session 1: Hear, Think, Speak – Vietnamese Voice AI

- The demo AI called a Cloud Engineer candidate for a three-minute Vietnamese interview, then produced a transcript, summary, and score.
- Flow: greeting → experience → two technical questions → candidate questions → goodbye.
- The STT → LLM → TTS pipeline needs good Vietnamese handling, interruption support, low latency, and content checks before speech output.

#### Session 2: AWS Resource Access Manager

- Share subnets, Transit Gateways, and Resolver rules across accounts to reuse infrastructure.
- The owner manages the resources; RAM permissions, IAM, and SCPs control access.
- Useful for enterprises with multiple teams/accounts and centralized network management.

#### Session 3: AWS Continuum / Security Agent

- Review designs, Terraform/code, and GitHub/GitLab PRs/MRs; comment on affected lines and suggest patches.
- Pentest SIT/UAT with test credentials, check exploit chains, and return attack graphs and evidence. Retest after fixing issues.
- Account for login flows, business context, and task-hour usage.

#### Session 4: Amazon Bedrock AgentCore

- Runtime supports session-isolated MicroVMs, runtime versions, and DEV/PROD endpoints.
- Memory stores interaction history (STM) and extracts long-term information (LTM) in the background for reuse across sessions.
- Strategies include Summary, User Preferences, Semantic facts, and Episodic records of how tasks were handled.

#### Session 5: Kiro Crew

- Agents support code reviews and system checks around the clock. The demo collected job-market data and created charts as Artifacts in chat.
- Auto Compact condenses context; Break down context shows token usage by component. Old memories need cleanup to control usage.
- Limit permissions to prevent unauthorized infrastructure deletion or exposure of secrets/credentials.

### Takeaways

- Manage context and memory to retain useful information and control token usage.
- Limit agent permissions, check outputs, and retest after fixes.

### My Impressions

The Vietnamese AI interview demo stood out to me. My favourite part was the Kiro Crew Q&A because it got straight to tokens, memory, and agent permissions.

---

![Checked in at the event](/images/4-EventParticipated/4.3-Event3/check-in.png)

_Checked in at the event_
