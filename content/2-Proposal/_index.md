---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Genzite: AI-Powered No-Code Web Interface Builder
## A Cloud-Native AWS Infrastructure Solution for AI-Driven Frontend Generation

### 1. Executive Summary
Genzite is an AI No-Code platform that enables non-technical users to create and operate fully functional web interfaces simply by describing their requirements in natural language. Rather than restricting users to fixed templates, Genzite leverages **Google Gemini AI** to automatically parse a user prompt and generate a structured JSON layout, which is then rendered into a live React interface directly in the browser.

The system is deployed entirely on AWS with a clear separation of concerns: the **React SPA Frontend** is stored on **Amazon S3** and distributed globally via **Amazon CloudFront**, while the **NestJS backend API** running on **EC2** handles AI orchestration, async job queue management, and data persistence into **Amazon RDS PostgreSQL**. User authentication is managed by **Amazon Cognito**. AI web generation tasks are processed **asynchronously** via **BullMQ + Amazon ElastiCache Redis**, with real-time progress streamed back to the client over **SSE**. Internal events between services are propagated via **Apache Kafka**.

The project scope is focused on the core delivery loop: **User submits prompt → Job queued in BullMQ → AI Worker calls Gemini to generate JSON layout → System saves & renders UI → User edits canvas**.

---

### 2. Problem Statement
#### What's the Problem?
Building a custom website requires professional knowledge of HTML, CSS, and JavaScript—along with significant budget for developer resources. Popular drag-and-drop tools (WordPress, Wix, Squarespace) are easy to use but constrained by fixed layouts, limited customization depth, and no ability to automatically generate interface designs from plain text descriptions.

Non-technical users currently have no tool that allows them to:
- Describe a website in plain language and receive a complete interface immediately
- Visually edit each UI component directly on a canvas
- Manage their own website content without writing any code

#### The Solution
Genzite addresses this with a four-step process:

1. **AI Layout Generation (Async)**: The user submits a prompt. The API immediately pushes a **Job** into the **BullMQ** queue (backed by **Amazon ElastiCache Redis**) and returns a `jobId` instantly—without blocking the user. An **AI Worker** running in the background pops the job, calls the **Google Gemini API** to generate the JSON layout, persists the result, and emits a completion event.
2. **Progress Tracking (SSE)**: The Frontend opens a **Server-Sent Events** connection using the `jobId`. When the AI Worker completes, the SSE stream pushes a `100% Done` notification with the layout to the client.
3. **Render & Edit**: The Frontend consumes the JSON layout and renders an interactive canvas. Users drag, drop, and edit each widget visually.
4. **Cross-Service Events (Kafka)**: When a new site is saved, the **Site Service** publishes a `SiteCreated` event to **Apache Kafka**. Downstream services (e.g., Notification Service) consume this event to send a welcome email—without blocking the main creation flow.

All React static assets are stored on **S3** and served via **CloudFront** for fast global delivery. JSON layout data is persisted in **RDS PostgreSQL** via the backend API on **EC2**.

#### Benefits
- **Zero coding required**: Users can produce a professional-looking web interface without any programming knowledge.
- **Speed**: From prompt submission to a fully rendered interface in **under 5 minutes**.
- **Low operational cost**: By serving static assets via S3 + CloudFront and keeping EC2 usage minimal for API work, the MVP infrastructure cost is estimated at only **~$30–$50/month**.

---

### 3. Solution Architecture

#### AWS Services Used

| AWS Service | Role in the System |
|---|---|
| **Amazon Route 53** | Custom domain management and DNS routing to CloudFront |
| **Amazon CloudFront** | Global caching and delivery of the React SPA, SSL/TLS via ACM |
| **Amazon S3** | Hosts all static React application assets (JS, CSS, HTML) |
| **Amazon Cognito** | User account authentication and JWT token issuance |
| **Application Load Balancer (ALB)** | Receives API requests, performs SSL termination, routes to EC2 |
| **Amazon EC2** | Runs the NestJS API server for AI orchestration and data persistence |
| **Amazon RDS PostgreSQL** | Stores JSON layout data and website metadata per user |
| **Amazon ElastiCache Redis** | Runs the BullMQ job queue for AI Workers and caches duplicate prompt results |
| **NAT Gateway** | Allows private-subnet EC2 instances to safely reach the Gemini API |
| **AWS Certificate Manager** | Automatic SSL/TLS certificate provisioning and renewal |

#### Component Design

The backend consists of **4 core services**, communicating via **Apache Kafka** (event bus) and offloading heavy tasks to **BullMQ** workers:

- **Identity Service**: Integrates with Amazon Cognito for user registration, login, and JWT issuance. Manages per-user workspaces.
- **Site Service**: Reads and writes website configuration (JSON layout: page list + widget array) to/from PostgreSQL. Publishes a `SiteCreated` event to Kafka after each successful save.
- **AI Service**: Receives prompt → pushes a **BullMQ Job** → AI Worker calls **Google Gemini API** to generate the JSON layout → persists result → emits SSE `completed` event to Frontend. Caches prompt hashes in Redis to avoid redundant LLM calls.
- **Notification Service**: Consumes `SiteCreated` events from Kafka to automatically send a welcome email to the user—without impacting the main site creation flow.

---

### 4. Technical Implementation

#### Technical Requirements

| Component | Technology |
|---|---|
| **Frontend** | React 18, Vite, TypeScript, Tailwind CSS v4 |
| **Canvas Editor** | Custom drag-and-drop widget system (15+ widget types) |
| **Backend API** | NestJS, Prisma ORM, PostgreSQL |
| **Message Queue** | BullMQ + Amazon ElastiCache Redis (async AI job processing) |
| **Event Bus** | Apache Kafka (inter-service communication) |
| **Authentication** | Amazon Cognito (JWT) |
| **AI Engine** | Google Gemini 2.0 Flash API |
| **Infrastructure** | AWS EC2, S3, RDS, ElastiCache, CloudFront, ALB, Route 53 |

#### Core AI Web Generation Flow

```
User submits prompt
        │
        ▼
Frontend sends POST /api/v1/ai/generate
        │
        ▼
AI Service pushes Job into BullMQ (ElastiCache Redis)
        │
        ▼
API returns HTTP 202 Accepted + jobId (non-blocking)
        │
        ▼
Frontend opens SSE stream (/api/v1/ai/stream/:jobId)
        │
        ▼  [Background: AI Worker pops job from BullMQ]
 AI Worker calls Google Gemini API → generates JSON Layout
        │
        ▼
Site Service persists JSON to RDS PostgreSQL
        │  → publishes SiteCreated event to Kafka
        │     → Notification Service sends welcome email
        ▼
SSE stream pushes '100% Done' + layout to Frontend
        │
        ▼
Frontend renders canvas UI
        │
        ▼
User edits widgets directly on the canvas
```

#### Sample JSON Layout Structure

```json
{
  "siteName": "Coffee Shop Website",
  "subdomain": "coffee-shop",
  "pages": [
    {
      "slug": "/",
      "title": "Home",
      "widgets": [
        { "type": "HeroBanner", "props": { "title": "Welcome to Our Coffee Shop", "bgColor": "#3E2723" } },
        { "type": "ProductGrid", "props": { "columns": 3, "items": [] } },
        { "type": "ContactForm", "props": { "email": "hello@coffee.com" } }
      ]
    }
  ]
}
```

#### Implementation Phases

1. **Phase 1 – Infrastructure & Authentication Setup (Weeks 1–2)**
   - Configure VPC, Security Groups, and public/private subnets
   - Create S3 bucket and CloudFront distribution with OAI
   - Set up Amazon Cognito User Pool and App Client
   - Deploy NestJS API on EC2, connect to RDS PostgreSQL

2. **Phase 2 – AI Integration & Canvas Editor (Weeks 3–4)**
   - Build AI Service: receive prompt, call Gemini API, return JSON
   - Build Site Service: CRUD JSON layout in PostgreSQL
   - Develop React Canvas Editor (JSON-driven widget renderer)
   - Integrate Identity Service (Cognito JWT) across all flows

3. **Phase 3 – Testing & Launch (Weeks 5–6)**
   - End-to-end testing: login → submit prompt → render canvas → save
   - Security Group audit (ALB → EC2 only; EC2 → RDS only)
   - Deploy React production build to S3, configure CloudFront cache behaviors
   - Configure Route 53 to point domain to CloudFront distribution

---

### 5. Timeline & Milestones

| Milestone | Timeline | Deliverable |
|---|---|---|
| **Architecture Design** | Month 0 | System diagram, database ERD, VPC allocation |
| **Core Infrastructure & Auth** | Weeks 1–2 | EC2 + RDS + Cognito + CloudFront operational |
| **AI Generation & Canvas** | Weeks 3–4 | Prompt → JSON → canvas render flow complete |
| **Testing & Launch** | Weeks 5–6 | System live, domain configured, end-to-end tested |
| **Optimization** | Post-launch | CloudFront cache hit monitoring, RDS index tuning |

---

### 6. Budget Estimation (Monthly ~ 730 hours)

Based on the system architecture diagram, below is the estimated monthly cost (using `ap-southeast-1` Singapore region pricing) for 2 configurations: **Minimal MVP Configuration** (cost-saving, suitable for demo/project) and **Full Configuration** (production-ready as in your architecture diagram).

| AWS Component | Minimal MVP / Project Configuration | Full Configuration (Based on Architecture Diagram) |
|---|---|---|
| **Compute (EC2 & EBS)** | Single `t4g.small` in Public Subnet (~$12–$15/month) | Single `t4g.small` in Private Subnet (~$12–$15/month) |
| **Load Balancer (ALB)** | ❌ Direct access to EC2 IP (~$0) | ✅ ALB + ACM Certificate (~$20–$25/month) |
| **NAT Gateway** | ❌ Not used (~$0) | ✅ Required for Private Subnet EC2 internet access (~$42–$48/month) |
| **Database (RDS)** | Single-AZ PostgreSQL `db.t4g.micro` (~$18–$20/month) | Single-AZ PostgreSQL `db.t4g.micro` (~$18–$20/month) |
| **Cache (Redis)** | ❌ Redis installed directly on EC2 (~$0) | ✅ Dedicated ElastiCache Redis (~$15–$18/month) |
| **Security (WAF)** | ❌ Not used (~$0) | ✅ AWS WAF for Web Traffic filtering (~$6–$10/month) |
| **Static Storage (S3)** | ✅ S3 Frontend & Media bucket (~$1–$3/month) | ✅ S3 Frontend & Media bucket (~$1–$3/month) |
| **CloudFront & Route53** | ✅ CloudFront Free Tier + DNS (~$0–$1/month) | ✅ CloudFront + Route53 Hosted Zone (~$2–$5/month) |
| **Other Services** | Cognito, IAM, Backup (~$0) | Cognito, AWS Backup, CloudWatch (~$2–$5/month) |
| **Estimated Total** | **~$31–$40 / month** | **~$104–$128 / month** |

- **Budget Calculator Link**: [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=58e0506ec76a24dacd2cc6990c65981eba461c97)

---

### 7. Risk Assessment

#### Risk Matrix

| Risk | Probability | Impact | Mitigation Strategy |
|---|---|---|---|
| **Gemini API returns malformed JSON** | High | High | Build a JSON Schema validator in the AI Service. Auto-retry with a corrective prompt on failure. |
| **Gemini API Rate Limit exceeded** | Medium | High | Cache identical prompt results. Implement per-user rate limiting in NestJS. |
| **RDS performance degradation at scale** | Low | Medium | Add indexes on `site_id` and `user_id` columns. Cap maximum JSON layout size per page. |
| **High AI response latency (5–15s)** | High | Medium | All AI processing runs asynchronously via BullMQ. Frontend shows real-time progress via SSE stream instead of blocking on an HTTP response. |
| **SSE connection dropped mid-generation** | Low | Medium | Client auto-reconnects SSE. Job result is cached in Redis so the client can retrieve it upon reconnect. |

#### Contingency Plans
- If Google Gemini is unavailable, the BullMQ worker automatically retries the job (up to 3 times with exponential backoff) before surfacing an error to the user.
- If ElastiCache Redis is unavailable, the system can temporarily fall back to synchronous processing to maintain availability.
- All AWS infrastructure configurations (VPC, Security Groups, S3 policies) are documented as scripts for rapid environment rebuild if needed.

---

### 8. Expected Outcomes

- **Automatic UI Generation**: Users submit a plain-text site description and receive a fully rendered canvas interface within 5 minutes.
- **Visual Editing**: The canvas editor enables drag-and-drop editing of every widget—no code required.
- **Standards-Compliant AWS Deployment**: The full system runs on a secure AWS architecture (private subnets, Cognito auth, HTTPS everywhere), with high-performance static delivery via CloudFront CDN and managed reliability via RDS.
- **Cost-Optimized Operations**: The MVP configuration operates reliably under $50/month, making it ideal for the demo and initial evaluation phase.
