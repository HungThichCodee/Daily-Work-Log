---
title: "Worklog Tuần 10"
date: 2024-01-01
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---



### Mục tiêu tuần 10 (22/06/2026 – 28/06/2026):
* Kiểm thử toàn bộ API thuộc phân hệ Identity và AI Service.
* Hoàn thiện xong phần Backend (BE) của dự án đối với 2 dịch vụ này.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2 | - **Hoàn thiện Identity Service (Auth & Users):**<br>&emsp; + Hoàn tất module xác thực bằng `@nestjs/jwt`, `passport-jwt` và mã hóa mật khẩu với `bcryptjs`.<br>&emsp; + Cấu trúc lại schema cơ sở dữ liệu với Prisma (Prisma Migrate/Seed) cho module Users.<br>&emsp; + Tích hợp hệ thống gửi Email (Quên mật khẩu/Xác thực) sử dụng `Nodemailer`. | 22/06/2026 | 22/06/2026 | - |
| 3 | - **Hoàn thiện AI Service (Core Logic):**<br>&emsp; + Tích hợp thành công đa mô hình ngôn ngữ (LLM): `@google/generative-ai` (Gemini) và `openai`.<br>&emsp; + Hoàn thiện logic Pipeline, Generation và xử lý Agent (Agent orchestration).<br>&emsp; + Ứng dụng Model Context Protocol (MCP) để phân tích ngữ cảnh (Context Memory). | 23/06/2026 | 23/06/2026 | - |
| 4 | - **Xử lý Bất đồng bộ & Event-driven:**<br>&emsp; + Cấu hình và tối ưu Background Jobs cho AI Service bằng `BullMQ` (Workers).<br>&emsp; + Tích hợp Message Broker (`@genzite/kafka`) để đồng bộ dữ liệu (Events) giữa Identity Service và AI Service. | 24/06/2026 | 24/06/2026 | - |
| 5 | - **Kiểm thử System và Unit Test:**<br>&emsp; + Viết test (Jest / Supertest) cho các module Auth, Mail (Identity) và Recruitment, Generation (AI Service).<br>&emsp; + Test End-to-End toàn bộ luồng request, đảm bảo request gửi vào hàng đợi BullMQ được xử lý ổn định mà không dính timeout. | 25/06/2026 | 25/06/2026 | - |
| 6 | - **Đóng gói và Tài liệu hóa:**<br>&emsp; + Review và chuẩn hóa `Dockerfile.dev` cho cả hai microservices.<br>&emsp; + Cập nhật tài liệu API chi tiết cho team Frontend.<br>&emsp; + Báo cáo nghiệm thu hoàn thiện phân hệ Identity và AI Service. | 26/06/2026 | 26/06/2026 | - |


### Kết quả đạt được tuần 10:

* Kiểm thử API:
  * Đã kiểm thử và xử lý bug toàn bộ API, đảm bảo bảo mật và phản hồi đúng nghiệp vụ trên môi trường AWS.

* Identity Service:
  * Tích hợp JWKS Verification trên server EC2 (Private Subnet) để xác thực JWT token cấp bởi Cognito.
  * Triển khai cấu trúc Database qua Prisma để quản lý Users, RBAC, Config trên RDS PostgreSQL.
  * Hoàn thiện luồng Message Broker Kafka và hệ thống gửi Email (Nodemailer).

* AI Service:
  * Cấu hình luồng Outbound an toàn từ EC2 qua NAT Gateway để giao tiếp với AI bên ngoài.
  * Tích hợp caching Redis để lưu trữ AI Response, tối ưu độ trễ và tiết kiệm chi phí.
  * Hoàn thiện logic cấp link tải lên an toàn giúp client tải file lên S3.
  * Ứng dụng BullMQ làm Background Workers và Model Context Protocol (MCP).



