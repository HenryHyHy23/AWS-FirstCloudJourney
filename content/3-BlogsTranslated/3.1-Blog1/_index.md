---
title: "Blog 1"
date: 2025-09-23
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Artificial Intelligence: Integrating Tokenization with Amazon Bedrock Guardrails for Secure Data Processing

Written by **Nizar Kheir** and **Mark Warner** | **September 23, 2025** | Category: **Amazon API Gateway, Amazon Bedrock Guardrails, AWS Lambda, AWS Step Functions, Partner solutions, Technical How-to** |

This post is co-written by Mark Warner, Principal Solutions Architect for Thales, Cyber Security Products.

As Generative AI applications move into production environments, they integrate with a wider scope of business systems that handle **sensitive customer data**. This integration introduces new challenges around protecting **Personally Identifiable Information (PII)** while maintaining **data reversibility** for downstream applications that legitimately require it. Consider a financial services company deploying generative AI across different departments. The customer service team needs an AI assistant that can access customer records and provide personalized responses including contact information. Meanwhile, the fraud analysis team requires the same customer data but must analyze patterns without exposing the actual PII, working only with protected representations of the sensitive information.

**Amazon Bedrock Guardrails** helps detect sensitive information, such as PII, in a standard format within input prompts or model responses. Sensitive information filters give organizations control over how sensitive data is handled, with options to block requests containing PII or to **mask** sensitive information with generic placeholders like `{NAME}` or `{EMAIL}`.

While masking effectively protects sensitive information, it introduces a new challenge: the **loss of data reversibility**. When guardrails replace sensitive data with generic masks, the original information becomes inaccessible to downstream applications that might need it for legitimate business processes.

**Tokenization** offers a complementary approach to this challenge. Unlike masking, tokenization replaces sensitive data with **format-preserving tokens** that are mathematically unrelated to the original information but still maintain its structure and usability. These tokens can be safely **reversed** back to their original value when needed by authorized systems, creating a path for secure data flow throughout an organization's environment.

In this post, we show you how to integrate Amazon Bedrock Guardrails with third-party tokenization services to protect sensitive data while maintaining data reversibility. The solution described demonstrates how to combine Amazon Bedrock Guardrails with tokenization services from the **Thales CipherTrust Data Security Platform** to create an architecture that protects sensitive data without sacrificing the ability to process that data securely when needed.

---

## Amazon Bedrock Guardrails APIs

Amazon Bedrock Guardrails offers two distinct approaches for implementing content safety controls:

* **Direct integration** with model invocation via APIs like `InvokeModel` and `Converse`.
* **Independent evaluation** via the **ApplyGuardrail API**, which decouples guardrail assessment from model invocation.

This post utilizes the **ApplyGuardrail API** for tokenization integration because it separates the content assessment from the model call, allowing the insertion of the tokenization process between these steps.

The solution extends a typical `ApplyGuardrail` API deployment by inserting the tokenization processing between the guardrail assessment and the model invocation, as follows:

1.  The application calls the **ApplyGuardrail API** to assess the user input for sensitive information.
2.  If sensitive information is detected (`action = "ANONYMIZED"`):
    * The application captures the detected PII and its location.
    * It calls a tokenization service to convert these entities into **format-preserving tokens**.
    * It replaces the generic guardrail masks with these tokens.
3.  The application then calls the foundation model with the tokenized content.

---

## Solution Overview

To illustrate how this workflow provides value in practice, consider a financial advisory application. Three separate application components work together to provide secure, AI-powered financial insights:

* **Customer gateway service:** This trusted interface coordinator receives customer queries that often contain sensitive information.
* **Financial analysis engine:** This AI-powered component analyzes financial patterns and generates recommendations but does not need access to the actual customer PII. It operates with anonymized or tokenized information.
* **Response processing service:** This trusted service handles the final communication with the customer, including **detokenizing** sensitive information before presenting the result to the customer.



The workflow is orchestrated by **AWS Step Functions** and uses **AWS Lambda** functions to sequence PII detection, tokenization, AI model invocation, and detokenization across the three main components:

1.  The **Customer gateway service** receives user input $\rightarrow$ Calls **ApplyGuardrail API**.
2.  Guardrail detects PII $\rightarrow$ The gateway service calls the **tokenization service** (e.g., Thales CipherTrust) to generate **format-preserving tokens**.
3.  The input with tokenized values (e.g., `[[TOKEN_123]]`) is passed to the **Financial analysis engine**.
4.  The analysis engine calls an **LLM on Amazon Bedrock** to generate financial advice using the tokenized data.
5.  The model response (which may contain tokens) is sent to the **Response processing service**.
6.  This service calls the tokenization service to **detokenize**, restoring the original sensitive values.
7.  The final, detokenized response is sent to the customer.

This architecture maintains data security throughout the processing flow while preserving the utility of the information.

---

## Prerequisites

To deploy the described solution, you must have the following components configured in your environment:

* An **AWS account** with **Amazon Bedrock** enabled.
* Appropriate **AWS Identity and Access Management (IAM) permissions**: `bedrock:CreateGuardrail`, `bedrock:ApplyGuardrail`, and `bedrock-runtime:InvokeModel`.
* A **Python 3.7+ environment** with the `boto3` library and configured AWS credentials.
* A deployed **tokenization service** accessible via REST API endpoints (e.g., Thales CipherTrust Data Security Platform).

---

## Amazon Bedrock Guardrails Configuration

Use the AWS SDK to configure the Guardrail with a sensitive information policy to **ANONYMIZE** PII types such as `URL`, `EMAIL`, and `NAME` for both inputs and outputs:

```python
import boto3
def create_bedrock_guardrail():
    """
    Create a guardrail in Amazon Bedrock for financial applications with PII protection.
    """
    bedrock = boto3.client('bedrock')
    
    response = bedrock.create_guardrail(
        name="FinancialServiceGuardrail",
        description="Guardrail for financial applications with PII protection",
        sensitiveInformationPolicyConfig={
            'piiEntitiesConfig': [
                {'type': 'URL', 'action': 'ANONYMIZE', 'inputAction': 'ANONYMIZE', 'outputAction': 'ANONYMIZE', 'inputEnabled': True, 'outputEnabled': True},
                {'type': 'EMAIL', 'action': 'ANONYMIZE', 'inputAction': 'ANONYMIZE', 'outputAction': 'ANONYMIZE', 'inputEnabled': True, 'outputEnabled': True},
                {'type': 'NAME', 'action': 'ANONYMIZE', 'inputAction': 'ANONYMIZE', 'outputAction': 'ANONYMIZE', 'inputEnabled': True, 'outputEnabled': True}
            ]
        },
        blockedInputMessaging="I can't provide information with PII data.",
        blockedOutputsMessaging="I can't generate content with PII data."
    )
    return response
```

---

## Tokenization Workflow Integration

### 1. Apply Guardrails to Detect PII Entities

Use the **ApplyGuardrail API** to validate the input text:

```python
def invoke_guardrail(user_query):
    #... (ApplyGuardrail API call logic)
    bedrock_runtime = boto3.client('bedrock-runtime')
    response = bedrock_runtime.apply_guardrail(
        guardrailIdentifier='your-guardrail-id', 
        guardrailVersion='your-guardrail-version', 
        source="INPUT",
        content=[{"text": {"text": user_query}}]
    )
    return response
```

### 2. Invoke the Tokenization Service

Parse the response from the Guardrail and call the third-party tokenization service (e.g., Thales CipherTrust) to convert the sensitive values into tokens.

```python
import requests
def thales_ciphertrust_tokenizer(guardrail_response):
    #... (logic to extract PII entities)
    #... (logic to call Thales CipherTrust /v1/protect endpoint)
    # Get sensitive_value and call API
    # response = requests.post(url_str, headers=headers, data=json.dumps(crdp_payload))
    # protected_results.append({...})
    #...
    pass
```

### 3. Replace Guardrail Masks with Tokens

Replace the generic guardrail masks (`{EMAIL}`, `{URL}`, etc.) in the guardrail's output with the generated format-preserving tokens.

```python
def process_guardrail_output(protected_results, guardrail_response):
    #... (replacement logic)
    protection_map = {res['type'].upper(): res['protection_response']['protected_data'] for res in protected_results}
    modified_outputs = []
    for output_item in guardrail_response.get('outputs', []):
        if 'text' in output_item:
            modified_text = output_item['text']
            for pii_type, protected_value in protection_map.items():
                modified_text = modified_text.replace(f"{{{pii_type}}}", protected_value) # Replace {PII_TYPE} with token
            modified_outputs.append({"text": modified_text})
    return modified_outputs
```

**Example of Sanitized Input:**
* **Original Input:** `"Hi, this is john.smith@example.com. Based on my last five transactions on acme.com..."`
* **Tokenized (Sanitized) Input:** `"Hi, this is 1001000GC5gDh1.D8eK71@EjaWV.lhC. Based on my last five transactions on 1001000WcFzawG.Jc9Tfc..."`

### 4. Downstream Application Processing

The financial analysis engine can call the LLM with the tokenized data to generate a relevant response. The **response processing service** then calls the tokenization service's **Detokenize API** (e.g., Thales CipherTrust) to **detokenize** the tokens and restore the original values before sending the final response to the customer.

---

## Cleanup

To avoid incurring additional charges, delete the AWS resources you created after testing:

1.  Delete the guardrails you created (refer to the Amazon Bedrock documentation on deleting a guardrail).
2.  Delete the AWS Lambda, API Gateway, or Step Functions resources if you deployed the full workflow.
3.  Refer to the documentation for your third-party tokenization solution (e.g., Thales CipherTrust) for instructions on appropriate resource removal.

---

## Conclusion

Combining **Amazon Bedrock Guardrails** with **tokenization** (e.g., Thales CipherTrust) provides a robust solution for protecting **PII** within generative AI workflows while preserving the necessary **data utility and reversibility** for authorized downstream applications. This approach helps organizations in tightly regulated industries balance generative AI innovation with compliance requirements.