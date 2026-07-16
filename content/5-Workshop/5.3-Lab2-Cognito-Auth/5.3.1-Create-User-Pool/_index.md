---
title: "Create User Pool"
weight: 31
chapter: false
pre: "<b>5.3.1. </b>"
---


A User Pool is a user directory in Amazon Cognito. With a user pool, your users can sign in to your web or mobile app through Amazon Cognito, or federate through a third-party identity provider.

## Step-by-step Guide

### 1. Configure the User Pool
1. Navigate to the **Amazon Cognito Console**.
2. Click **Create user pool**.
3. **Configure sign-in experience**:
   - Provider types: `Cognito user pool`
   - Cognito user pool sign-in options: Select `Email` (and optionally `User name` if you want users to choose a username).
   - Click **Next**.

### 2. Configure Security Requirements
1. **Password policy**: Choose the default policy or customize it (e.g., require numbers, symbols).
2. **Multi-factor authentication (MFA)**: Select `No MFA` for this workshop to keep things simple, or `Optional MFA` if you want to test it.
3. **User account recovery**: Leave default (`Email`).
4. Click **Next**.

### 3. Configure Sign-up Experience
1. **Self-service sign-up**: Enable self-registration.
2. **Attribute verification and user account confirmation**: Allow Cognito to automatically send messages to verify and confirm. Select `Send email message, verify email address`.
3. **Required attributes**: Add any custom attributes you need (e.g., `given_name`, `family_name`).
4. Click **Next**.

> **Note:** You cannot change the required attributes after the User Pool has been created.

![Warning Step 3](/images/cognito/warning_muc3_cognito.png)

### 4. Configure Message Delivery
1. Choose **Send email with Cognito** (for free-tier/testing). If you have an SES identity, you can select Amazon SES.
2. Click **Next**.

![Setup Email Cognito](/images/cognito/setupFE_mail_cognito.png)

### 5. Integrate your App
1. **User pool name**: Enter `Workshop-UserPool`.
2. **Hosted UI**: Leave unchecked (we will use our custom UI in the frontend).
3. **Initial app client**: 
   - App client name: `Workshop-Frontend-Client`.
   - Client secret: **Do not generate a client secret**. (Client secrets are not supported for public SPA frontend apps).
4. Click **Next**, review the settings, and click **Create user pool**.

![Create Cognito 1](/images/cognito/create_cognito1.png)

![Create Cognito 2](/images/cognito/create_cognito2.png)

### 6. Save Identifiers
Once created, open your User Pool and save these two values; you will need them for your Frontend and Backend configuration:
- **User Pool ID**: (e.g., `us-east-1_xxxxxxxxx`)
- **App Client ID**: (e.g., `1234567890abcdefghijklmnop`)

### 7. Create a Test Account (Optional)
To test the User Pool, you can manually create a user:
1. Go to the **Users** tab in your User Pool.
2. Click **Create user**.
3. Fill in the credentials and select **Mark email address as verified**.
4. Click **Create user**.

![Test Account Cognito](/images/cognito/testaccount_cognito.png)
