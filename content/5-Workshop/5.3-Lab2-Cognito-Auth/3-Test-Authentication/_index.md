---
title: "3. Test Authentication"
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---


In this section, we will run the Frontend application in the local environment (or access it via the S3/CloudFront domain) to verify if the connection to Amazon Cognito works properly.

## Step 1: Launch the Frontend
If you are running the application locally, open a terminal and execute:
```bash
npm run dev
```
The browser will automatically open at `http://localhost:5173`.
*(Note: If you are accessing it via the CloudFront link from the previous Lab, use that link instead).*

## Step 2: Create a New Account (Sign-up)
1. On the Genzite UI, click the **Sign Up** button.
2. Enter a valid email address (one you can check) and a password (make sure to follow the security policy: 8 characters, numbers, uppercase, lowercase).
3. Click **Create Account**.

## Step 3: Email Verification
1. After clicking to create an account, the UI will transition to the **OTP Verification** screen.
2. Check the Inbox of the email address you just registered. Open the email titled "Your verification code" sent from Cognito.
3. Enter the 6-digit code into the input field on the screen.
4. Click **Verify**.

## Step 4: Sign-in and Check Tokens
1. After successful verification, the UI will automatically redirect to the home page or the **Sign In** screen.
2. Enter the email and password you just registered.
3. Click **Login**.
4. **Inspect JWT Tokens**:
   - Right-click on the webpage and select **Inspect** to open the browser's Developer Tools.
   - Switch to the **Application** (or Storage) tab.
   - Open the **Local Storage** section, you will see Cognito storage keys starting with `CognitoIdentityServiceProvider...`.
   - Click on the key containing `accessToken` or `idToken`, and you will see a very long string (that is the JWT Token).

## Step 5: Verify in the AWS Console
To ensure the user was successfully created on the system:
1. Go back to the AWS Management Console and open the **Cognito** service.
2. Click on the `genzite-user-pool`.
3. In the **Users** tab, you will see the email address you just created with the status **CONFIRMED**.

---
**Congratulations!** The sign-up/sign-in features of your application are running smoothly. With this JWT token, users can start utilizing the system's AI website generation capabilities.

Let's move on to **Lab 3** to build the "brain" of Genzite: the **Database and Backend API**.
