---
title: "4. Tạo User Pool"
weight: 31
chapter: false
pre: "<b>5.3.1. </b>"
---


User Pool là một thư mục người dùng trong Amazon Cognito. Với user pool, người dùng có thể đăng nhập vào ứng dụng web của bạn thông qua Amazon Cognito, hoặc thông qua một nhà cung cấp danh tính bên thứ ba (Google, Facebook).

> **Lưu ý phân quyền:** Toàn bộ quá trình tạo và cấu hình Cognito do **User C (Application & Storage)** phụ trách.

## Hướng dẫn từng bước

### 1. Cấu hình User Pool
1. Truy cập **Amazon Cognito Console**.
2. Nhấn **Create user pool**.
3. **Configure sign-in experience**:
   - Provider types: `Cognito user pool`
   - Cognito user pool sign-in options: Chọn `Email` (và có thể chọn thêm `User name` nếu bạn muốn).
   - Nhấn **Next**.

### 2. Cấu hình Yêu cầu Bảo mật
1. **Password policy**: Chọn chính sách mật khẩu mặc định hoặc tùy chỉnh độ khó.
2. **Multi-factor authentication (MFA)**: Chọn `No MFA` trong khuôn khổ workshop này để đơn giản hóa, hoặc `Optional MFA` nếu muốn thử nghiệm.
3. **User account recovery**: Giữ nguyên mặc định (`Email`).
4. Nhấn **Next**.

### 3. Cấu hình Trải nghiệm Đăng ký
1. **Self-service sign-up**: Bật cho phép người dùng tự đăng ký.
2. **Attribute verification and user account confirmation**: Cho phép Cognito tự động gửi tin nhắn xác nhận. Chọn `Send email message, verify email address`.
3. **Required attributes**: Thêm các thuộc tính bắt buộc (ví dụ: `given_name`, `family_name`).
4. Nhấn **Next**.

> **Lưu ý:** Bạn không thể thay đổi các thuộc tính bắt buộc (Required attributes) sau khi User Pool đã được tạo.

![Warning Mục 3](/images/cognito/warning_muc3_cognito.png)

### 4. Cấu hình Gửi tin nhắn
1. Chọn **Send email with Cognito** (để dùng free-tier/môi trường test). Nếu bạn đã cấu hình Amazon SES, hãy chọn SES.
2. Nhấn **Next**.

![Setup Email Cognito](/images/cognito/setupFE_mail_cognito.png)

### 5. Tích hợp Ứng dụng
1. **User pool name**: Nhập `Workshop-UserPool`.
2. **Hosted UI**: Bỏ chọn (chúng ta sẽ xây dựng giao diện UI riêng ở Frontend).
3. **Initial app client**: 
   - App client name: `Workshop-Frontend-Client`.
   - Client secret: **Do not generate a client secret**. (Client secret không được hỗ trợ và không an toàn cho public frontend app như SPA).
4. Nhấn **Next**, xem lại các thiết lập và nhấn **Create user pool**.

![Create Cognito 1](/images/cognito/create_cognito1.png)

![Create Cognito 2](/images/cognito/create_cognito2.png)

### 6. Lưu trữ thông tin
Sau khi tạo xong, hãy mở User Pool của bạn lên và lưu lại 2 giá trị sau. Bạn sẽ cần chúng cho việc cấu hình Frontend và Backend:
- **User Pool ID**: (ví dụ: `us-east-1_xxxxxxxxx`)
- **App Client ID**: (ví dụ: `1234567890abcdefghijklmnop`)

### 7. Tạo tài khoản Test (Tùy chọn)
Để kiểm tra User Pool, bạn có thể tạo thủ công một người dùng:
1. Chuyển sang tab **Users** trong User Pool của bạn.
2. Nhấn **Create user**.
3. Điền thông tin đăng nhập và chọn **Mark email address as verified**.
4. Nhấn **Create user**.

![Test Account Cognito](/images/cognito/testaccount_cognito.png)
