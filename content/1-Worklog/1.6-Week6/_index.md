---
title: "Week 6 Worklog"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---



### Week 6 Objectives:

* AWS Backup: Learn how to use Machine Learning to detect malware or anomalies in backup data.
* Amazon DLM: Automate the retention of long-term backups (over 90 days) to save costs and comply with regulations.
* IAM Boundary: Limit the maximum permissions of an account to prevent the risk of hackers escalating privileges.
* Service Quotas: Know how to request AWS to relax default resource limits when the system needs it.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                        | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 2   | - Analyze AWS Backup:<br>&emsp; + Evaluate AWS Backup workflow.<br>&emsp; + Integrate with EventBridge, Lambda, DynamoDB.<br>&emsp; + Purpose of automating event processing.<br>&emsp; + Analyze modified block extraction mechanism. | 25/05/2026 | 25/05/2026      | <https://000089.awsstudygroup.com/> |
| 3   | - Analyze Machine Learning:<br>&emsp; + Apply CloudWatch Machine Learning.<br>&emsp; + Detect anomalies from block data.<br>&emsp; + Protect data storage system.<br>&emsp; + Prevent cyber attacks and ransomware. | 26/05/2026 | 26/05/2026      | <https://000089.awsstudygroup.com/> |
| 4   | - Analyze Amazon DLM:<br>&emsp; + Benefits of EBS Snapshots Archive.<br>&emsp; + Keep data long-term over 90 days.<br>&emsp; + Set up automated policies (DLM).<br>&emsp; + Reduce risk of data loss due to human error. | 27/05/2026 | 27/05/2026      | <https://000088.awsstudygroup.com/> |
| 5   | - Analyze IAM Boundary:<br>&emsp; + Permission Boundary concept.<br>&emsp; + Limit maximum permissions of User/Group.<br>&emsp; + Integrate with Identity-based policy.<br>&emsp; + Prevent privilege escalation errors. | 28/05/2026 | 28/05/2026      | <https://000030.awsstudygroup.com/> |
| 6   | - Analyze Service Quotas:<br>&emsp; + Evaluate resource limits (quotas).<br>&emsp; + Learn AWS default values.<br>&emsp; + Evaluate actual system needs.<br>&emsp; + Process of requesting limit increase support. | 29/05/2026 | 29/05/2026      | <https://000063.awsstudygroup.com/> |


### Week 6 Achievements:

* AWS Backup:
  * Understood how the system automatically compares snapshots to find the number of modified data blocks.

* Amazon DLM:
  * Knew how to create automated save/delete policies, eliminating cumbersome scripts that easily cause data loss errors.

* IAM Boundary:
  * Grasped how to combine Permission Boundary with Policies to tightly lock down permissions.

* Service Quotas:
  * Understood the process of looking up quotas and the steps to submit requests to AWS Support to ask for more resources.
