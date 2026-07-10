---
title: "Week 10 Worklog"
date: 2024-01-01
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---



### Week 10 Objectives (22/06/2026 – 28/06/2026):
* Test all APIs belonging to the Identity and AI Service modules.
* Complete the Backend (BE) part of the project for these 2 services.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                        | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 2   | - **Complete Identity Service (Auth & Users):**<br>&emsp; + Finalize authentication module with `@nestjs/jwt`, `passport-jwt` and password hashing with `bcryptjs`.<br>&emsp; + Restructure database schema with Prisma (Prisma Migrate/Seed) for Users module.<br>&emsp; + Integrate Email sending system (Forgot password/Verify) using `Nodemailer`. | 22/06/2026 | 22/06/2026      | - |
| 3   | - **Complete AI Service (Core Logic):**<br>&emsp; + Successfully integrate multiple language models (LLM): `@google/generative-ai` (Gemini) and `openai`.<br>&emsp; + Finalize Pipeline, Generation logic and Agent orchestration.<br>&emsp; + Apply Model Context Protocol (MCP) to analyze context (Context Memory). | 23/06/2026 | 23/06/2026      | - |
| 4   | - **Handle Asynchronous & Event-driven processing:**<br>&emsp; + Configure and optimize Background Jobs for AI Service using `BullMQ` (Workers).<br>&emsp; + Integrate Message Broker (`@genzite/kafka`) to synchronize data (Events) between Identity Service and AI Service. | 24/06/2026 | 24/06/2026      | - |
| 5   | - **System and Unit Testing:**<br>&emsp; + Write tests (Jest / Supertest) for Auth, Mail (Identity) and Recruitment, Generation (AI Service) modules.<br>&emsp; + End-to-End test the entire request flow, ensuring requests sent to the BullMQ queue are processed stably without timeouts. | 25/06/2026 | 25/06/2026      | - |
| 6   | - **Packaging and Documentation:**<br>&emsp; + Review and standardize `Dockerfile.dev` for both microservices.<br>&emsp; + Update detailed API documentation for the Frontend team.<br>&emsp; + Report on the completion and acceptance of the Identity and AI Service modules. | 26/06/2026 | 26/06/2026      | - |


### Week 10 Achievements:

* API Testing:
  * Tested and debugged all APIs, ensuring security and correct business response on the AWS environment.

* Identity Service:
  * Integrated JWKS Verification on EC2 server to authenticate JWT tokens issued by Cognito.
  * Deployed Database structure via Prisma to manage Users, RBAC, Config on RDS PostgreSQL.
  * Completed Kafka Message Broker flow and Email sending system (Nodemailer).

* AI Service:
  * Configured secure Outbound flow from EC2 via NAT Gateway to communicate with external AI.
  * Integrated Redis caching to store AI Responses, optimizing latency and saving costs.
  * Completed secure upload link generation logic allowing clients to upload files to S3.
  * Applied BullMQ as Background Workers and Model Context Protocol (MCP).
