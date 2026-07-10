---
title: "Week 11 Worklog"
date: 2024-01-01
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---



### Week 11 Objectives:

* CloudFormation & FSx: Know the theory of automated infrastructure deployment (IaC) and how to manage Windows Server file storage systems.
* Container Operations: Clearly understand the lifecycle of managing Docker applications on an ECS cluster and strategies for smooth version updates.
* CI/CD Automation: Evaluate the continuous CI/CD deployment flow from source code to container to minimize manual errors.
* Monitoring & Security: Grasp how to visualize monitoring data and securely access servers without opening SSH ports.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                        | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 2   | - Analyze AWS CloudFormation:<br>&emsp; + Learn the concept of Infrastructure as Code (IaC).<br>&emsp; + Analyze how to use code to model and automatically create AWS resources.<br>&emsp; + Evaluate advanced features: Custom Resources, Mappings, Stacksets. | 29/06/2026 | 29/06/2026      | <https://000037.awsstudygroup.com/> |
| 3   | - Research Amazon FSx for Windows:<br>&emsp; + Analyze Windows Server file storage architecture running on SMB protocol.<br>&emsp; + Learn the automatic data replication mechanism (Multi-AZ).<br>&emsp; + Evaluate data deduplication and capacity allocation features. | 30/06/2026 | 30/06/2026      | <https://000025.awsstudygroup.com/> |
| 4   | - Analyze Amazon ECS architecture:<br>&emsp; + Learn how ECS manages and runs Docker containers on an EC2 cluster.<br>&emsp; + Analyze Task Definition and how it integrates with Application Load Balancer.<br>&emsp; + Evaluate zero-downtime deployment strategies (Blue/Green, Rolling). | 01/07/2026 | 01/07/2026      | <https://000016.awsstudygroup.com/> |
| 5   | - Learn CI/CD Pipeline for ECS:<br>&emsp; + Analyze CI/CD flow that helps automate the deployment process.<br>&emsp; + Evaluate integration between ECS with Gitlab, Github Actions and CodeBuild.<br>&emsp; + Research how to push container logs to S3 via Firelens. | 02/07/2026 | 02/07/2026      | <https://000017.awsstudygroup.com/> |
| 6   | - Evaluate System Monitoring & Management:<br>&emsp; + Analyze Grafana's ability to visualize monitoring data.<br>&emsp; + Learn how to securely connect to EC2 using System Manager - Session Manager.<br>&emsp; + Evaluate the mechanism of storing session history into Amazon S3. | 03/07/2026 | 03/07/2026      | <https://000029.awsstudygroup.com/><br><https://000058.awsstudygroup.com/> |


### Week 11 Achievements:

* CloudFormation & FSx:
  * Understood how to automate the construction of AWS resources and clearly grasped the components of highly available Windows file storage architecture.

* ECS Container:
  * Clearly distinguished the roles of Cluster, Task Definition, Service, and the load balancing flow with Load Balancer.

* CI/CD Flow:
  * Mastered the theory of automated deployment from source code repositories (Gitlab/Github/CodeBuild) straight to containers.

* Management & Monitoring:
  * Understood the visual log analysis mechanism of Grafana and the process of setting up VPC Endpoints to manage Private EC2 securely via Session Manager.
