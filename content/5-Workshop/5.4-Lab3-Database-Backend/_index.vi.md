---
title: "Lab 3: Cơ sở dữ liệu & Backend"
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---


Chào mừng bạn đến với **Lab 3**. Trong module này, chúng ta sẽ xây dựng tầng lưu trữ dữ liệu (Database) và máy chủ tính toán (Backend) để xử lý logic của ứng dụng.

## Giới thiệu

Hệ thống Genzite cần lưu trữ thông tin về người dùng, lịch sử sinh mã, và JSON layout của các trang web. Việc tách biệt Database và Backend ra khỏi nhau, đồng thời đặt Database trong Private Subnet là một "Best Practice" về bảo mật trên AWS.

Trong bài lab này, bạn sẽ:
- Triển khai một cơ sở dữ liệu quan hệ **Amazon RDS PostgreSQL** an toàn.
- Cài đặt và cấu hình **Amazon EC2** để chạy mã nguồn API backend (Node.js/NestJS).
- Cấu hình **Application Load Balancer (ALB)** để phân phối lưu lượng truy cập từ Internet vào EC2 một cách an toàn.

## Nội dung chi tiết

Lab 3 bao gồm các phần sau:

- **[1. Khởi tạo RDS PostgreSQL](1-database-rds/)**: Cấu hình cơ sở dữ liệu quản lý (Managed Database).
- **[2. Triển khai Backend trên EC2](2-backend-ec2/)**: Khởi tạo máy chủ, cài đặt môi trường và chạy API.
- **[3. Cấu hình Load Balancer](3-load-balancer/)**: Thiết lập ALB để điều hướng HTTP traffic vào EC2.

---
Bắt đầu ngay với bước: **[Khởi tạo RDS PostgreSQL](1-database-rds/)**.
