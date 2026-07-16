---
title: "Lab 5: Kiến trúc Hướng Sự kiện"
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---


Chào mừng bạn đến với **Lab 5**. Trong phần cuối cùng của chuỗi Workshop, chúng ta sẽ ứng dụng mô hình Event-Driven Architecture (EDA) vào hệ thống Genzite.

## Giới thiệu

Khi ứng dụng mở rộng, việc gọi API đồng bộ trực tiếp giữa các service (ví dụ: `SiteService` gọi thẳng `NotificationService` để gửi email) tạo ra sự phụ thuộc chặt chẽ (tight coupling) và có thể gây nghẽn cổ chai. 

Giải pháp là sử dụng một Event Bus (như **Apache Kafka**). Khi một trang web được tạo xong, hệ thống chỉ cần phát (publish) một sự kiện (event) lên Kafka. Các dịch vụ khác (như gửi email, cập nhật thống kê) sẽ tự động lắng nghe và xử lý sự kiện đó một cách độc lập.

Trong bài lab này, bạn sẽ:
- Khởi tạo cluster **Apache Kafka**.
- Cấu hình Genzite phát sự kiện `SiteCreated` khi web được sinh xong.
- Cấu hình Notification Service lắng nghe sự kiện và mô phỏng gửi email chúc mừng.

## Nội dung chi tiết

Lab 5 được chia làm các bước sau:

- **[1. Khởi tạo Kafka](1-kafka-setup/)**: Thiết lập cluster Apache Kafka.
- **[2. Xử lý Sự kiện](2-event-notification/)**: Cấu hình luồng Publish/Subscribe cho hệ thống gửi thông báo.

---
Bắt đầu ngay với bước: **[Khởi tạo Kafka](1-kafka-setup/)**.
