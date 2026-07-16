---
title: "Prerequisites"
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---


To ensure a smooth workshop experience, you need to prepare a few basic requirements regarding your AWS account and environment.

## 1. AWS Account

You need an AWS account to create resources such as VPC, EC2, IAM, etc. If you do not have one, you can sign up for an AWS Free Tier account following the instructions on the AWS homepage.

> [!WARNING]
> Although this Lab is designed to use Free Tier eligible services whenever possible, misconfiguration or using paid resources (such as NAT Gateways) may incur charges. Please make sure to follow the Cleanup steps at the end of the workshop to delete all resources after you finish.

## 2. Select Region

Throughout this Lab, we will use the **US East (N. Virginia) `us-east-1`** region. 

Please ensure you have selected this region from the top right corner of the AWS Management Console before creating any resources to avoid errors and resource isolation issues in later steps.

## 3. Web Browser

It is recommended to use the latest version of modern web browsers (Google Chrome, Firefox, Microsoft Edge, Safari) for the best experience with the AWS Management Console.

## 4. IAM User (Recommended)

Following AWS security best practices, it is highly recommended **not** to use the Root user for everyday tasks and workshops. Please create an IAM User with **AdministratorAccess** permissions and log in using that IAM account to proceed with the lab.

---
**Are you ready?** Once you have successfully logged into the AWS Console with the `us-east-1` region selected, move on to the next section to **Configure VPC**.
