---
title: "Project & Business Overview"
menuTitle: "Overview"
weight: 1
chapter: false
pre: "<b> 5.1. </b> "
---



## What is Bite to Business?

BTB is a **direct food seller** to Customer accounts representing individuals/households or businesses/cooperatives. A Guest may view catalog/prices/reviews; an activated Customer can create purchase Quotes and place Orders after approval. Staff roles are **E-commerce Executive** (catalog, volume pricing, promotions, Quote approval), **Logistics Staff** (stock receipts, packages and trip assignment), **Delivery Staff** (assigned delivery and completion) and **Admin** (account approval, staff roles and operational metrics).

## Core business workflow

```text
Guest/Customer → Register → Admin approves → OTP activation → Login
Customer → Browse catalog/reviews → Select SKU & quantity → Volume price
→ Create Quote → Allocate all quantities to delivery packages
→ E-commerce Executive approves Quote → Customer confirms purchase
→ Backend rechecks all line items and reserves all stock atomically
→ Order → Logistics packs → Payment confirmation (SePay integration)
→ Delivery assignment → Delivery Staff completes actual delivery
```

A **Quote does not reserve stock**. If **any** requested item is short when placing the Order, no partial stock reservation or successful Order should occur. One Order may have multiple delivery packages/destinations, with a single trip for the Order in the MVP. ETA is illustrative, based on a fixed 40 km/h assumption rather than live traffic.

## Technology and responsibility split

| Layer | Technology | Responsibility |
|---|---|---|
| Frontend | React, TypeScript, i18n | Trusted UI, language switching, application behavior |
| Backend | Java, Spring Boot, Spring Security | Validation, roles, pricing, Quote/Order/stock rules, AI action boundaries |
| Database/Auth | Supabase | PostgreSQL and reported auth usage; validate JWT ownership in running code |
| AI | Amazon Bedrock | Model inference; final business authorization remains backend-owned |
| Frontend assets | S3 + CloudFront | Static web build and HTTPS delivery |
| Product and avatar imagery | Cloudinary | Images/media, not frontend build files |
| Payment | SePay | Bank-transfer QR and payment callback; never trust frontend-only success |
| Email | Gmail SMTP + SNS | OTP/application mail vs AWS infrastructure alert mail |

## Learning objectives

By the end of the lab, the team should be able to reconstruct the production architecture using official AWS service icons, trace end-to-end HTTP/CI/CD/telemetry flows, identify IAM/security boundaries, verify each integration and explain operational costs and rollback.
