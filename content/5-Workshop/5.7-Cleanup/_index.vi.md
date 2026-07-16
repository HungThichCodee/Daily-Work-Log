---
title: "Dọn dẹp tài nguyên"
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

Chúc mừng bạn đã hoàn thành chuỗi Workshop xây dựng hệ thống Genzite trên AWS! 🎉

Để tránh phát sinh các chi phí không mong muốn (đặc biệt là các dịch vụ không nằm trong Free Tier như NAT Gateway, ALB, RDS, ElastiCache), bước dọn dẹp tài nguyên (Cleanup) là cực kỳ quan trọng. Hãy làm theo đúng thứ tự dưới đây để đảm bảo mọi thứ được xoá thành công mà không bị kẹt vì ràng buộc (dependency).

## Bước 1: Xóa các dịch vụ tính toán (Compute) và Cân bằng tải (ALB)

1. **Auto Scaling Group / EC2 Instances**:
   - Truy cập **EC2 Dashboard**.
   - Nếu bạn có dùng Auto Scaling Group (ASG), hãy chọn ASG đó và xoá nó (điều này sẽ tự động terminate các EC2 instances).
   - Nếu bạn chạy EC2 độc lập (như `genzite-backend-ec2`, `genzite-worker`), hãy chọn từng instance -> **Instance state** -> **Terminate instance**.
2. **Application Load Balancer (ALB)**:
   - Trong menu EC2, cuộn xuống phần **Load Balancing**, chọn **Load Balancers**.
   - Chọn ALB `genzite-alb` -> **Actions** -> **Delete**.
3. **Target Groups**:
   - Chọn **Target Groups**, xoá các Target Group đã tạo cho ALB.

## Bước 2: Xóa cơ sở dữ liệu và bộ nhớ đệm (Databases & Cache)

1. **ElastiCache Redis**:
   - Truy cập **ElastiCache Dashboard**.
   - Chọn **Redis clusters**, chọn `genzite-redis` -> **Delete**.
   - Bỏ chọn phần tạo backup (nếu được hỏi) để xóa nhanh.
   - Xóa **Subnet groups** tương ứng của Redis.
2. **Amazon RDS PostgreSQL**:
   - Truy cập **RDS Dashboard**.
   - Chọn **Databases**, chọn instance `genzite-db` -> **Actions** -> **Delete**.
   - Bỏ chọn "Create final snapshot" và xác nhận "I acknowledge...". Nhập `delete me` vào ô xác nhận để xoá.
   - Xóa **Subnet groups** của RDS.

## Bước 3: Xóa Amazon Cognito

1. Truy cập **Cognito Dashboard**.
2. Chọn **User pools**.
3. Chọn `genzite-user-pool` -> **Delete**. Xác nhận tên user pool để hoàn tất việc xoá.

## Bước 4: Xóa tài nguyên Giám sát (CloudWatch & SNS)

1. **CloudWatch Alarms**:
   - Truy cập **CloudWatch Dashboard**, chọn **Alarms** -> **All alarms**.
   - Chọn Alarm bạn đã tạo (VD: `Genzite-High-CPU-Alarm`) -> **Actions** -> **Delete**.
2. **CloudWatch Dashboards**:
   - Chọn **Dashboards**, chọn `Genzite-System-Health` -> **Delete**.
3. **CloudWatch Log Groups**:
   - Chọn **Logs** -> **Log groups**.
   - Chọn `genzite-backend-logs` -> **Delete log group**.
4. **Amazon SNS**:
   - Truy cập **SNS Dashboard**, chọn **Topics**.
   - Chọn Topic `Genzite-CPU-Alert-Topic` -> **Delete**. Xác nhận chữ `delete me` để xoá.

## Bước 5: Xóa Frontend (CloudFront & S3)

1. **CloudFront**:
   - Truy cập **CloudFront Dashboard**.
   - Chọn Distribution của bạn, nhấn **Disable** (Quá trình này mất khoảng 3-5 phút).
   - Sau khi trạng thái chuyển sang Disabled, chọn lại distribution đó và nhấn **Delete**.
2. **Amazon S3**:
   - Truy cập **S3 Dashboard**.
   - Chọn bucket chứa Frontend (VD: `genzite-frontend-bucket-...`).
   - Nhấn **Empty** để xoá toàn bộ file bên trong (bạn cần gõ `permanently delete` để xác nhận).
   - Sau khi bucket trống, nhấn **Delete** để xoá bucket.

## Bước 6: Xóa Mạng lưới (VPC & Security Groups)

*Lưu ý: Bạn phải xóa NAT Gateway và giải phóng Elastic IP trước tiên để không bị mất phí.*

1. **NAT Gateway**:
   - Truy cập **VPC Dashboard**, chọn **NAT gateways**.
   - Chọn NAT Gateway của bạn -> **Actions** -> **Delete NAT gateway**. (Đợi vài phút để trạng thái chuyển thành Deleted).
2. **Elastic IP**:
   - Chuyển sang phần **Elastic IPs** (Menu bên trái).
   - Chọn EIP vừa dùng cho NAT Gateway -> **Actions** -> **Release Elastic IP addresses**.
3. **VPC Endpoints**:
   - Chọn **Endpoints**, xoá S3 Gateway Endpoint đã tạo.
4. **VPC**:
   - Chọn **Your VPCs**, chọn `genzite-vpc`.
   - Nhấn **Actions** -> **Delete VPC**.
   - Hành động này sẽ tự động xóa các Subnets, Route Tables, Internet Gateway và các Security Groups đi kèm VPC đó.

---
**Hoàn tất!** Bạn đã dọn dẹp sạch sẽ toàn bộ môi trường AWS của Workshop này. Hẹn gặp lại bạn ở những dự án Cloud tiếp theo!
