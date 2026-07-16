---
title: "2. Tích hợp Ứng dụng"
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---


Sau khi có User Pool, bước tiếp theo là lấy các tham số kết nối để đưa vào mã nguồn Frontend (React). AWS cung cấp thư viện `aws-amplify` giúp việc gọi các hàm đăng nhập/đăng ký vô cùng đơn giản.

## Bước 1: Lấy thông tin User Pool ID và Client ID

1. Tại giao diện **Cognito**, click vào User Pool `genzite-user-pool` bạn vừa tạo.
2. Sao chép **User pool ID** (có dạng `us-east-1_xxxxxxxxx`) và lưu vào một file text tạm thời.
3. Chuyển sang tab **App integration**.
4. Kéo xuống phần **App client list**, bạn sẽ thấy `genzite-react-client`.
5. Sao chép **Client ID** (một chuỗi khoảng 26 ký tự) và lưu lại.

## Bước 2: Cấu hình biến môi trường trên Frontend

Trong thư mục source code Frontend của Genzite, tìm file `.env` (nếu chưa có thì tạo mới file `.env` ở thư mục gốc của project Frontend).

Dán các thông tin bạn vừa lấy được vào file này:

```env
VITE_AWS_REGION=us-east-1
VITE_COGNITO_USER_POOL_ID=us-east-1_xxxxxxxxx
VITE_COGNITO_APP_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxxxxxxx
```

*(Lưu ý: Thay `us-east-1_xxxxxxxxx` và `xxxxxxxxxxxxxxxxxxxxxxxxxx` bằng thông tin thực tế của bạn).*

## Bước 3: Cài đặt và tích hợp AWS Amplify (Tham khảo)

*Lưu ý: Mã nguồn Frontend của khoá học có thể đã được cấu hình sẵn phần này. Đây là các bước giải thích nguyên lý hoạt động để bạn nắm rõ.*

Để kết nối với Cognito từ React, dự án sẽ cài đặt thư viện:
```bash
npm install aws-amplify
```

Trong file khởi tạo ứng dụng (ví dụ `main.tsx` hoặc `App.tsx`), cấu hình Amplify như sau:

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

Từ đây, mỗi khi người dùng gọi hàm `signIn({ username, password })` của thư viện Amplify, Frontend sẽ tự động gọi API lên AWS Cognito để xác thực và nhận về **JWT Token**.

---
Việc cấu hình kết nối đã xong. Ở phần tiếp theo, chúng ta sẽ bắt đầu **Kiểm thử luồng Đăng nhập/Đăng ký** trực tiếp trên giao diện!
