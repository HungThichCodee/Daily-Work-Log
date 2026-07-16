---
title: "1. Initialize RDS"
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---


The Genzite system requires a place to store structured data (User information, created Web Projects, JSON layout structures). In the AWS environment, the most optimal service is **Amazon Relational Database Service (RDS)**.

To save costs for this Lab and meet the MVP (Minimum Viable Product) architecture, we will choose PostgreSQL running on a `db.t4g.micro` instance, placed securely within a Private Subnet.

## Step 1: Create a DB Subnet Group

Before creating the RDS instance, we must tell AWS which Subnets the Database is allowed to reside in. Following Security Best Practices, we will only place the DB in Private Subnets.

1. Open the **RDS** service in the AWS Console.
2. From the left menu, select **Subnet groups**.
3. Click **Create DB subnet group**.
4. **Name**: `genzite-db-subnet-group`.
5. **Description**: `Subnet group for Genzite RDS in Private Subnet`.
6. **VPC**: Select `genzite-vpc`.
7. Scroll down to the **Add subnets** section:
   - Choose **Availability Zones**: Select the AZs where you created your Subnets in Lab 1.
   - Choose **Subnets**: Check the boxes for the **Private Subnets** (Be careful to look at the CIDR block column to ensure you do not accidentally select Public Subnets).
8. Click **Create**.

## Step 2: Provision Database Instance

1. From the left menu, select **Databases** and click **Create database**.
2. **Choose a database creation method**: Select **Standard create**.
3. **Engine options**: Choose **PostgreSQL**.
4. **Templates**: Select **Free tier** (Highly important to avoid incurring large charges).
5. **Settings**:
   - **DB instance identifier**: `genzite-db`.
   - **Master username**: `postgres` (default).
   - **Master password**: Enter a strong password (e.g., `GenziteDBPass123!`). Memorize this password.
6. **Instance configuration**:
   - DB instance class: Select `db.t4g.micro` (Cost-effective ARM-based chip).
7. **Storage**:
   - Allocated storage: `20` GB.
   - Uncheck **Enable storage autoscaling**.
8. **Connectivity**:
   - **Virtual private cloud (VPC)**: Select `genzite-vpc`.
   - **DB Subnet Group**: Choose the `genzite-db-subnet-group` you just created.
   - **Public access**: Select **No** (The database must not be accessible from the Internet).
   - **VPC security group (firewall)**: Choose **Choose existing**, remove the `default` tag, and select `genzite-rds-sg` (Created in Lab 1 - Security).
9. **Database authentication**: Leave default (Password authentication).
10. Expand the **Additional configuration** section:
    - Enter **Initial database name**: `genzite`. *(If you skip this, RDS will not create a default database for you)*.
    - Scroll down and uncheck **Enable automated backups** (To save storage space for the Lab).
11. Review the information, scroll to the bottom, and click **Create database**.

## Step 3: Retrieve the Connection Endpoint

The database initialization process may take 5-10 minutes.

1. Once the Database status changes to `Available`, click on the name `genzite-db`.
2. In the **Connectivity & security** tab, find the **Endpoint** section.
3. Copy this **Endpoint** URL (e.g., `genzite-db.xxxxxxxxx.us-east-1.rds.amazonaws.com`).

You will need this Endpoint along with the Username, Password, and Database Name (`genzite`) to configure the EC2 Backend server in the next step.
