---
title: "2. Event Notification"
weight: 2
chapter: false
pre: " <b> 5.6.2. </b> "
---


In Genzite, when the AI Worker finishes generating the layout and successfully saves it to the Database, the Service responsible for Websites (Site Service) will "Publish" a message to Kafka. Any interested services (Subscribers) will receive that message.

In this Lab, we will set up the **Notification Service** to act as a Consumer, listening to events to send emails to users.

## Step 1: Understanding Pub/Sub in Genzite

1. **Publisher (Site Service)**: Once the user's site is created, the API executes code to send a JSON message:
   ```json
   {
      "userId": "user-123",
      "email": "student@example.com",
      "siteName": "Coffee Shop",
      "siteUrl": "http://coffee-shop.genzite.com"
   }
   ```
   into the `SiteCreated` Topic on Kafka.
   
2. **Subscriber (Notification Service)**: This is an independent service (or module) constantly connected to Kafka, listening to the `SiteCreated` topic. Whenever a new message drops into this topic, it automatically reads (Consumes) it.

## Step 2: Update Kafka Environment Variables for Backend

To allow the Backend to communicate with the Kafka cluster we just created using Docker, let's reopen the `.env` file:

```bash
cd genzite-backend
nano .env
```
Append the Kafka environment variables:
```env
# Kafka Configuration
KAFKA_BROKER=localhost:9092
KAFKA_CLIENT_ID=genzite-api
```
Save and restart the API using PM2 (`pm2 restart all`).

## Step 3: Test the Notification Flow

The Notification Service is typically designed to integrate with Amazon SES (Simple Email Service) to send real emails. However, in a Sandbox or Free Tier environment, configuring SES is quite complex (requires domain/email verification).

Therefore, in this step, we will verify the flow by checking the Notification Service logs to see if it successfully caught the event.

1. Open the terminal and view PM2 logs:
   ```bash
   pm2 logs
   ```
2. Open your browser, return to the Genzite UI, and create a new website.
3. When the progress bar hits 100%, observe the terminal on your EC2 instance.
4. If the console outputs a line like:
   `[NotificationService] Received SiteCreated event for student@example.com. Sending Welcome email...`
   
   Congratulations! The event was pushed through Kafka, and the Consumer caught it successfully.

## Benefits of this Architecture
Suppose later on you want to add a feature: "Award 100 reward points upon successful website creation". You do not need to modify the Site Service code. You simply create a new `RewardService`, have it listen to the `SiteCreated` topic from Kafka, and add the points. The core system remains completely unaffected and uninterrupted!

---
**🎉 CONGRATULATIONS ON COMPLETING THE ENTIRE WORKSHOP! 🎉**

From an empty VPC, you have manually built a comprehensive Cloud-Native system:
- A **Frontend** hosted on **S3 + CloudFront** delivering lightning-fast CDN speeds.
- Robust **Security** leveraging **Cognito** and a **Public/Private Subnet + NAT Gateway** layout.
- A powerful **Backend** connected to **RDS PostgreSQL** via an **ALB**.
- A queuing system using **BullMQ (Redis)** to solve timeout issues when calling the **Google Gemini API**.
- And finally, a highly decoupled **Event-Driven** architecture using **Kafka**.

You have mastered the foundational architectures utilized by large-scale systems. Please proceed to the next section to clean up the resources (Cleanup) and avoid incurring any unexpected charges.
