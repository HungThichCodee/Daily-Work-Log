---
title: "Clean Up"
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

Congratulations on completing the Genzite Workshop on AWS! 🎉

To avoid incurring unexpected charges (especially for services outside the Free Tier like NAT Gateways, ALBs, and RDS), the resource cleanup step is critical. Please follow the exact order below to ensure everything is deleted successfully without dependency conflicts.

## Step 1: Delete Compute and Load Balancing (ALB)

1. **EC2 Instances**:
   - Go to the **EC2 Dashboard**.
   - Select the `genzite-backend` instance -> **Instance state** -> **Terminate instance**.
2. **Application Load Balancer (ALB)**:
   - In the EC2 menu, scroll down to **Load Balancing**, select **Load Balancers**.
   - Select the ALB `genzite-alb` -> **Actions** -> **Delete**.
3. **Target Groups**:
   - Go to **Target Groups** and delete the target groups you created (`genzite-backend-tg`, `frontend-tg`).

## Step 2: Delete Databases

1. **Amazon RDS PostgreSQL**:
   - Go to the **RDS Dashboard**.
   - Select **Databases**, select the `genzitedb` instance -> **Actions** -> **Delete**.
   - Uncheck "Create final snapshot" and check the acknowledgment box. Type `delete me` to confirm.
   - Go to **Subnet groups** in the left menu, and delete the `genzite-subnet-rds` subnet group.

## Step 3: Delete Amazon Cognito

1. Go to the **Cognito Dashboard**.
2. Select **User pools**.
3. Choose the user pool you created (e.g., `genzite-user-pool`) -> **Delete**. Confirm the user pool name to finalize the deletion.

## Step 4: Delete Monitoring Resources (CloudWatch & SNS)

1. **CloudWatch Alarms**:
   - Go to the **CloudWatch Dashboard**, select **Alarms** -> **All alarms**.
   - Select the CPU Alarm you created -> **Actions** -> **Delete**.
2. **CloudWatch Log Groups**:
   - Select **Logs** -> **Log groups**.
   - Find and delete the EC2 log group.
3. **Amazon SNS (If any)**:
   - Go to the **SNS Dashboard**, select **Topics**.
   - Select the alert topic you created -> **Delete**. Type `delete me` to confirm.

## Step 5: Delete Frontend and Media (CloudFront & S3)

1. **CloudFront**:
   - Go to the **CloudFront Dashboard**.
   - Select your Distribution, click **Disable** (this takes about 3-5 minutes).
   - Once the status changes to Disabled, select the distribution again and click **Delete**.
2. **Amazon S3**:
   - Go to the **S3 Dashboard**.
   - Perform the following for both buckets: the Frontend Bucket (e.g., `workshop-frontend-app-12345`) and the Media Bucket (`genzite-media-bucket`).
   - Click **Empty** to delete all files inside (you need to type `permanently delete` to confirm).
   - Once the bucket is empty, click **Delete** to remove the bucket itself.

## Step 6: Delete AWS Backup Resources

1. **AWS Backup Plans**:
   - Go to the **AWS Backup Dashboard**, select **Backup plans**.
   - Select the backup plan you created (e.g., `genzite-backup-plan`) -> **Delete**.
2. **Recovery Points**:
   - Select **Backup vaults**, click on the Vault you created (e.g., `genzite-backup-vault`).
   - Select all Recovery points inside -> **Delete** (type to confirm deletion).
3. **AWS Backup Vaults**:
   - Once the Vault is empty, select the Vault and click **Delete**.

## Step 7: Delete Networking (VPC & Security Groups)

*Note: You must delete the NAT Gateway and release the Elastic IP first to avoid ongoing charges.*

1. **NAT Gateway**:
   - Go to the **VPC Dashboard**, select **NAT gateways**.
   - Select `genzite-nat-gw` -> **Actions** -> **Delete NAT gateway**. (Wait a few minutes for the status to become Deleted).
2. **VPC Endpoints**:
   - Go to **Endpoints**, delete the S3 Gateway Endpoint.
3. **VPC**:
   - Go to **Your VPCs**, select `genzite` (or `genzite-vpc`).
   - Click **Actions** -> **Delete VPC**.
   - This will automatically delete the associated Subnets, Route Tables, Internet Gateway, and Security Groups.

---
**All done!** You have successfully cleaned up your AWS environment for this Workshop. See you in the next Cloud project!
