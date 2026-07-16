---
title: "1. Tạo User Pool"
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---


Trong phần này, chúng ta sẽ tạo một User Pool để lưu trữ và quản lý tài khoản người dùng an toàn.

## Bước 1: Khởi tạo User Pool
1. Truy cập vào dịch vụ **Cognito** trên giao diện AWS Management Console.
2. Đảm bảo bạn đang ở đúng Region `us-east-1` (US East N. Virginia).
3. Nhấn vào nút **Create user pool**.

## Bước 2: Cấu hình trải nghiệm đăng nhập (Sign-in experience)
1. **Cognito user pool sign-in options**: Đánh dấu chọn vào **Email**. Người dùng sẽ sử dụng địa chỉ email làm tên đăng nhập.
2. Giữ nguyên các thiết lập khác và nhấn **Next**.

## Bước 3: Cấu hình yêu cầu bảo mật (Security requirements)
1. **Password policy**: Để mặc định (Cognito defaults) yêu cầu tối thiểu 8 ký tự, có số, ký tự đặc biệt, chữ in hoa, in thường.
2. **Multi-factor authentication (MFA)**: Chọn **No MFA** (Không dùng xác thực 2 bước để giữ bài Lab đơn giản, dễ test).
3. **User account recovery**: Chọn **Email only**.
4. Nhấn **Next**.

## Bước 4: Cấu hình trải nghiệm đăng ký (Sign-up experience)
1. **Self-service sign-up**: Tích chọn **Enable self-registration** (Cho phép người dùng tự do đăng ký tài khoản từ web).
2. **Cognito-assisted verification and confirmation**: Để mặc định (Allow Cognito to automatically send messages to verify and confirm).
3. **Required attributes**: Chọn **email** (bắt buộc cung cấp email khi đăng ký).
4. Nhấn **Next**.

## Bước 5: Cấu hình gửi thông báo (Message delivery)
1. **Email provider**: Chọn **Send email with Cognito** (Đây là tuỳ chọn miễn phí, cho phép gửi tối đa 50 email mỗi ngày - hoàn toàn phù hợp cho mục đích thực hành).
2. Nhấn **Next**.

## Bước 6: Tích hợp ứng dụng (Integrate your app)
1. **User pool name**: Nhập tên gợi nhớ, ví dụ: `genzite-user-pool`.
2. **Hosted authentication pages**: Không chọn (Chúng ta sẽ dùng giao diện đăng nhập tự code bằng React thay vì dùng Hosted UI của AWS).
3. **Initial app client**: Chọn **Public client**.
4. **App client name**: Nhập `genzite-react-client`.
5. **Client secret**: Chọn **Don't generate a client secret** (RẤT QUAN TRỌNG: Môi trường Frontend như React/SPA không bảo mật được client secret, nếu tạo secret thì frontend sẽ không gọi API được).
6. Nhấn **Next**.

## Bước 7: Xem lại và tạo
1. Kiểm tra lại toàn bộ thông tin đã cấu hình.
2. Kéo xuống dưới cùng và nhấn **Create user pool**.

---
Vậy là bạn đã có một "kho chứa" tài khoản người dùng an toàn! Hãy chuyển sang bước tiếp theo để lấy thông tin kết nối và **tích hợp vào mã nguồn Frontend**.
