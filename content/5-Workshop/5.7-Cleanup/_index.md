---
title: "Clean Up"
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

Congratulations on completing the Genzite Workshop on AWS! 🎉

To avoid incurring unexpected charges (especially for services outside the Free Tier like NAT Gateways, ALBs, RDS, and ElastiCache), the resource cleanup step is critical. Please follow the exact order below to ensure everything is deleted successfully without dependency conflicts.

## Step 1: Delete Compute and Load Balancing (ALB)

1. **Auto Scaling Group / EC2 Instances**:
   - Go to the **EC2 Dashboard**.
   - If you used an Auto Scaling Group (ASG), select it and delete it (this will automatically terminate the instances).
   - If you ran standalone EC2 instances (like `genzite-backend-ec2`, `genzite-worker`), select each instance -> **Instance state** -> **Terminate instance**.
2. **Application Load Balancer (ALB)**:
   - In the EC2 menu, scroll down to **Load Balancing**, select **Load Balancers**.
   - Select the ALB `genzite-alb` -> **Actions** -> **Delete**.
3. **Target Groups**:
   - Go to **Target Groups** and delete the target groups you created for the ALB.

## Step 2: Delete Databases and Cache

1. **ElastiCache Redis**:
   - Go to the **ElastiCache Dashboard**.
   - Select **Redis clusters**, choose `genzite-redis` -> **Delete**.
   - Uncheck the option to create a final backup (if prompted) for a faster deletion.
   - Delete the corresponding **Subnet groups** for Redis.
2. **Amazon RDS PostgreSQL**:
   - Go to the **RDS Dashboard**.
   - Select **Databases**, select the `genzite-db` instance -> **Actions** -> **Delete**.
   - Uncheck "Create final snapshot" and check the acknowledgment box. Type `delete me` to confirm.
   - Delete the RDS **Subnet groups**.

## Step 3: Delete Amazon Cognito

1. Go to the **Cognito Dashboard**.
2. Select **User pools**.
3. Choose `genzite-user-pool` -> **Delete**. Confirm the user pool name to finalize the deletion.

## Step 4: Delete Monitoring Resources (CloudWatch & SNS)

1. **CloudWatch Alarms**:
   - Go to the **CloudWatch Dashboard**, select **Alarms** -> **All alarms**.
   - Select the Alarm you created (e.g., `Genzite-High-CPU-Alarm`) -> **Actions** -> **Delete**.
2. **CloudWatch Dashboards**:
   - Select **Dashboards**, choose `Genzite-System-Health` -> **Delete**.
3. **CloudWatch Log Groups**:
   - Select **Logs** -> **Log groups**.
   - Select `genzite-backend-logs` -> **Delete log group**.
4. **Amazon SNS**:
   - Go to the **SNS Dashboard**, select **Topics**.
   - Select the `Genzite-CPU-Alert-Topic` topic -> **Delete**. Type `delete me` to confirm.

## Step 5: Delete Frontend (CloudFront & S3)

1. **CloudFront**:
   - Go to the **CloudFront Dashboard**.
   - Select your Distribution, click **Disable** (this takes about 3-5 minutes).
   - Once the status changes to Disabled, select the distribution again and click **Delete**.
2. **Amazon S3**:
   - Go to the **S3 Dashboard**.
   - Select your Frontend bucket (e.g., `genzite-frontend-bucket-...`).
   - Click **Empty** to delete all files inside (you need to type `permanently delete` to confirm).
   - Once the bucket is empty, click **Delete** to remove the bucket itself.

## Step 6: Delete Networking (VPC & Security Groups)

*Note: You must delete the NAT Gateway and release the Elastic IP first to avoid ongoing charges.*

1. **NAT Gateway**:
   - Go to the **VPC Dashboard**, select **NAT gateways**.
   - Select your NAT Gateway -> **Actions** -> **Delete NAT gateway**. (Wait a few minutes for the status to become Deleted).
2. **Elastic IP**:
   - Go to **Elastic IPs** (Left menu).
   - Select the EIP associated with your NAT Gateway -> **Actions** -> **Release Elastic IP addresses**.
3. **VPC Endpoints**:
   - Go to **Endpoints**, delete the S3 Gateway Endpoint.
4. **VPC**:
   - Go to **Your VPCs**, select `genzite-vpc`.
   - Click **Actions** -> **Delete VPC**.
   - This will automatically delete the associated Subnets, Route Tables, Internet Gateway, and Security Groups.

---
**All done!** You have successfully cleaned up your AWS environment for this Workshop. See you in the next Cloud project!
