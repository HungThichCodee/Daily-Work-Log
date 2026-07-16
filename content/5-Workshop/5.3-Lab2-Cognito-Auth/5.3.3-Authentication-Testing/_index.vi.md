---
title: "Kiểm thử Xác thực"
weight: 33
chapter: false
pre: "<b>5.3.3. </b>"
---


Kiểm thử luồng xác thực giúp đảm bảo rằng người dùng có thể đăng ký một cách an toàn và những kẻ không có thẩm quyền không thể truy cập tài nguyên bảo mật.

## 1. Khởi động Frontend
Mở terminal và thực thi lệnh:
```bash
npm run dev
```
Trình duyệt sẽ mở ở `http://localhost:5173`.

![Run Terminal Cognito](/images/Cognito/run_terminal_cognito.png)

## 2. Kiểm thử Đăng ký (Sign Up) & Xác minh Email
1. Click vào nút **Sign Up**.
2. Nhập email và mật khẩu (theo đúng chính sách bảo mật).
3. Nhấn tạo tài khoản, giao diện sẽ chuyển sang màn hình xác minh OTP.
4. Mở email của bạn để lấy mã xác thực.
5. Nhập mã code gồm 6 chữ số và nhấn **Verify**.

![Test Mail Verify 1](/images/Cognito/test_mailverify_cognito1.png)

![Test Mail Verify 2](/images/Cognito/test_mailverify_cognito2.png)

## 3. Kiểm tra trên AWS Console
Để chắc chắn người dùng đã được tạo thành công trên hệ thống:
1. Mở dịch vụ **Cognito**, vào User Pool của bạn.
2. Tại tab **Users**, bạn sẽ thấy địa chỉ email vừa tạo với trạng thái là **CONFIRMED**.

![Test Account Cognito](/images/Cognito/testaccount_cognito.png)

## 4. Kiểm thử Backend Guard
Bây giờ, hãy sử dụng các công cụ như **Postman** hoặc **cURL** để gọi API backend NestJS của bạn.

**Không có Token (Phải thất bại với lỗi 401):**
```bash
curl -X GET http://localhost:3000/secure-data
```

**Có Token (Phải thành công với mã 200):**
```bash
curl -X GET http://localhost:3000/secure-data \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

Nếu bạn thấy dữ liệu bảo mật được trả về, chúc mừng! Backend NestJS của bạn đã xác minh JWT từ Cognito thành công.
