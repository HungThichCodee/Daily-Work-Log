---
title: "3. Kiểm thử Xác thực"
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---


Trong phần này, chúng ta sẽ chạy ứng dụng Frontend trên môi trường local (hoặc truy cập qua domain S3/CloudFront) để kiểm tra xem quá trình kết nối với Amazon Cognito đã hoạt động đúng chưa.

## Bước 1: Khởi chạy Frontend
Nếu bạn đang chạy ứng dụng ở máy local, hãy mở terminal và chạy:
```bash
npm run dev
```
Trình duyệt sẽ tự động mở ra ở địa chỉ `http://localhost:5173`. 
*(Lưu ý: Nếu bạn truy cập qua link của CloudFront ở bài Lab trước, hãy dùng link đó).*

## Bước 2: Tạo tài khoản mới (Sign-up)
1. Trên giao diện Genzite, nhấn vào nút **Sign Up** (Đăng ký).
2. Nhập một địa chỉ email hợp lệ của bạn (email bạn có thể nhận được thư) và mật khẩu (nhớ tuân thủ các quy tắc bảo mật: 8 ký tự, có số, chữ hoa, chữ thường).
3. Nhấn **Create Account**.

## Bước 3: Xác thực Email (Verification)
1. Sau khi nhấn tạo tài khoản, giao diện sẽ chuyển sang màn hình **Xác nhận mã OTP**.
2. Kiểm tra hộp thư đến (Inbox) của email bạn vừa đăng ký. Mở thư có tiêu đề "Your verification code" được gửi từ Cognito.
3. Nhập mã code gồm 6 chữ số vào ô nhập trên giao diện.
4. Nhấn **Verify**.

## Bước 4: Đăng nhập (Sign-in) và kiểm tra Token
1. Sau khi xác thực thành công, giao diện sẽ tự động chuyển hướng về trang chủ hoặc màn hình **Sign In**.
2. Nhập email và mật khẩu bạn vừa đăng ký.
3. Nhấn **Login**.
4. **Kiểm tra JWT Token**:
   - Nhấn chuột phải vào trang web, chọn **Inspect** (Kiểm tra) để mở Developer Tools của trình duyệt.
   - Chuyển sang tab **Application** (hoặc Storage).
   - Mở phần **Local Storage**, bạn sẽ thấy các key lưu trữ của Cognito bắt đầu bằng `CognitoIdentityServiceProvider...`.
   - Nhấn vào key chứa `accessToken` hoặc `idToken`, bạn sẽ thấy một chuỗi ký tự rất dài (đó chính là JWT Token).

## Bước 5: Xác minh trên AWS Console
Để chắc chắn user đã được tạo thành công trên hệ thống:
1. Quay lại trang quản trị AWS, mở dịch vụ **Cognito**.
2. Nhấp vào **User pool** `genzite-user-pool`.
3. Trong tab **Users**, bạn sẽ thấy địa chỉ email vừa tạo với trạng thái **CONFIRMED** (Đã xác nhận).

---
**Chúc mừng!** Tính năng đăng ký/đăng nhập của ứng dụng đã hoạt động trơn tru. Với JWT token này, người dùng có thể bắt đầu sử dụng các tính năng tạo web bằng AI của hệ thống. 

Hãy tiếp tục sang **Lab 3** để xây dựng phần "não bộ" của Genzite: **Cơ sở dữ liệu và Backend API**.
