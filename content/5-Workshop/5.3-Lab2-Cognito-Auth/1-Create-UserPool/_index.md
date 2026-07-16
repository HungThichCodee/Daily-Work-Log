---
title: "1. Create User Pool"
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---


In this section, we will create a User Pool to securely store and manage user accounts.

## Step 1: Initialize User Pool
1. Go to the **Cognito** service in the AWS Management Console.
2. Ensure you are in the correct Region `us-east-1` (US East N. Virginia).
3. Click the **Create user pool** button.

## Step 2: Configure sign-in experience
1. **Cognito user pool sign-in options**: Check the **Email** box. Users will use their email addresses as their usernames.
2. Leave other settings as default and click **Next**.

## Step 3: Configure security requirements
1. **Password policy**: Leave as default (Cognito defaults), which requires a minimum of 8 characters, numbers, special characters, uppercase, and lowercase letters.
2. **Multi-factor authentication (MFA)**: Select **No MFA** (Keep it disabled to simplify testing for this Lab).
3. **User account recovery**: Select **Email only**.
4. Click **Next**.

## Step 4: Configure sign-up experience
1. **Self-service sign-up**: Check **Enable self-registration** (Allow users to sign up from the web interface).
2. **Cognito-assisted verification and confirmation**: Leave as default (Allow Cognito to automatically send messages to verify and confirm).
3. **Required attributes**: Select **email** (Email is required upon registration).
4. Click **Next**.

## Step 5: Configure message delivery
1. **Email provider**: Select **Send email with Cognito** (This is a free option allowing up to 50 emails per day - perfect for testing purposes).
2. Click **Next**.

## Step 6: Integrate your app
1. **User pool name**: Enter a descriptive name, e.g., `genzite-user-pool`.
2. **Hosted authentication pages**: Uncheck this (We will use our custom React UI for login instead of AWS Hosted UI).
3. **Initial app client**: Select **Public client**.
4. **App client name**: Enter `genzite-react-client`.
5. **Client secret**: Select **Don't generate a client secret** (VERY IMPORTANT: Frontend environments like React/SPA cannot securely store client secrets. If generated, the frontend will fail to authenticate).
6. Click **Next**.

## Step 7: Review and create
1. Review all the configured information.
2. Scroll to the bottom and click **Create user pool**.

---
You now have a secure user directory! Move on to the next step to retrieve connection details and **integrate them into the Frontend source code**.
