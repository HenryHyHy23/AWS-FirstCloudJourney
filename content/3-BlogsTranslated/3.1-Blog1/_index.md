---
title: "Blog 1"
date: 2026-11-19
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# WHEN AI CAN CHANGE THE INTERFACE: HOW DOES GENERATIVE UI WORK?

**What if each user could see an interface suited to their needs?**

As AI models become more capable, I find **Generative UI** an interesting direction: instead of giving everyone the same fixed layout, an application can adapt its interface to a user's request and context.

For example, one customer may want to browse food images, while a restaurant buyer needs to compare prices and packaging quickly. The same product list could prioritize cards for the first customer and a table for the second.

**How is Generative UI different from a typical chatbot?**

A chatbot usually responds with text or speech. With Generative UI, the response can also include a suitable presentation: a comparison table, product cards, filters, or a chart. I can still interact with AI conversationally, but the result does not have to remain inside a chat box.

- **AI understands intent:** A user might say, “Show me the products in a table so I can compare prices.”
- **The application updates the interface:** AI suggests a presentation, and the application updates the interface and DOM through existing components.
- **A more personal experience:** The same products can appear as a table or as cards, depending on what helps the user.
- **The system remains responsible for data:** Prices, inventory, and order information should come from APIs and databases, not be invented by AI.
- **Safety boundaries matter:** The backend should validate AI output and user permissions before applying changes or invoking important functions.

On AWS, I can explore **Amazon Bedrock** to connect to an AI model and **Bedrock Guardrails** to help assess content. Guardrails add a layer of protection, but they still need testing with real data.

**How can Generative UI work with A2UI 1.0?**

- First, I define an A2UI **catalog** listing supported components, properties, actions, and functions. For example, it can declare `ProductTable` and a `searchProducts` operation.
- A customer asks: “Show me a table comparing pork prices.” The frontend sends the request to a backend agent, which can use a model through Amazon Bedrock and supplied business tools to obtain relevant data.
- The **A2UI Agent SDK** can generate model instructions from the catalog, then parse and validate the model output. It produces structured A2UI messages such as `updateComponents` and `updateDataModel`.
- A frontend **renderer** receives the messages and maps catalog entries to components implemented by the application. With React, React updates the DOM according to the resulting UI state.

In this example, the model can suggest a product table; the renderer displays a registered component with data supplied by the application. The application integrates the model, agent, and business APIs; A2UI does not create those pieces automatically.

I think this makes the interface more flexible while keeping it manageable: AI suggests **how to present information**, and the application decides which components and actions are allowed.

**How does AI know which API a component uses?**

The catalog describes components and functions, while the agent can also receive separately declared **business tools** for interacting with APIs. I do not need to let the model discover every API in the system.

- To find products, the agent can propose the declared `searchProducts` tool; the backend validates its arguments and executes the corresponding API logic.
- When the user clicks a button, A2UI can send an **event** to the agent or invoke a registered **function**. A function may run on the renderer or agent, depending on its declaration.
- The result can update data or the interface. Mapping to APIs and enforcing business rules remain the responsibility of application code.

**Does AI have access to every API and all data?**

I treat AI as a backend-integrated capability, not as something that automatically inherits all backend permissions. I expose only the tools needed for each task, and the backend must authenticate the user and check their role and access to each resource on every call. API keys and user tokens should not be included in prompts; only data needed for the task should be sent to the model.

For example, AI may suggest searching products, but it must not read another customer's orders. Operations such as ordering or payment still require backend checks and appropriate user confirmation. Guardrails help assess content but do not replace API authorization.

I learned about this approach from the A2UI 1.0 documentation and its Quickstart guide (links at the end of this post). The catalog and SDK help control UI structure, but do not replace authentication, authorization, or API security.

**How does Amazon Bedrock Guardrails help protect the AI flow?**

Guardrails adds content checks before or after a model call. I can configure it to help detect and filter harmful content, unwanted topics, or sensitive information such as email addresses and phone numbers. Depending on the policy, the system can block content or mask sensitive data.

For example, if a customer accidentally includes a phone number in a product request, I can configure a check to limit sending it to the model. Detection is probabilistic, so I still need to test actual data and languages. Guardrails cannot guarantee every case is detected and does not replace API authorization.

**How can AWS services help?**

- **Amazon Bedrock:** Connects the application to an AI model that interprets requests and suggests interface configurations.
- **Amazon Bedrock Guardrails:** Helps assess input and output against configured policies.
- **Amazon ECS and AWS Fargate:** Run the backend in containers while Fargate reduces server-management work.
- **Amazon S3 and Amazon CloudFront:** Store and deliver frontend files to users.
- **Amazon CloudWatch:** Tracks logs and metrics to help identify errors or performance issues.

I think these services offer a clear separation of responsibilities: Bedrock handles AI, the backend controls data and permissions, and infrastructure services help deploy and monitor the application. Not every system needs all of them; the choice depends on requirements and cost.

**What should I consider before deployment?**

- **Minimize personal data:** Provide the model only with information needed to handle the request.
- **Validate AI output:** Do not assume generated JSON is always correct; check its structure and allowed options.
- **Keep transactions under application control:** Ordering and payment still need the appropriate APIs, business rules, and user confirmation.
- **Monitor cost and response time:** Call volume and token usage affect cost, and the application should handle slow or unavailable AI responses.

I think Generative UI opens up opportunities for more flexible and engaging interfaces. The key is to let AI support user needs while the application remains in control of data and actions.

**References:**

- [What is Amazon Bedrock?](https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html)
- [Carry out a conversation with the Converse API](https://docs.aws.amazon.com/bedrock/latest/userguide/conversation-inference.html)
- [Amazon Bedrock Guardrails](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails.html)
- [Sensitive information filters in Guardrails](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails-sensitive-filters.html)
- [A2UI 1.0](https://a2ui.org/)
- [A2UI Quickstart guide](https://a2ui.org/quickstart/)
