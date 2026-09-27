---
title: "Event 4"
date: 2025-09-09
weight: 1
chapter: false
pre: " <b> 4.4. </b> "
---

# Event Report: AWS Edge Services Workshop

### Purpose of the Event
- **Master Global Content Delivery**: To provide a deep understanding of how to leverage Amazon CloudFront to architect and optimize global content delivery, reducing latency and improving user experience.
- **Fortify Application Security**: To equip attendees with the knowledge and tools to secure web applications against common threats, using AWS WAF to block malicious traffic, mitigate OWASP Top 10 vulnerabilities, and defend against bot attacks.
- **Provide Hands-On, Practical Experience**: To move beyond theory and allow attendees to apply what they’ve learned in a lab environment, implementing optimization and security configurations in real-world scenarios.
- **Build a Secure and Optimized Foundation**: To offer a comprehensive blueprint for using AWS Edge Services as the foundational layer for building robust, scalable, and secure web applications for a global audience.

### List of Speakers
-   **Nguyễn Gia Hưng** - Head of Solutions Architect
-   **Julian Ju** - Senior Edge Services Specialist Solutions Architect
-   **Kevin Lim**

### Key Content & Highlights

#### From Edge to Origin: CloudFront as Your Foundation
- **Focus**: The first major session established the core principles of using a Content Delivery Network (CDN) to accelerate and scale web applications.
- **Content**: The speaker, Nguyễn Gia Hưng, provided a comprehensive overview of Amazon CloudFront. The session covered:
    - **Global Architecture**: How to utilize AWS's global network of edge locations to serve content closer to users.
    - **Core CDN Capabilities**: Caching strategies, optimizing content delivery, and integrating with origin servers like S3 or EC2.
    - **Security at the Edge**: An introduction to security controls available directly within CloudFront to build a first line of defense.
- **Key Moment**: The clear, architectural diagrams showing how a user request is intelligently routed to the nearest edge location, instead of traveling all the way to the origin, was a foundational "aha!" moment. It perfectly illustrated the performance gains possible with a well-configured CDN.
- **Related Session**: "From Edge to Origin: CloudFront as Your Foundation".

#### Attack Surface Defense: WAF & Application Protection
- **Focus**: This session shifted from performance to security, demonstrating how to build a robust defense against a wide array of web-based threats.
- **Content**: Julian Ju delivered a deep dive into the AWS Web Application Firewall (WAF). Key topics included:
    - **Threat Mitigation**: Using AWS WAF to block malicious traffic and mitigate common vulnerabilities like the OWASP Top 10 (e.g., SQL injection, Cross-Site Scripting).
    - **Bot Defense**: Strategies and rulesets for identifying and defending against automated bot attacks that can scrape content, skew analytics, or attempt credential stuffing.
    - **Managed Rules & Customization**: A look at using both AWS Managed Rules for quick protection and how to build custom rules for application-specific threats.
- **Key Moment**: The live demonstration of how easily a WAF rule could be configured to block a simulated SQL injection attack was incredibly powerful. It showed how a complex threat could be neutralized in minutes, providing immediate, tangible security value.
- **Related Session**: "Attack Surface Defense: WAF & Application Protection".

#### From Theory to Practice: Hands-On Workshops
- **Focus**: The entire afternoon was dedicated to practical application, allowing attendees to implement the concepts from the morning sessions in a guided lab environment.
- **Content**: Led by the full team of specialists, the labs were split into two core areas:
    - **Optimization**: Attendees configured a CloudFront distribution, implemented caching policies, and worked through scenarios to optimize content delivery for a sample web application.
    - **Security**: The second lab focused on deploying AWS WAF, applying rule sets to protect against common attacks, and observing how the firewall blocked simulated malicious requests in real-time.
- **Key Moment**: The hands-on security lab was a standout. After learning about threats in the morning, actually configuring the WAF and then running an attack script to see it get blocked provided immense satisfaction and cemented the day's learnings. Discussing real-world implementations with the specialists during the lab was invaluable.
- **Related Sessions**: "Hands On Workshop: Optimize Internet Web Application" & "Hands On Workshop: Secure Internet Web Application".

### What You Learned

#### On Performance & Global Scale
- **Architect for Low Latency**: You learned how to use Amazon CloudFront to design a content delivery strategy that serves users from edge locations around the world, drastically improving your application's load times and responsiveness.
- **Implement Caching Strategies**: You now understand how to configure caching policies to reduce the load on your origin servers, lower data transfer costs, and build a more scalable and resilient application foundation.

#### On Application Security
- **Deploy a Web Application Firewall**: You gained the practical skills to deploy and configure AWS WAF to protect your applications from common vulnerabilities and malicious traffic.
- **Mitigate Common Threats**: You are now equipped to mitigate threats listed in the OWASP Top 10 and defend your application against harmful automated bots, securing your data and your users.

#### On Practical Implementation
- **Bridge Theory and Practice**: Through the hands-on labs, you translated theoretical knowledge about CDNs and WAFs into practical, repeatable skills that you can apply directly to your own projects.
- **Solve Real-World Scenarios**: You worked through realistic scenarios for both optimizing and securing a web application, gaining experience in making trade-offs and implementing effective solutions with expert guidance.

### Experience at the Event
The full-day "AWS Edge Services Workshop" was an intensive and incredibly rewarding masterclass. From the moment it began, there was a clear sense that this was not just a series of lectures, but a cohesive journey through the entire lifecycle of securing and accelerating a modern web application.

#### The Morning: Building the Blueprint
The morning sessions felt like being handed the architectural blueprints for building a digital fortress. The CloudFront session laid the groundwork, showing us how to build a super-fast, global "highway" to our application. Immediately following, the WAF session felt like learning how to construct the impenetrable "walls" and "gatehouses" to protect that highway from invaders. The logical progression from performance to security built a complete and powerful mental model.

#### The Afternoon: Navigating Real-World Complexity
After lunch, the workshop shifted from blueprints to construction. The hands-on labs were where theory became tangible reality. Configuring a CloudFront distribution to speed up a sluggish site and then setting up WAF rules to fend off simulated attacks felt incredibly empowering. It was the difference between seeing a diagram of a shield and actually using one to block a blow. The ability to ask the specialists questions during the lab transformed it from a simple tutorial into a personalized coaching session, making the lessons stick.
