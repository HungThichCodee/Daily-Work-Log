---
title: "1. Khởi tạo Kafka"
weight: 1
chapter: false
pre: " <b> 5.6.1. </b> "
---


Để xây dựng kiến trúc Hướng sự kiện (Event-Driven), Genzite cần một Event Bus đóng vai trò làm trung tâm vận chuyển thông điệp giữa các Microservices.

Trong môi trường thực tế (Production), AWS cung cấp dịch vụ **Amazon MSK (Managed Streaming for Apache Kafka)**. Tuy nhiên, MSK có chi phí khá cao và không nằm trong Free Tier. Để tối ưu chi phí cho bài thực hành này, chúng ta sẽ cài đặt Kafka thông qua Docker trực tiếp trên máy chủ EC2 Backend.

## Bước 1: Cài đặt Docker trên EC2

1. Truy cập lại **EC2**, dùng **Session Manager** để mở terminal của `genzite-backend-ec2`.
2. Chạy các lệnh sau để cài đặt Docker và Docker Compose:

```bash
# Cài đặt Docker
sudo dnf install -y docker

# Khởi động Docker service
sudo systemctl start docker
sudo systemctl enable docker

# Thêm user 'ssm-user' (hoặc 'ec2-user') vào group docker để chạy lệnh không cần sudo
sudo usermod -aG docker ssm-user
sudo usermod -aG docker ec2-user
```
*Lưu ý: Bạn có thể cần đóng terminal Session Manager và mở lại (Connect lại) để phân quyền user có hiệu lực.*

## Bước 2: Tạo cấu hình Kafka (docker-compose)

Chúng ta sẽ dùng một bản Kafka rút gọn gọn nhẹ (như Confluent Local hoặc Bitnami Kafka) kèm theo Zookeeper.

1. Tại thư mục root của máy chủ, tạo thư mục chứa file cấu hình:
   ```bash
   mkdir kafka-setup && cd kafka-setup
   ```
2. Tạo file `docker-compose.yml`:
   ```bash
   nano docker-compose.yml
   ```
3. Dán nội dung sau vào:
   ```yaml
   version: '3'
   services:
     zookeeper:
       image: bitnami/zookeeper:latest
       ports:
         - "2181:2181"
       environment:
         - ALLOW_ANONYMOUS_LOGIN=yes
     kafka:
       image: bitnami/kafka:latest
       ports:
         - "9092:9092"
       environment:
         - KAFKA_CFG_ZOOKEEPER_CONNECT=zookeeper:2181
         - ALLOW_PLAINTEXT_LISTENER=yes
         - KAFKA_CFG_LISTENERS=PLAINTEXT://:9092
         - KAFKA_CFG_ADVERTISED_LISTENERS=PLAINTEXT://localhost:9092
       depends_on:
         - zookeeper
   ```
   *(Bấm `Ctrl + O` -> `Enter` để lưu, `Ctrl + X` để thoát).*

## Bước 3: Khởi động Kafka Cluster

1. Chạy lệnh sau để khởi động Zookeeper và Kafka ở chế độ chạy ngầm (detached mode):
   ```bash
   # Nếu lệnh docker-compose báo lỗi không tìm thấy, cài bằng script hoặc dùng docker compose plugin
   sudo dnf install -y docker-compose-plugin
   docker compose up -d
   ```
2. Kiểm tra các container đang chạy:
   ```bash
   docker ps
   ```
   Bạn sẽ thấy 2 container (kafka và zookeeper) đang ở trạng thái `Up`.

## Bước 4: Kiểm thử Kafka

Tạo một topic có tên `SiteCreated` để chuẩn bị cho bước sau:

```bash
# Vào bên trong container kafka
docker exec -it $(docker ps -q -f ancestor=bitnami/kafka:latest) bash

# Chạy lệnh tạo topic
kafka-topics.sh --create --topic SiteCreated --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1

# Thoát khỏi container
exit
```

---
Kafka đã sẵn sàng! Bây giờ các dịch vụ của Genzite có thể bắt đầu "hét lên" (Publish) các thông báo lên topic `SiteCreated` thay vì phải gọi trực tiếp cho nhau. Hãy sang bước tiếp theo để cấu hình Notification Service.
