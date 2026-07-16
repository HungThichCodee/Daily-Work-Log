---
title: "Lab 2: Xác thực Cognito"
weight: 3
chapter: false
pre: "<b>5.3. </b>"
---

## Tổng quan

Bảo mật là ưu tiên hàng đầu của mọi dự án. Trong Lab này, chúng ta sẽ tìm hiểu cách Genzite sử dụng **Amazon Cognito** để cung cấp tính năng Đăng nhập/Đăng ký (Authentication) thay vì tự xây dựng cơ chế quản lý mật khẩu thủ công.



## Mục tiêu của Lab
1. Tích hợp AWS Amplify vào Frontend (React) để gọi API của Cognito.
2. Tìm hiểu cách Backend xác thực JWT Token được cấp bởi Cognito.
3. Cách đồng bộ User ID của Cognito với Database cục bộ của dự án.

## Cấu hình Cognito trong Mã nguồn

Trong file `infra/.env`, các thông số cấu hình của Cognito được nạp vào biến môi trường:

```ini
VITE_COGNITO_AUTHORITY=https://cognito-idp.us-east-1.amazonaws.com/us-east-1_JN6WuwuuM
VITE_COGNITO_CLIENT_ID=20gjbjlmo2pekj4jfj98js6ilk
VITE_COGNITO_DOMAIN=https://genzite.auth.us-east-1.amazoncognito.com
```

## Tích hợp Frontend

Genzite sử dụng thư viện `@aws-amplify/auth` để giao tiếp với Cognito. 
Trong `apps/frontend/src/main.tsx`, cấu hình Amplify được nạp động:

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

Trong `Login.tsx`, hàm `signIn` của Amplify được gọi. Sau khi đăng nhập thành công, Cognito trả về các Tokens (Access Token, Id Token, Refresh Token), chúng được lưu vào Global State bằng Zustand (`auth.ts`).

## Tích hợp Backend

Khi Frontend gửi request đến API, Access Token của Cognito sẽ được đính kèm trong Header `Authorization: Bearer <token>`.

### 1. Xác thực tại API Gateway

`apps/gateway/src/auth/auth.middleware.ts` chặn các request để kiểm tra tính hợp lệ của token. Nó giải mã JWT và lấy ra `sub` (Cognito User ID):

```typescript
const decodedToken = jwt.decode(token) as any;
if (decodedToken && decodedToken.iss && decodedToken.iss.includes("cognito-idp.")) {
  // Token từ AWS Cognito - được tin cậy trong môi trường dev
  decoded = {
    sub: decodedToken.sub,
    email: decodedToken.email || decodedToken.username,
    roles: decodedToken["cognito:groups"] || ["ADMIN", "USER"],
  };
}
```

### 2. Đồng bộ Database tại Identity Service

Mỗi user trong Cognito có một ID duy nhất gọi là `sub`. Khi user đăng nhập lần đầu, `apps/identity-service/src/users/users.service.ts` sẽ đồng bộ ID này vào Database PostgreSQL bằng câu lệnh Raw SQL để đảm bảo tính toàn vẹn dữ liệu:

```typescript
// Cập nhật user ID để khớp với Cognito sub qua raw SQL
await this.prisma.$executeRawUnsafe(
  `UPDATE "identity"."users" SET id = $1 WHERE email = $2`,
  decoded.sub,
  decoded.email
);
```

Nhờ cơ chế này, mọi dữ liệu quan hệ trong Genzite đều tham chiếu đồng nhất đến Cognito ID!
