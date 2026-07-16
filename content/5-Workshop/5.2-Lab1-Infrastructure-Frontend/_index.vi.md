---
title: "Lab 1: Hạ tầng & Frontend"
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---


Chào mừng bạn đến với **Lab 1** của chuỗi bài thực hành. Trong phần này, chúng ta sẽ bắt tay vào xây dựng nền móng kiến trúc hệ thống trên AWS và triển khai ứng dụng Frontend.

Đây là bước đi đầu tiên vô cùng quan trọng nhằm đảm bảo ứng dụng có một môi trường mạng bảo mật, biệt lập và được kiểm soát chặt chẽ.

## Kiến trúc Lab 1

Trong Module này, chúng ta sẽ tập trung vào các thành phần chính sau:
1. **Network (Mạng)**: Xây dựng Virtual Private Cloud (VPC) với các Public và Private Subnet.
2. **Security (Bảo mật)**: Phân quyền truy cập an toàn bằng IAM Role và kiểm soát luồng dữ liệu (traffic) bằng Security Group.
3. **Frontend Hosting**: Đưa ứng dụng web (Frontend) lên AWS để người dùng có thể truy cập được thông qua Internet.

## Nội dung chi tiết

Lab 1 được chia làm các bước nhỏ sau đây. Hãy lần lượt thực hiện theo thứ tự:

- **[1. Cấu hình VPC](1-vpc/)**: Thiết lập môi trường mạng mạng bảo mật (VPC, Subnet, Internet Gateway, NAT Gateway).
- **[2. Cấu hình Security](2-security/)**: Định nghĩa các quy tắc tường lửa (Security Group) và cấp quyền IAM cho các dịch vụ.
- **[3. Triển khai Frontend](3-deploy-frontend/)**: Upload và cấu hình hosting cho source code Frontend để website có thể hoạt động.

---
Bạn đã sẵn sàng chưa? Hãy click vào menu bên trái hoặc bắt đầu ngay với phần đầu tiên: **Cấu hình VPC**.
