---
title: "Application Integration"
weight: 32
chapter: false
pre: "<b>5.3.2. </b>"
---


Now that our User Pool is ready, we need to integrate it into our applications. 

## Frontend Integration
On the frontend (React/Next.js), you can use the `@aws-sdk/client-cognito-identity-provider` or the `aws-amplify` library to handle sign-up, sign-in, and token management.

![Setup Cognito Environment](/images/Cognito/setup_cognito_env.png)

```javascript
import { CognitoIdentityProviderClient, InitiateAuthCommand } from "@aws-sdk/client-cognito-identity-provider";

const client = new CognitoIdentityProviderClient({ region: "us-east-1" });

async function login(username, password) {
  const command = new InitiateAuthCommand({
    AuthFlow: "USER_PASSWORD_AUTH",
    ClientId: "YOUR_APP_CLIENT_ID",
    AuthParameters: {
      USERNAME: username,
      PASSWORD: password,
    },
  });
  
  const response = await client.send(command);
  // Store response.AuthenticationResult.AccessToken
  // Store response.AuthenticationResult.IdToken
  return response;
}
```

## Backend Integration (NestJS)
The backend doesn't handle the login directly. Instead, it receives the JWT `AccessToken` from the frontend in the `Authorization` header and verifies it.

We use the `aws-jwt-verify` library, officially maintained by AWS, to securely verify the signature and claims of the token.

### 1. Install Dependencies
In your NestJS backend folder:
```bash
pnpm install aws-jwt-verify
```

### 2. Create the Auth Guard
Create a guard to protect your endpoints:

```typescript
// auth.guard.ts
import { CanActivate, ExecutionContext, Injectable, UnauthorizedException } from '@nestjs/common';
import { CognitoJwtVerifier } from 'aws-jwt-verify';

@Injectable()
export class AuthGuard implements CanActivate {
  private verifier = CognitoJwtVerifier.create({
    userPoolId: process.env.COGNITO_USER_POOL_ID,
    tokenUse: "access",
    clientId: process.env.COGNITO_CLIENT_ID,
  });

  async canActivate(context: ExecutionContext): Promise<boolean> {
    const request = context.switchToHttp().getRequest();
    const token = this.extractTokenFromHeader(request);
    
    if (!token) {
      throw new UnauthorizedException('Token not found');
    }

    try {
      const payload = await this.verifier.verify(token);
      request['user'] = payload; // Attach user info to request
      return true;
    } catch (err) {
      throw new UnauthorizedException('Invalid token');
    }
  }

  private extractTokenFromHeader(request: any): string | undefined {
    const [type, token] = request.headers.authorization?.split(' ') ?? [];
    return type === 'Bearer' ? token : undefined;
  }
}
```

### 3. Protect your Endpoints
Use the guard in your controllers:

```typescript
// app.controller.ts
import { Controller, Get, UseGuards, Request } from '@nestjs/common';
import { AuthGuard } from './auth.guard';

@Controller('secure-data')
export class AppController {
  
  @UseGuards(AuthGuard)
  @Get()
  getSecureData(@Request() req) {
    return {
      message: 'This is protected data',
      user: req.user.username
    };
  }
}
```

With this setup, any request missing a valid token or providing an expired token will automatically receive a `401 Unauthorized` response.
