---
title: "Lab 5: AWS Backup"
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

In this Lab, we will configure **AWS Backup** to automatically back up critical resources for the Genzite system (such as RDS). This ensures data safety and disaster recovery capabilities.

## Step 1: Create a Backup Plan and Backup Vault

A Backup Plan defines the automated backup schedule and rules, while a Backup Vault is a secure container where backups (Recovery points) are stored.

1. Open the **AWS Backup** console and click **Create backup plan**.
![Create backup plan](/images/AWS_backup/backup1.jpg)
2. Choose **Build a new plan**. 
   - **Backup plan name**: Enter `genzite-backup-plan`.
   - **Backup rule name**: Enter `daily-backup`.
![Build a new plan](/images/AWS_backup/backup2.jpg)
3. In the **Backup vault** section, choose to create a new vault:
   - **Vault name**: Enter `genzite-backup-vault`.
   - Create the vault.
![Create Vault](/images/AWS_backup/backup3.jpg)
4. Set up the Schedule (Backup window):
   - Change the **Time zone** to `Asia/Saigon (UTC+07:00)` (or your preferred time zone).
![Schedule](/images/AWS_backup/backup4.jpg)
5. Configure Lifecycle:
   - **Total retention period**: Choose `7 Days`.
   - Review the settings and click **Create plan**.
![Lifecycle](/images/AWS_backup/backup5.jpg)

## Step 2: Assign Resources

After successfully creating the Backup Plan, you will be redirected to the Assign resources page to specify which resources will be backed up.

1. Start configuring the resource assignment:
![Assign resources](/images/AWS_backup/backup6.jpg)
2. General settings:
   - **Resource assignment name**: Enter `genzite-resource`.
   - **IAM role**: Select **Default role**.
   - **Define resource selection**: Choose **Include specific resource types**.
![Resource assignment details](/images/AWS_backup/backup7.jpg)
3. Select specific resources:
   - **Resource type**: Select `RDS`.
   - **Database names**: Select `genzite-db`.
   - Finally, click **Assign resources** to complete the automated setup.
![Select resources](/images/AWS_backup/backup8.jpg)

## Step 3: Create an On-demand Backup

In addition to automated backups, you can trigger a backup immediately at any time.

1. Navigate to create an On-demand backup. Configure:
   - **Resource type**: `RDS`.
   - **Database name**: `genzite-db`.
   - **Total retention period**: `7 Days`.
![On-demand Settings](/images/AWS_backup/backup9.jpg)
2. Configure Vault:
   - **Backup vault**: Select `genzite-backup-vault`.
   - **IAM role**: Select **Default role**.
   - Click **Create on-demand backup**.
![On-demand Vault](/images/AWS_backup/backup10.jpg)
3. Check the progress in the **Jobs** section. Wait until the Status becomes `Completed`. Your backup is now ready!
![Jobs completed](/images/AWS_backup/backup11.jpg)
