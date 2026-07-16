---
title: "2. Cấu hình Security"
weight: 2
chapter: false
pre: " <b> 5.2.2. </b> "
---


Trong phần này, chúng ta sẽ thiết lập các lớp bảo mật để đảm bảo hệ thống an toàn, chỉ cho phép luồng dữ liệu (traffic) hợp lệ đi qua và cấp quyền vừa đủ (least privilege) cho các dịch vụ.

## Bước 1: Tạo IAM Role cho EC2

Để EC2 (Backend) có thể gửi log lên CloudWatch và tương tác với các dịch vụ AWS khác một cách an toàn mà không cần hardcode API Key, ta sẽ tạo một IAM Role.

1. Mở **AWS Management Console** và tìm dịch vụ **IAM**.
2. Ở menu bên trái, chọn **Roles** và nhấn **Create role**.
3. **Select trusted entity**: Chọn **AWS service**.
4. **Use case**: Chọn **EC2** và nhấn **Next**.
5. Trong trang Add permissions, tìm và đánh dấu vào các policy sau:
   - `AmazonSSMManagedInstanceCore` (Để dùng Session Manager kết nối vào EC2 thay vì mở port 22 SSH).
   - `CloudWatchAgentServerPolicy` (Để đẩy log lên CloudWatch).
6. Nhấn **Next**.
7. **Role name**: Nhập `genzite-role` và nhấn **Create role**.

## Bước 2: Tạo Security Group cho ALB (Internet Facing)

Application Load Balancer (ALB) sẽ là cửa ngõ giao tiếp trực tiếp với internet.

1. Chuyển sang dịch vụ **EC2**. Ở menu bên trái kéo xuống phần Network & Security, chọn **Security Groups**.
2. Nhấn **Create security group**.
3. **Security group name**: `genzite-alb-sg`.
4. **Description**: `Allow HTTP/HTTPS from Internet`.
5. **VPC**: Chọn VPC `genzite-vpc` bạn đã tạo ở phần trước.
6. **Inbound rules**:
   - Thêm Rule 1: Type `HTTP`, Source `Anywhere-IPv4` (`0.0.0.0/0`).
   - Thêm Rule 2: Type `HTTPS`, Source `Anywhere-IPv4` (`0.0.0.0/0`).
7. **Outbound rules**: Giữ nguyên mặc định (Allow All Traffic).
8. Nhấn **Create security group**.
![SG ALB](./images/5.2.2.1.png)
## Bước 3: Tạo Security Group cho EC2 (Backend)

EC2 backend chỉ nên nhận traffic từ ALB và cho phép bạn SSH vào, không được phép mở kết nối trực tiếp ra internet để tránh rủi ro.

1. Tương tự, nhấn **Create security group**.
2. **Security group name**: `genzite-sg`.
3. **Description**: Tuỳ chọn mô tả, ví dụ `genzite-sg created...`.
4. **VPC**: Chọn `genzite-vpc`.
5. **Inbound rules**:
   - Thêm Rule 1: Type `Custom TCP`, Port Range `3000`, Source chọn **Custom** và tìm kiếm tên SG của ALB là `genzite-alb-sg`, Description: `ALB`.
   - Thêm Rule 2: Type `SSH`, Port Range `22`, Source chọn **My IP**, Description: `MyIP`.
   - Thêm Rule 3: Type `Custom TCP`, Port Range `5173`, Source chọn **Custom** và tìm kiếm tên SG của ALB là `genzite-alb-sg`, Description: `ALB`.
6. **Outbound rules**: Giữ nguyên mặc định (Allow All Traffic) để tải thư viện và gọi ra bên ngoài.
7. Nhấn **Create security group**.

## Bước 4: Tạo Security Group cho RDS (Database)

PostgreSQL Database là nơi chứa dữ liệu quan trọng nên chỉ cho phép duy nhất máy chủ EC2 kết nối tới.

1. Nhấn **Create security group**.
2. **Security group name**: `genzite-rds-sg`.
3. **Description**: `genzite-rds-sg`.
4. **VPC**: Chọn `genzite-vpc`.
5. **Inbound rules**:
   - Thêm Rule 1: Type `PostgreSQL`, Port Range `5432`, Source chọn **Custom** và tìm kiếm SG của EC2 là `genzite-sg`.
6. Nhấn **Create security group**.

---

