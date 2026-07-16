---
title: "Lab 4: CloudWatch Monitoring"
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

## Overview

In this section, we will configure and use **Amazon CloudWatch** to monitor the resources of the Genzite system, specifically viewing the EC2 Backend logs and tracking operational metrics.

> **Permission Note:** Similar to previous IAM permission management, setting up monitoring on CloudWatch is a task within the scope of **User C (Application & Storage)**.

## Step 1: Initialize and View Dashboard
1. Open the AWS Management Console and search for the **CloudWatch** service.
2. On the left menu, access tools like Log groups or Metrics.

## Step 2: View Application Logs
1. On the left menu, select **Logs** -> **Log groups**.
2. Find the Log Group for the EC2 server (pushed via the CloudWatch Agent).
3. Click on the corresponding Log stream to check the incoming logs (Errors, Request information).

## Step 3: Create an Alarm (Automated Alert)
1. On the left menu, select **Alarms** -> **In alarm**.
2. Click **Create alarm**.
3. Select the metric **CPUUtilization** (CPU usage level) of the EC2 Backend.
4. Set the alert threshold (e.g., `> 80%` for 5 continuous minutes).
5. (Optional) Configure notifications via Amazon SNS to send an email to the administrator when the system is overloaded.

![Create CloudWatch Alarm](/images/CloudWatch/create_cloudwatch.png)

---
**Congratulations!** You have successfully set up the "monitoring center" for Genzite. Thanks to CloudWatch, the team can easily detect and handle issues.
