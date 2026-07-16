---
title: "Dọn dẹp tài nguyên"
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

Chúc mừng bạn đã hoàn thành chuỗi Workshop xây dựng hệ thống Genzite trên AWS! 🎉

Để tránh phát sinh các chi phí không mong muốn (đặc biệt là các dịch vụ không nằm trong Free Tier như NAT Gateway, ALB, RDS), bước dọn dẹp tài nguyên (Cleanup) là cực kỳ quan trọng. Hãy làm theo đúng thứ tự dưới đây để đảm bảo mọi thứ được xoá thành công mà không bị kẹt vì ràng buộc (dependency).

## Bước 1: Xóa các dịch vụ tính toán (Compute) và Cân bằng tải (ALB)

1. **EC2 Instances**:
   - Truy cập **EC2 Dashboard**.
   - Chọn instance `genzite-backend` -> **Instance state** -> **Terminate instance**.
2. **Application Load Balancer (ALB)**:
   - Trong menu EC2, cuộn xuống phần **Load Balancing**, chọn **Load Balancers**.
   - Chọn ALB `genzite-alb` -> **Actions** -> **Delete**.
3. **Target Groups**:
   - Chọn **Target Groups**, xoá các Target Group đã tạo (`genzite-backend-tg`, `frontend-tg`).

## Bước 2: Xóa cơ sở dữ liệu (Databases)

1. **Amazon RDS PostgreSQL**:
   - Truy cập **RDS Dashboard**.
   - Chọn **Databases**, chọn instance `genzitedb` -> **Actions** -> **Delete**.
   - Bỏ chọn "Create final snapshot" và xác nhận "I acknowledge...". Nhập `delete me` vào ô xác nhận để xoá.
   - Chọn **Subnet groups** ở menu trái, xóa Subnet group `genzite-subnet-rds`.

## Bước 3: Xóa Amazon Cognito

1. Truy cập **Cognito Dashboard**.
2. Chọn **User pools**.
3. Chọn User pool bạn đã tạo (VD: `genzite-user-pool`) -> **Delete**. Xác nhận tên user pool để hoàn tất việc xoá.

## Bước 4: Xóa tài nguyên Giám sát (CloudWatch & SNS)

1. **CloudWatch Alarms**:
   - Truy cập **CloudWatch Dashboard**, chọn **Alarms** -> **All alarms**.
   - Chọn Alarm về CPU bạn đã tạo -> **Actions** -> **Delete**.
2. **CloudWatch Log Groups**:
   - Chọn **Logs** -> **Log groups**.
   - Tìm và xoá Log group của EC2.
3. **Amazon SNS (Nếu có)**:
   - Truy cập **SNS Dashboard**, chọn **Topics**.
   - Chọn Topic cảnh báo bạn đã tạo -> **Delete**. Xác nhận chữ `delete me` để xoá.

## Bước 5: Xóa Frontend và Media (CloudFront & S3)

1. **CloudFront**:
   - Truy cập **CloudFront Dashboard**.
   - Chọn Distribution của bạn, nhấn **Disable** (Quá trình này mất khoảng 3-5 phút).
   - Sau khi trạng thái chuyển sang Disabled, chọn lại distribution đó và nhấn **Delete**.
2. **Amazon S3**:
   - Truy cập **S3 Dashboard**.
   - Thực hiện với cả 2 bucket: Bucket Frontend (VD: `workshop-frontend-app-12345`) và Bucket Media (`genzite-media-bucket`).
   - Nhấn **Empty** để xoá toàn bộ file bên trong (bạn cần gõ `permanently delete` để xác nhận).
   - Sau khi bucket trống, nhấn **Delete** để xoá bucket.

## Bước 6: Xóa tài nguyên Sao lưu (AWS Backup)

1. **AWS Backup Plans**:
   - Truy cập **AWS Backup Dashboard**, chọn **Backup plans**.
   - Chọn Backup plan đã tạo (VD: `genzite-backup-plan`) -> **Delete**.
2. **Recovery Points (Bản sao lưu)**:
   - Chọn **Backup vaults**, nhấp vào Vault bạn đã tạo (VD: `genzite-backup-vault`).
   - Chọn tất cả các Recovery points bên trong -> **Delete** (cần xác nhận để xóa).
3. **AWS Backup Vaults**:
   - Sau khi Vault đã trống, chọn Vault đó và nhấn **Delete**.

## Bước 7: Xóa Mạng lưới (VPC & Security Groups)

*Lưu ý: Bạn phải xóa NAT Gateway và giải phóng Elastic IP trước tiên để không bị mất phí.*

1. **NAT Gateway**:
   - Truy cập **VPC Dashboard**, chọn **NAT gateways**.
   - Chọn `genzite-nat-gw` -> **Actions** -> **Delete NAT gateway**. (Đợi vài phút để trạng thái chuyển thành Deleted).
2. **VPC Endpoints**:
   - Chọn **Endpoints**, xoá S3 Gateway Endpoint đã tạo.
3. **VPC**:
   - Chọn **Your VPCs**, chọn `genzite` (hoặc `genzite-vpc`).
   - Nhấn **Actions** -> **Delete VPC**.
   - Hành động này sẽ tự động xóa các Subnets, Route Tables, Internet Gateway và các Security Groups đi kèm VPC đó.

---
**Hoàn tất!** Bạn đã dọn dẹp sạch sẽ toàn bộ môi trường AWS của Workshop này. Hẹn gặp lại bạn ở những dự án Cloud tiếp theo!
