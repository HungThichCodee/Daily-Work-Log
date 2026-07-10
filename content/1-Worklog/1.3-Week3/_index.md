---
title: "Week 3 Worklog"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---


### Week 3 Objectives:

* Use AWS CLI to interact and manage resources instead of just using the console.
* Learn about caching service on AWS (Amazon ElastiCache - Redis).
* Learn about automatically scaling the number of EC2 servers based on demand and load balancing between servers.
* Learn Amazon Route 53 domain name resolution service.
* Get acquainted with NoSQL database through Amazon DynamoDB.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                        | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 2   | - Learn AWS CLI: <br>&emsp; + Install AWS CLI and basic configuration <br>&emsp; + Practice initializing EC2 instance via CLI <br>&emsp; + Clean up resources                                 | 04/05/2026 | 04/05/2026      | <https://000011.awsstudygroup.com/> |
| 3   | - Learn about Amazon ElastiCache (Redis): <br>&emsp; + Create Subnet and Cluster <br>&emsp; + Connect to Node <br>&emsp; + Manipulate data via SDK <br>&emsp; + Clean up resources            | 05/05/2026 | 05/05/2026      | <https://000061.awsstudygroup.com/> |
| 4   | - Learn Auto Scaling and ELB: <br>&emsp; + Create Launch Template <br>&emsp; + Configure Load Balancer <br>&emsp; + Create Auto Scaling Group <br>&emsp; + Try Scaling modes <br>&emsp; + Clean up resources | 06/05/2026 | 06/05/2026      | <https://000006.awsstudygroup.com/> |
| 5   | - Learn Route 53 & Hybrid DNS: <br>&emsp; + Create Inbound/Outbound Endpoints <br>&emsp; + Configure Resolver Rules <br>&emsp; + Check results <br>&emsp; + Clean up resources                | 07/05/2026 | 07/05/2026      | <https://000010.awsstudygroup.com/> |
| 6   | - Get acquainted with Amazon DynamoDB: <br>&emsp; + Learn Core components <br>&emsp; + Create Table & Access key <br>&emsp; + Read/Write/Query data <br>&emsp; + Clean up resources           | 08/05/2026 | 08/05/2026      | <https://000060.awsstudygroup.com/> |


### Week 3 Achievements:

* Know how to use AWS CLI to interact and manage resources.

* Know how to use Auto Scaling and Load Balancing:
  * Deploy Auto Scaling Group so the system can automatically add or remove EC2 servers

* Optimize Database and Caching: 
  * Grasp core knowledge of fully managed Redis cache service
  * Know how to connect and manipulate data with ElastiCache Redis via SDK

* Domain Name and DNS Management
  * Grasp basic concepts of DNS and Route 53, including Hosted Zone, Record, and DNS Resolution
  * Practice configuring Inbound/Outbound Resolver to integrate with internal DNS

* Get acquainted with Amazon DynamoDB: 
  * Grasp core components of DynamoDB, including Table, Item, Attribute, Primary Key
  * Know how to create Table and perform basic operations like read/write/query data
