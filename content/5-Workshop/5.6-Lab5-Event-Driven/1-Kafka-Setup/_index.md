---
title: "1. Initialize Kafka"
weight: 1
chapter: false
pre: " <b> 5.6.1. </b> "
---


To build an Event-Driven Architecture, Genzite needs an Event Bus to serve as the central message transport hub between Microservices.

In a real-world Production environment, AWS provides **Amazon MSK (Managed Streaming for Apache Kafka)**. However, MSK is quite expensive and not included in the Free Tier. To optimize costs for this workshop, we will install Kafka using Docker directly on our EC2 Backend server.

## Step 1: Install Docker on EC2

1. Go back to **EC2**, and use **Session Manager** to open the terminal for `genzite-backend-ec2`.
2. Run the following commands to install Docker and Docker Compose:

```bash
# Install Docker
sudo dnf install -y docker

# Start the Docker service
sudo systemctl start docker
sudo systemctl enable docker

# Add 'ssm-user' (or 'ec2-user') to the docker group to run commands without sudo
sudo usermod -aG docker ssm-user
sudo usermod -aG docker ec2-user
```
*Note: You may need to close the Session Manager terminal and reconnect for the user permissions to take effect.*

## Step 2: Create Kafka Configuration (docker-compose)

We will use a lightweight Kafka distribution (like Confluent Local or Bitnami Kafka) along with Zookeeper.

1. In the server's root directory, create a folder for the configuration:
   ```bash
   mkdir kafka-setup && cd kafka-setup
   ```
2. Create a `docker-compose.yml` file:
   ```bash
   nano docker-compose.yml
   ```
3. Paste the following content:
   ```yaml
   version: '3'
   services:
     zookeeper:
       image: bitnami/zookeeper:latest
       ports:
         - "2181:2181"
       environment:
         - ALLOW_ANONYMOUS_LOGIN=yes
     kafka:
       image: bitnami/kafka:latest
       ports:
         - "9092:9092"
       environment:
         - KAFKA_CFG_ZOOKEEPER_CONNECT=zookeeper:2181
         - ALLOW_PLAINTEXT_LISTENER=yes
         - KAFKA_CFG_LISTENERS=PLAINTEXT://:9092
         - KAFKA_CFG_ADVERTISED_LISTENERS=PLAINTEXT://localhost:9092
       depends_on:
         - zookeeper
   ```
   *(Press `Ctrl + O` -> `Enter` to save, `Ctrl + X` to exit).*

## Step 3: Start the Kafka Cluster

1. Run the following command to start Zookeeper and Kafka in detached mode:
   ```bash
   # If docker-compose command is not found, install the plugin
   sudo dnf install -y docker-compose-plugin
   docker compose up -d
   ```
2. Verify the running containers:
   ```bash
   docker ps
   ```
   You should see 2 containers (kafka and zookeeper) with the status `Up`.

## Step 4: Test Kafka

Create a topic named `SiteCreated` in preparation for the next step:

```bash
# Shell into the kafka container
docker exec -it $(docker ps -q -f ancestor=bitnami/kafka:latest) bash

# Run the create topic command
kafka-topics.sh --create --topic SiteCreated --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1

# Exit the container
exit
```

---
Kafka is ready! Now the Genzite services can start "shouting" (Publishing) messages to the `SiteCreated` topic instead of calling each other directly. Let's move to the next step to configure the Notification Service.
