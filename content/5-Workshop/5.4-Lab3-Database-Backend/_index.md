---
title: "Lab 3: Database & Backend"
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---


Welcome to **Lab 3**. In this module, we will build the data storage layer (Database) and the compute server (Backend) to handle our application logic.

## Overview

The Genzite system needs to store user information, generation history, and JSON layouts of websites. Separating the Database from the Backend, and placing the Database in a Private Subnet is an AWS Security Best Practice.

In this lab, you will:
- Deploy a secure **Amazon RDS PostgreSQL** relational database.
- Provision and configure an **Amazon EC2** instance to run the backend API (Node.js/NestJS).
- Configure an **Application Load Balancer (ALB)** to securely distribute internet traffic to the EC2 instance.

## Step-by-Step Instructions

Lab 3 consists of the following sections:

- **[1. Initialize RDS PostgreSQL](1-database-rds/)**: Configure a managed database.
- **[2. Deploy Backend on EC2](2-backend-ec2/)**: Provision a server, set up the environment, and run the API.
- **[3. Configure Load Balancer](3-load-balancer/)**: Set up an ALB to route HTTP traffic to EC2.

---
Let's get started with: **[Initialize RDS PostgreSQL](1-database-rds/)**.
