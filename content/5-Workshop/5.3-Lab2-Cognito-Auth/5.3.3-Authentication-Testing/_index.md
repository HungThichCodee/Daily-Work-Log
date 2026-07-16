---
title: "6. Authentication Testing"
weight: 33
chapter: false
pre: "<b>5.3.3. </b>"
---


Testing your authentication flow ensures that your users can sign up securely, and unauthorized users cannot access protected resources.

## 1. Test Sign Up
If you have implemented the frontend SDK, use your application's Sign Up page. If not, you can use the AWS CLI to test it:

```bash
aws cognito-idp sign-up \
  --client-id YOUR_APP_CLIENT_ID \
  --username testuser@example.com \
  --password "Test@12345"
```
Check your email inbox for the verification code.

![Test Mail Verify 1](/images/Cognito/test_mailverify_cognito1.png)

![Test Mail Verify 2](/images/Cognito/test_mailverify_cognito2.png)

To confirm the user via CLI:
```bash
aws cognito-idp confirm-sign-up \
  --client-id YOUR_APP_CLIENT_ID \
  --username testuser@example.com \
  --confirmation-code 123456
```

![Test Account Cognito](/images/Cognito/testaccount_cognito.png)

## 2. Test Sign In (Get Tokens)
To retrieve the Access Token, run:

```bash
aws cognito-idp initiate-auth \
  --auth-flow USER_PASSWORD_AUTH \
  --client-id YOUR_APP_CLIENT_ID \
  --auth-parameters USERNAME=testuser@example.com,PASSWORD="Test@12345"
```

The response will contain the `AccessToken`, `IdToken`, and `RefreshToken`.

## 3. Test the Backend Guard
Now, use tools like **Postman** or **cURL** to test your NestJS backend.

**Without Token (Should fail with 401):**
```bash
curl -X GET http://localhost:3000/secure-data
```

**With Token (Should succeed with 200):**
```bash
curl -X GET http://localhost:3000/secure-data \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

If you see the protected data response, congratulations! Your NestJS backend is correctly verifying the Cognito JWT.
