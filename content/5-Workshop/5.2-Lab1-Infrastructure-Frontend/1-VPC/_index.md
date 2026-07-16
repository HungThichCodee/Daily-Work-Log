---
title: "1. Configure VPC"
weight: 1
chapter: false
pre: " <b> 5.2.1. </b> "
---


In this section, we will create and configure the Virtual Private Cloud (VPC) for our infrastructure. The VPC serves as the network foundation for securely deploying services like EC2.

## Objectives
- Create a custom VPC with a CIDR block of `10.0.0.0/16`.
- Configure 2 Public Subnets and 2 Private Subnets across 2 Availability Zones.
- Set up an Internet Gateway (IGW) to allow internet access for the Public Subnet.
- Set up a NAT Gateway to allow outbound internet access for the Private Subnet.
- Set up an S3 Gateway Endpoint for private access to S3 from within the VPC.

## Step 1: Create the VPC

1. Log in to the **AWS Management Console**.
2. Search for **VPC** in the search bar and select **VPC**.
3. In the left navigation pane, select **Your VPCs**.
4. Click the **Create VPC** button in the top right corner.
5. Configure the following settings to match the 2 provided images:
   - **VPC settings**: Select **VPC and more** (this creates subnets, route tables, and gateways together).
   - **Name tag auto-generation**: Enter `genzite`.
   - **IPv4 CIDR block**: `10.0.0.0/16`.

![Create VPC Step 1](./images/create-vpc-step1.png)

   - **Number of Availability Zones (AZs)**: `2`.
   - **Number of public subnets**: `2`.
   - **Number of private subnets**: `2`.
   - **NAT gateways ($)**: Select **Zonal** and **In 1 AZ** (This creates a NAT Gateway for the Private Subnet).
   - **VPC endpoints**: Select **S3 Gateway**.
   - **DNS options**: Ensure both **Enable DNS hostnames** and **Enable DNS resolution** are checked.

![Create VPC Step 2](./images/create-vpc-step2.png)

6. Review the configuration in the preview pane on the right and click **Create VPC**.

### Create NAT Gateway (If done manually)
If you chose **None** for NAT gateways during VPC creation (to save initial costs) and want to create it manually in later Labs, follow these steps:

1. Go to the **VPC** service, select **NAT gateways** from the left menu.
2. Click **Create NAT gateway**.
3. Configure the following settings:
   - **Name**: `genzite-nat-gw`
   - **Availability mode**: `Zonal`
   - **Subnet**: Select your Public Subnet (e.g., `genzite-subnet-public1-us-east-1a`)
   - **Connectivity type**: `Public`
   - **Elastic IP allocation ID**: Click the **Allocate Elastic IP** button

![Create NAT Gateway](./images/create-nat-gw.png)

4. Click **Create NAT gateway** and wait a few minutes for the status to change to **Available**.
*(Note: If created manually, you must go to your Private Subnet's Route Table and add a route for `0.0.0.0/0` pointing to the newly created NAT Gateway).*

## Step 2: Verify Network Resources

The creation process will take a few minutes as AWS needs to provision the resources. Once finished, verify the following:

1. **Subnets**: Go to **Subnets** in the left menu and ensure you have 2 Public Subnets and 2 Private Subnets associated with your `genzite-vpc`.
2. **Internet Gateways**: Go to **Internet Gateways** and ensure 1 IGW is in the **Attached** state to your `genzite-vpc`.
3. **NAT Gateways**: Go to **NAT Gateways** and ensure 1 NAT Gateway is in the **Available** state.
4. **Route Tables**: Go to **Route Tables**.
   - Ensure there is 1 Route Table for the Public Subnets (shared across both AZs, with a route `0.0.0.0/0` pointing to the Internet Gateway).
   - Ensure there are 2 Route Tables for the Private Subnets (one per AZ, each with a route `0.0.0.0/0` pointing to the NAT Gateway, and a route to the S3 Gateway Endpoint).

## Step 3: Enable Auto-assign public IPv4 address

To ensure EC2 instances launched in the Public Subnet automatically receive a Public IP, we need to enable the auto-assign setting on the Public Subnet.

1. In the VPC Dashboard, select **Subnets**.
2. Select your **Public Subnet** (e.g., `genzite-subnet-public1-us-east-1a`).
3. Click on **Actions** -> **Edit subnet settings**.
4. Check the box for **Enable auto-assign public IPv4 address**.
5. Click **Save**.

---
**Module 1 Complete!** You have successfully established a secure network foundation for the workshop. Next, we will move on to configure Security (IAM Roles & Security Groups).
