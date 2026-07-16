---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---


#### Overview

This workshop guides you through deploying the **Genzite** platform—an AI No-Code solution that allows users to create website interfaces using natural language prompts.

You will learn how to build the complete Cloud-Native AWS infrastructure required to run the frontend, backend APIs, asynchronous AI workflows, and event-driven notifications.

#### Architecture Highlights
- **Frontend**: Hosted on Amazon S3 and distributed globally via Amazon CloudFront.
- **Backend**: NestJS APIs running on Amazon EC2 with PostgreSQL on Amazon RDS.
- **Asynchronous AI**: Google Gemini integration using BullMQ on Amazon ElastiCache (Redis).
- **Event-Driven**: Apache Kafka for decoupling internal services like notifications.
- **Security**: Amazon Cognito for user authentication.

#### Content

1. [Prerequisites](5.1-Prerequisites/)
2. [Lab 1: Infrastructure & Frontend](5.2-Lab1-Infrastructure-Frontend/)
3. [Lab 2: Cognito Authentication](5.3-Lab2-Cognito-Auth/)
4. [Lab 3: Database & Backend](5.4-Lab3-Database-Backend/)
5. [Lab 4: AI & Asynchronous Processing](5.5-Lab4-AI-Async/)
6. [Lab 5: Event-Driven Architecture](5.6-Lab5-Event-Driven/)
7. [Clean up](5.7-Cleanup/)

![Genzite Architecture](/images/Genzite.drawio.png)