---
title: "Lab 2: Cognito Authentication"
weight: 3
chapter: false
pre: "<b>5.3. </b>"
---

## Overview

Security is a top priority for any project. In this Lab, we will explore how Genzite utilizes **Amazon Cognito** to provide Login/Registration functionality (Authentication) rather than building a custom password management system.

## Lab Objectives
1. Integrate AWS Amplify into the Frontend (React) to call Cognito APIs.
2. Understand how the Backend validates the JWT Token issued by Cognito.
3. Learn how the Cognito User ID is synchronized with the local Database.

## Cognito Configuration in Source Code

In the `infra/.env` file, Cognito parameters are loaded into environment variables:

```ini
VITE_COGNITO_AUTHORITY=https://cognito-idp.us-east-1.amazonaws.com/us-east-1_JN6WuwuuM
VITE_COGNITO_CLIENT_ID=20gjbjlmo2pekj4jfj98js6ilk
VITE_COGNITO_DOMAIN=https://genzite.auth.us-east-1.amazoncognito.com
```

## Frontend Integration

Genzite uses the `@aws-amplify/auth` library to communicate with Cognito. 
In `apps/frontend/src/main.tsx`, the Amplify configuration is loaded dynamically:

```typescript
const cognitoUserPoolId = import.meta.env.VITE_COGNITO_AUTHORITY?.split("/").pop() || "";
const cognitoClientId = import.meta.env.VITE_COGNITO_CLIENT_ID || "";

if (cognitoUserPoolId && cognitoClientId && !cognitoUserPoolId.includes("xxxxxx")) {
  Amplify.configure({
    Auth: {
      Cognito: {
        userPoolId: cognitoUserPoolId,
        userPoolClientId: cognitoClientId,
      },
    },
  });
}
```

In `Login.tsx`, the Amplify `signIn` function is invoked. Upon a successful login, Cognito returns Tokens (Access Token, Id Token, Refresh Token), which are stored in the Global State using Zustand (`auth.ts`).

## Backend Integration

When the Frontend sends a request to the API, the Cognito Access Token is attached in the `Authorization: Bearer <token>` Header.

### 1. API Gateway Verification

The `apps/gateway/src/auth/auth.middleware.ts` intercepts requests to verify token validity. It decodes the JWT and extracts the `sub` (Cognito User ID):

```typescript
const decodedToken = jwt.decode(token) as any;
if (decodedToken && decodedToken.iss && decodedToken.iss.includes("cognito-idp.")) {
  // AWS Cognito token - trusted in dev environment
  decoded = {
    sub: decodedToken.sub,
    email: decodedToken.email || decodedToken.username,
    roles: decodedToken["cognito:groups"] || ["ADMIN", "USER"],
  };
}
```

### 2. Database Synchronization in Identity Service

Every user in Cognito has a unique ID called a `sub`. When a user logs in for the first time, `apps/identity-service/src/users/users.service.ts` synchronizes this ID into the PostgreSQL Database using a Raw SQL command to ensure data integrity:

```typescript
// Update user ID to match Cognito sub via raw SQL
await this.prisma.$executeRawUnsafe(
  `UPDATE "identity"."users" SET id = $1 WHERE email = $2`,
  decoded.sub,
  decoded.email
);
```

Thanks to this mechanism, all relational data in Genzite consistently references the Cognito ID!
