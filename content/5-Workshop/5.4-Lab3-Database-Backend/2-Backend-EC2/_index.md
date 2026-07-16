---
title: "2. Deploy Backend"
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---


For the Genzite application to process complex requests (generating JSON from prompts, interacting with the Database), we need a Virtual Machine. In AWS, this is the **Amazon Elastic Compute Cloud (EC2)** service.

Based on our design, the EC2 instance will be placed in a **Private Subnet** to hide it from the internet, only allowing traffic routed through the Application Load Balancer (ALB).

## Step 1: Launch the EC2 Instance

1. Open the **EC2** service in the AWS Console.
2. Click **Launch instances**.
3. **Name**: Enter `genzite-backend`.
4. **Application and OS Images (Amazon Machine Image)**:
   - Select **Ubuntu**.
   - Choose **Ubuntu Server 24.04 LTS**.
5. **Instance type**:
   - Select `t3a.large`.
6. **Key pair (login)**:
   - Choose **Create new key pair** with the name `genzite-key`.
7. **Network settings**:
   - Click **Edit**.
   - **VPC**: Select `genzite-vpc`.
   - **Subnet**: Select a **Private Subnet**.
   - **Auto-assign public IP**: **Disable**.
   - **Firewall (security groups)**: Choose **Create security group**.
   - **Security group name**: `genzite-sg`.
![Config EC2](./images/5.4.2.1.png)
8. **Configure storage**:
   - Increase the storage from `8` to `30` GiB.
9. Leave the rest as default. Click **Launch instance**.
![Config EC2](./images/5.4.2.2.png)
## Step 2: Add IAM Role for EC2

1. Go back to the **EC2** dashboard, select **genzite-backend**, click **Actions**, then **Security** and **Modify IAM role**.
![Config EC2](./images/5.4.2.3.png)
2. Change the IAM role to the **genzite-role** role.
3. Click **Update IAM role**.
![Config EC2](./images/5.4.2.4.png)
4. Return to the **EC2** page, proceed to **Reboot** the EC2 instance and wait a moment.
5. You have successfully added permissions for the EC2 instance.


## Step 3: Connect and Setup Environment (Docker, Node.js)

1. After rebooting, select the EC2 instance again and click **Connect**.
2. Switch to the **Session Manager** tab, scroll down, and click **Connect**.
3. In the terminal, test with the command `whoami` (if it returns `ssm-user`, it is correct).
4. Proceed to run the following commands to update the system and setup the environment:

```bash
sudo apt update
sudo apt update && sudo apt upgrade -y

# Install Docker
sudo apt install -y docker.io
docker --version
sudo systemctl enable docker
sudo systemctl start docker
sudo systemctl status docker
```
*(Press `Ctrl + C` to exit the Docker status screen)*

Continue installing Docker Compose and Git:
```bash
sudo apt install -y docker-compose-v2
docker compose version

sudo apt install -y git
git --version
```

## Step 4: Download Source Code and Run the Application

Switch to the root user to clone the code and run the project:
```bash
sudo -i
git clone https://github.com/KrisCTer/Genzite
cd Genzite

# Install Node.js 22.x
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt install -y nodejs
node -v
npm -v

# Install pnpm
sudo npm install -g pnpm
pnpm install
pnpm run build:packages

# Configure environment variables
cd infra
cp .env.example .env

# Start infrastructure services with Docker Compose
docker compose up -d db cache zookeeper kafka
cd ..

# Migrate database
pnpm run prisma:migrate

# Start the project's microservices (run in the background with nohup)
nohup pnpm run dev:gateway > gateway.log 2>&1 &
nohup pnpm run dev:ai > ai.log 2>&1 &
nohup pnpm run dev:data > data.log 2>&1 &
nohup pnpm run dev:identity > identity.log 2>&1 &
nohup pnpm run dev:media > media.log 2>&1 &
nohup pnpm run dev:site > site.log 2>&1 &
nohup pnpm run dev:notification > notification.log 2>&1 &
nohup pnpm run dev:frontend --host 0.0.0.0 > frontend.log 2>&1 &
```
