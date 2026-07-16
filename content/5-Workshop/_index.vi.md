---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---


#### Tổng quan

Workshop này hướng dẫn bạn cách triển khai nền tảng **Genzite**—một giải pháp AI No-Code cho phép người dùng tự động tạo giao diện trang web hoàn chỉnh bằng cách nhập mô tả (prompt) bằng ngôn ngữ tự nhiên.

Bạn sẽ học cách xây dựng toàn bộ kiến trúc Cloud-Native trên AWS để chạy ứng dụng React (Frontend), NestJS API (Backend), luồng xử lý AI bất đồng bộ, và hệ thống thông báo dựa trên sự kiện (Event-driven).

#### Điểm nhấn Kiến trúc
- **Frontend**: Lưu trữ tĩnh trên Amazon S3 và phân phối qua Amazon CloudFront.
- **Backend**: NestJS API chạy trên Amazon EC2 kết hợp cùng Amazon RDS PostgreSQL.
- **AI Bất đồng bộ**: Tích hợp Google Gemini thông qua hàng đợi BullMQ trên Amazon ElastiCache (Redis).
- **Event-Driven**: Sử dụng Apache Kafka để gửi thông báo tự động (Email) qua Notification Service.
- **Bảo mật**: Xác thực và quản lý tài khoản qua Amazon Cognito.

#### Nội dung

1. [Chuẩn bị (Prerequisites)](5.1-Prerequisites/)
2. [Lab 1: Hạ tầng cơ sở & Frontend](5.2-Lab1-Infrastructure-Frontend/)
3. [Lab 2: Xác thực với Cognito](5.3-Lab2-Cognito-Auth/)
4. [Lab 3: Cơ sở dữ liệu & Backend](5.4-Lab3-Database-Backend/)
5. [Lab 4: Xử lý AI Bất đồng bộ](5.5-Lab4-AI-Async/)
6. [Lab 5: Kiến trúc Event-Driven](5.6-Lab5-Event-Driven/)
7. [Dọn dẹp tài nguyên (Cleanup)](5.7-Cleanup/)

![Genzite Architecture](/images/Genzite.drawio.png)