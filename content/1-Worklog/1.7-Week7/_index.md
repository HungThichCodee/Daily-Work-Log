---
title: "Week 7 Worklog"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---


### Week 7 Objectives:

* Security Hub: Group and manage security alerts centrally.
* IAM Condition: Strictly limit access permissions by time frame and IP address.
* IAM Boundary: Set a permission ceiling to block privilege escalation errors.
* VPC Endpoints: Create a private network path to access S3 without going through the Internet.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                        | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 2   | - Practice AWS Security Hub:<br>&emsp; + Enable AWS Security Hub feature.<br>&emsp; + Group security alerts into a single dashboard.<br>&emsp; + View system security score.<br>&emsp; + Clean up resources. | 01/06/2026 | 01/06/2026      | <https://000018.awsstudygroup.com/> |
| 3   | - Manage permissions with IAM Role & Condition:<br>&emsp; + Create basic Groups and Users.<br>&emsp; + Configure Switch role for Admin.<br>&emsp; + Set up permission lock to only allow access from specific IPs.<br>&emsp; + Lock permissions to only allow access by time frame.<br>&emsp; + Clean up resources. | 02/06/2026 | 02/06/2026      | <https://000044.awsstudygroup.com/> |
| 4   | - Block permissions with IAM Permission Boundary:<br>&emsp; + Create Restriction Policies.<br>&emsp; + Attach this permission boundary to User.<br>&emsp; + Try logging in and test if User is blocked.<br>&emsp; + Clean up resources. | 03/06/2026 | 03/06/2026      | <https://000030.awsstudygroup.com/> |
| 5   | - Private S3 Connection (Part 1 - VPC):<br>&emsp; + Create Gateway Endpoint connection port.<br>&emsp; + Connect to S3 directly from within VPC network.<br>&emsp; + Ensure data is not leaked to the Internet.<br>&emsp; + Clean up resources. | 04/06/2026 | 04/06/2026      | - |
| 6   | - Private S3 Connection (Part 2 - On-premises):<br>&emsp; + Prepare and simulate internal network (On-premises).<br>&emsp; + Create Interface Endpoint connection port.<br>&emsp; + Use DNS to safely access S3 from internal network.<br>&emsp; + Clean up resources. | 05/06/2026 | 05/06/2026      | - |


### Week 7 Achievements:

* Security Hub:
  * Successfully enabled the service and viewed the security score.

* IAM Condition:
  * Successfully configured forcing users to switch roles at the correct IP and time.

* IAM Boundary:
  * Successfully attached maximum permission limits, Users cannot arbitrarily exceed permissions.

* VPC Endpoints:
  * Successfully created internal network ports (Gateway and Interface) safely connecting to S3.
