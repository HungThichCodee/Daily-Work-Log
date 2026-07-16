---
title: "2. Xử lý Sự kiện"
weight: 2
chapter: false
pre: " <b> 5.6.2. </b> "
---


Trong Genzite, khi AI Worker sinh xong giao diện và lưu thành công vào Database, Service chịu trách nhiệm về Website (Site Service) sẽ "phát" (Publish) một tin nhắn lên Kafka. Bất kỳ service nào quan tâm (Subscribe) đều sẽ nhận được tin nhắn đó. 

Ở bài Lab này, ta sẽ thiết lập **Notification Service** đóng vai trò là một Consumer, lắng nghe sự kiện để gửi email cho người dùng.

## Bước 1: Khái niệm Pub/Sub trong Genzite

1. **Publisher (Site Service)**: Khi người dùng tạo xong web, API sẽ chạy một đoạn code gửi thông điệp dạng JSON:
   ```json
   {
      "userId": "user-123",
      "email": "student@example.com",
      "siteName": "Coffee Shop",
      "siteUrl": "http://coffee-shop.genzite.com"
   }
   ```
   vào Topic `SiteCreated` trên Kafka.
   
2. **Subscriber (Notification Service)**: Là một service (hoặc module) chạy độc lập, luôn kết nối với Kafka và lắng nghe topic `SiteCreated`. Khi có tin nhắn mới rơi vào topic này, nó sẽ tự động đọc (Consume).

## Bước 2: Cập nhật biến môi trường Kafka cho Backend

Để Backend có thể giao tiếp với Kafka vừa tạo bằng Docker, ta mở lại file `.env`:

```bash
cd genzite-backend
nano .env
```
Thêm biến môi trường Kafka:
```env
# Kafka Configuration
KAFKA_BROKER=localhost:9092
KAFKA_CLIENT_ID=genzite-api
```
Lưu lại và khởi động lại API bằng PM2 (`pm2 restart all`).

## Bước 3: Kiểm thử luồng gửi thông báo

Do Notification Service thường được thiết kế để tích hợp với Amazon SES (Simple Email Service) để gửi email thật. Tuy nhiên trong môi trường Sandbox hoặc Free Tier, việc cấu hình SES khá phức tạp (cần verify domain/email). 

Nên ở bước này, ta sẽ kiểm tra bằng cách xem log của Notification Service xem nó đã bắt được event chưa.

1. Bật terminal xem log của PM2:
   ```bash
   pm2 logs
   ```
2. Mở trình duyệt, quay lại giao diện Genzite và tạo một trang web mới.
3. Khi thanh tiến trình báo 100%, hãy quan sát terminal trên EC2.
4. Nếu màn hình console in ra dòng chữ:
   `[NotificationService] Received SiteCreated event for student@example.com. Sending Welcome email...`
   
   Thì xin chúc mừng! Sự kiện đã được đẩy qua Kafka và Consumer đã bắt được thành công.

## Lợi ích của kiến trúc này
Giả sử sau này bạn muốn thêm tính năng "Tặng 100 điểm thưởng khi tạo web thành công", bạn không cần phải sửa code của Site Service. Bạn chỉ cần tạo một `RewardService` mới, cho nó lắng nghe topic `SiteCreated` từ Kafka và cộng điểm. Hệ thống hoàn toàn không bị ảnh hưởng hay gián đoạn!

---
**🎉 CHÚC MỪNG BẠN ĐÃ HOÀN THÀNH TOÀN BỘ WORKSHOP! 🎉**

Từ một VPC trống rỗng, bạn đã tự tay xây dựng một hệ thống Cloud-Native hoàn chỉnh:
- Có **Frontend** chạy trên **S3 + CloudFront** với tốc độ CDN siêu tốc.
- Có **Bảo mật** với **Cognito** và cấu trúc **Public/Private Subnet + NAT Gateway**.
- Có **Backend** mạnh mẽ kết nối với **RDS PostgreSQL** qua **ALB**.
- Có hệ thống hàng đợi **BullMQ (Redis)** giải quyết bài toán timeout khi gọi **Google Gemini API**.
- Và cuối cùng là kiến trúc **Event-Driven** linh hoạt với **Kafka**.

Bạn đã nắm trong tay những kiến trúc nền tảng nhất mà các hệ thống lớn đang sử dụng. Hãy chuyển sang phần tiếp theo để dọn dẹp tài nguyên (Cleanup) tránh phát sinh chi phí nhé.
