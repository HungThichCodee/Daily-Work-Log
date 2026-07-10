---
title: "Week 8 Worklog"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---



### Week 8 Objectives:

* Architecture Orientation: Clearly understand how components in the system interact with each other from the Internet to the Database.
* Service Decomposition: Identify and fully list all necessary AWS services from the diagram.
* Modeling: Manually redraw and clarify the data flow to present the project.
* Budget Estimation: Create a monthly operational cost estimate close to reality.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                        | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 2   | - Project research and orientation:<br>&emsp; + Read and understand user authentication flow (Cognito).<br>&emsp; + Analyze static web access flow (CloudFront & S3).<br>&emsp; + Analyze dynamic data processing flow (Route 53 & ALB).<br>&emsp; + Orient security and backup requirements. | 08/06/2026 | 08/06/2026      | - |
| 3   | - List related AWS services:<br>&emsp; + Network & Delivery: VPC, IGW, NAT Gateway, ALB, Route 53, CloudFront.<br>&emsp; + Security & Identity: WAF, Amazon Cognito.<br>&emsp; + Compute & Storage: EC2, EBS, S3 (Frontend & Media).<br>&emsp; + Data & Management: RDS, Media/Cache, AWS Backup, CloudWatch. | 09/06/2026 | 09/06/2026      | - |
| 4   | - Draw architecture flow diagram (Part 1 - Network & Frontend):<br>&emsp; + Draw user flow calling to Route 53 and Cognito.<br>&emsp; + Draw static flow: CloudFront combined with WAF and S3 Frontend.<br>&emsp; + Plan VPC, divide Public/Private Subnets. | 10/06/2026 | 10/06/2026      | - |
| 5   | - Draw architecture flow diagram (Part 2 - Backend & Management):<br>&emsp; + Draw dynamic flow: ALB routing into Private Subnet (EC2).<br>&emsp; + Connect EC2 with DB (RDS), Cache, EBS and S3 Media.<br>&emsp; + Complete monitoring (CloudWatch) and Backup flow. | 11/06/2026 | 11/06/2026      | - |
| 6   | - Calculate project costs:<br>&emsp; + Determine expected configuration parameters for each service.<br>&emsp; + Use AWS Pricing Calculator to input data.<br>&emsp; + Export monthly cost estimate table (Monthly Cost). | 12/06/2026 | 12/06/2026      | - |


### Week 8 Achievements:

* Analysis Document:
  * Mastered the role of each cluster (Static Frontend, Dynamic Backend, Authentication, Monitoring).

* Service List:
  * Have a detailed layout of services (VPC, EC2, RDS, Cognito, WAF...) ready for deployment.

* Complete Diagram:
  * The Architecture Flow diagram has been clearly redrawn, separating each component.

* Cost Spreadsheet:
  * Completed the Estimate Cost file to report/evaluate feasibility.
