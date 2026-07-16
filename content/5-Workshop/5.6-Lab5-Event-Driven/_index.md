---
title: "Lab 5: Event-Driven Architecture"
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---


Welcome to **Lab 5**. In the final part of the Workshop, we will apply the Event-Driven Architecture (EDA) pattern to the Genzite system.

## Overview

As applications scale, calling APIs synchronously between microservices (e.g., `SiteService` directly calling `NotificationService` to send an email) creates tight coupling and potential bottlenecks.

The solution is to use an Event Bus (like **Apache Kafka**). When a website generation is complete, the system simply publishes an event to Kafka. Other services (like email notification, analytics) will automatically listen to and process that event independently.

In this lab, you will:
- Initialize an **Apache Kafka** cluster.
- Configure Genzite to publish a `SiteCreated` event when a site is successfully generated.
- Configure the Notification Service to listen to this event and simulate sending a welcome email.

## Step-by-Step Instructions

Lab 5 is broken down into the following steps:

- **[1. Initialize Kafka](1-kafka-setup/)**: Set up the Apache Kafka cluster.
- **[2. Event Processing](2-event-notification/)**: Configure the Publish/Subscribe flow for the notification system.

---
Let's get started with: **[Initialize Kafka](1-kafka-setup/)**.
