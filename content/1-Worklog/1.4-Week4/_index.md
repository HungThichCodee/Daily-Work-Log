---
title: "Week 4 Worklog"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---



### Week 4 Objectives:

* Secure EC2 Management: Use Systems Manager (Session Manager, Patch Manager, Run Command) to connect to servers, manage patches, and automate tasks.
* Static Content Delivery: Host web on S3 and set up CloudFront for security, faster delivery speed.
* System Monitoring: Install Grafana software to visualize and monitor resource metrics.
* Resource Organization: Classify and automate management of large amounts of resources using Tags and Resource Groups.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                        | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 2   | - Learn Amazon Systems Manager - Session Manager:<br>&emsp; + Prepare resources (VPC, EC2, IAM Role)<br>&emsp; + Connect to EC2 instances (Public & Private)<br>&emsp; + Manage session logs & configure Port Forwarding<br>&emsp; + Clean up resources | 11/05/2026 | 11/05/2026      | <https://000058.awsstudygroup.com/> |
| 3   | - Learn AWS Systems Manager:<br>&emsp; + Prepare resources (VPC, Windows EC2, IAM Role)<br>&emsp; + Configure Patch Manager to manage patches<br>&emsp; + Execute commands via Run Command on multiple servers<br>&emsp; + Clean up resources | 12/05/2026 | 12/05/2026      | <https://000031.awsstudygroup.com/> |
| 4   | - Deploy CloudFront with S3 Bucket Origin:<br>&emsp; + Create S3 bucket and upload index.html file<br>&emsp; + Configure Amazon CloudFront to accelerate content delivery<br>&emsp; + Clean up resources | 13/05/2026 | 13/05/2026      | <https://000094.awsstudygroup.com/> |
| 5   | - Getting started with basic Grafana:<br>&emsp; + Prepare environment: VPC, Security Group, EC2 and IAM<br>&emsp; + Install Grafana on Linux EC2<br>&emsp; + Practice resource monitoring<br>&emsp; + Clean up resources | 14/05/2026 | 14/05/2026      | <https://000029.awsstudygroup.com/> |
| 6   | - Manage resources with Tags and Resource Groups:<br>&emsp; + Manage, add/remove Tags via Console and CLI<br>&emsp; + Filter resources based on Tags<br>&emsp; + Create Resource Group based on Tags or CloudFormation<br>&emsp; + Clean up resources | 15/05/2026 | 15/05/2026      | <https://000027.awsstudygroup.com/> |


### Week 4 Achievements:

* Systems Manager:
  * Successfully connected EC2 (Public/Private), configured Port Forwarding, saved session logs. Also used Patch Manager and Run Command for centralized management.

* S3 & CloudFront:
  * Created S3 bucket containing index.html file and successfully configured CloudFront content delivery network (CDN).

* Grafana:
  * Finished preparing the environment, successfully installed Grafana on Linux EC2, and set up monitoring dashboard.

* Tags & Resource Groups:
  * Learned how to attach metadata (Tags) via Console/CLI and create Resource Groups (based on Tags or CloudFormation) to group resources.
