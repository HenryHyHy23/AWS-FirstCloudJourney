---
title: "Container & Storage"
menuTitle: "Container & Storage"
weight: 5
chapter: false
pre: "<b> 5.5. </b> "
---



Finish this phase with a backend image URI/digest and a private bucket containing the storefront index.html and assets.

- [5.5.1. Amazon ECR](5.5.1-amazon-ecr/) — Private repository containing the Docker image consumed by ECS Fargate.
- [5.5.2. Amazon S3](5.5.2-amazon-s3/) — Stores static compiled frontend files; product and avatar media use Cloudinary instead.
