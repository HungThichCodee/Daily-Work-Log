---
title: "Tích hợp Ứng dụng"
weight: 32
chapter: false
pre: "<b>5.3.2. </b>"
---


Sau khi User Pool đã sẵn sàng, chúng ta cần tích hợp nó vào ứng dụng của mình.

## Tích hợp Frontend
Ở phía frontend (React/Next.js), bạn có thể dùng thư viện `@aws-sdk/client-cognito-identity-provider` hoặc `aws-amplify` để xử lý đăng ký, đăng nhập và quản lý token.

Lưu ý: Thiết lập các biến môi trường cho Frontend với User Pool ID và Client ID mà bạn vừa lưu:

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
  // Lưu trữ response.AuthenticationResult.AccessToken
  // Lưu trữ response.AuthenticationResult.IdToken
  return response;
}
```

## Tích hợp Backend (NestJS)
Backend sẽ không xử lý việc đăng nhập trực tiếp. Thay vào đó, nó nhận JWT `AccessToken` từ frontend thông qua Header `Authorization` và tiến hành xác minh token đó.

Chúng ta sử dụng thư viện `aws-jwt-verify`, do chính AWS bảo trì, để xác minh bảo mật chữ ký và các claims của token.

### 1. Cài đặt thư viện
Tại thư mục backend NestJS của bạn:
```bash
pnpm install aws-jwt-verify
```

### 2. Tạo Auth Guard
Tạo một guard để bảo vệ các API endpoint:

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
      throw new UnauthorizedException('Không tìm thấy token');
    }

    try {
      const payload = await this.verifier.verify(token);
      request['user'] = payload; // Gắn thông tin user vào request
      return true;
    } catch (err) {
      throw new UnauthorizedException('Token không hợp lệ');
    }
  }

  private extractTokenFromHeader(request: any): string | undefined {
    const [type, token] = request.headers.authorization?.split(' ') ?? [];
    return type === 'Bearer' ? token : undefined;
  }
}
```

### 3. Bảo vệ các Endpoint
Sử dụng guard này trong các controller của bạn:

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
      message: 'Đây là dữ liệu được bảo vệ',
      user: req.user.username
    };
  }
}
```

Với thiết lập này, bất kỳ request nào thiếu token hợp lệ hoặc token đã hết hạn sẽ tự động nhận về phản hồi `401 Unauthorized`.
