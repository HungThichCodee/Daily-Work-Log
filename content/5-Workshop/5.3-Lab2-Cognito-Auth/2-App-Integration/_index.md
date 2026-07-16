---
title: "2. App Integration"
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---


Once the User Pool is ready, the next step is to retrieve the connection parameters to put into the Frontend (React) source code. AWS provides the `aws-amplify` library, making authentication calls (sign-up/sign-in) incredibly simple.

## Step 1: Retrieve User Pool ID and Client ID

1. In the **Cognito** console, click on the `genzite-user-pool` you just created.
2. Copy the **User pool ID** (it looks like `us-east-1_xxxxxxxxx`) and save it to a temporary text file.
3. Switch to the **App integration** tab.
4. Scroll down to the **App client list** section, where you will find `genzite-react-client`.
5. Copy the **Client ID** (an alphanumeric string of about 26 characters) and save it.

## Step 2: Configure Environment Variables in Frontend

In the Genzite Frontend source code directory, locate the `.env` file (create a new `.env` file at the root of the project if it doesn't exist).

Paste the information you just retrieved into this file:

```env
VITE_AWS_REGION=us-east-1
VITE_COGNITO_USER_POOL_ID=us-east-1_xxxxxxxxx
VITE_COGNITO_APP_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxxxxxxx
```

*(Note: Replace `us-east-1_xxxxxxxxx` and `xxxxxxxxxxxxxxxxxxxxxxxxxx` with your actual values).*

## Step 3: Install and Integrate AWS Amplify (Reference)

*Note: The Frontend source code for this workshop might already have this configured. These steps explain the underlying mechanism for your understanding.*

To connect to Cognito from React, the project installs the following library:
```bash
npm install aws-amplify
```

In the application's entry file (e.g., `main.tsx` or `App.tsx`), Amplify is configured like this:

```typescript
import { Amplify } from 'aws-amplify';

Amplify.configure({
  Auth: {
    Cognito: {
      userPoolId: import.meta.env.VITE_COGNITO_USER_POOL_ID,
      userPoolClientId: import.meta.env.VITE_COGNITO_APP_CLIENT_ID,
      signUpVerificationMethod: 'code',
    }
  }
});
```

From now on, whenever a user calls the `signIn({ username, password })` function from the Amplify library, the Frontend will automatically make an API call to AWS Cognito to authenticate and receive a **JWT Token**.

---
Connection configuration is complete. In the next section, we will **Test the Sign-in/Sign-up flow** directly from the UI!
