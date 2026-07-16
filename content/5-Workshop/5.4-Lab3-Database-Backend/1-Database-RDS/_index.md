---
title: "1. Initialize RDS"
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---


The Genzite system requires a place to store structured data (User information, created Web Projects, JSON layout structures). In the AWS environment, the most optimal service is **Amazon Relational Database Service (RDS)**.

To save costs for this Lab and meet the MVP (Minimum Viable Product) architecture, we will choose PostgreSQL running on a `db.t3.micro` instance and place it in a Private Subnet.

## Step 1: Create a DB Subnet Group

Before creating the RDS instance, we must tell AWS which Subnets the Database is allowed to reside in. Following Security Best Practices, we will only place the DB in a Private Subnet.

1. Open the **RDS** service in the AWS Console.
2. From the left menu, select **Subnet groups**.
3. Click **Create DB subnet group**.
4. **Name**: `genzite-subnet-rds`.
5. **Description**: `genzite-subnet-rds`.
6. **VPC**: Select `genzite-vpc`.
![Create Subnet Group](./images/5.4.1.1.png)
7. Scroll down to the **Add subnets** section:
   - Choose **Availability Zones**: Select `us-east-1a` and `us-east-1b`.
   - Choose **Subnets**: Select 2 **Private Subnets**.
![Subnet Group](./images/5.4.1.2.png)
8. Click **Create**.
![Create Done](./images/5.4.1.3.png)
## Step 2: Provision Database Instance

1. From the left menu, select **Databases**, click **Create database** and choose **Full Configuration**.
![Create RDS](./images/5.4.1.4.png)
2. **Engine options**: Choose **PostgreSQL** (Version `PostgreSQL 16.14-R2`).
3. **Templates**: Choose **Sand box**.
4. **Settings**:
   - **DB instance identifier**: `genzitedb`.
   - **Master username**: `genzite_admin`.
   - **Credentials management**: Choose **Self managed**.
   - **Master password**: Enter a strong password and confirm it in the **Confirm master password** box.
![Credentials](./images/5.4.1.6.png)
![Authentication](./images/5.4.1.7.png)

5. **Instance configuration**:
   - Instance type: Choose `db.t3.micro`.
6. **Storage**:
   - **Storage type**: Choose **General Purpose SSD (gp2)**.
   - **Allocated storage**: `30` GiB.
   - Uncheck **Enable storage autoscaling** (in the Additional storage configuration section if available).

7. **Connectivity**:
   - **Compute resource**: Choose **Don't connect to an EC2 compute resource**.
   - **Network type**: Choose **IPv4**.
![Storage & Connectivity](./images/5.4.1.8.png)
   - **Virtual private cloud (VPC)**: Select `genzite-vpc`.
   - **DB Subnet Group**: Select `genzite-subnet-rds`.
   - **Public access**: Choose **No** (The Database must not be accessible from the Internet).
   - **VPC security group (firewall)**: Choose **Choose existing**, remove the `default` tag, and select `genzite-rds-sg` (Created in Lab 1 - Security).
   ![Public Access](./images/5.4.1.5.png)

8. **Database authentication**: Choose **Password authentication**.
9. **Monitoring**:
   - **Database Insights**: Choose **Database Insights - Standard**.
   - **Performance Insights**: Uncheck **Enable Performance Insights**.
   - **Enhanced Monitoring**: Uncheck **Enable Enhanced monitoring**.
![Monitoring](./images/5.4.1.9.png)

10. Expand the **Additional configuration** section:
    - **Database options**:
      - Enter **Initial database name**: `genzite`. *(If you leave this blank, RDS will not create a default Database for you)*.
      - **DB parameter group**: Select `default.postgres16`.
    - **Encryption**: Uncheck **Enable encryption**.
    - **Backup**:
      - Check **Enable automated backup**.
      - **Backup retention period**: Select `1 day`.
![Additional configuration](./images/5.4.1.10.png)

11. Review the information, scroll down to the bottom, and click **Create database**.

## Step 3: Retrieve the Connection Endpoint

The Database initialization process may take 5-10 minutes.

1. Once the Database status changes to `Available`, click on the name `genzite-db`.
2. In the **Connectivity & security** tab, find the **Endpoint** section.
3. Copy this **Endpoint** URL (e.g., `genzite-db.xxxxxxxxx.us-east-1.rds.amazonaws.com`).

You will need this Endpoint along with the Username, Password, and Database Name (`genzite`) to configure the EC2 Backend server in the next step.
