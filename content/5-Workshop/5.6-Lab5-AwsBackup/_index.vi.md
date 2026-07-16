---
title: "Lab 5: AWS Backup"
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

Trong bài Lab này, chúng ta sẽ thiết lập dịch vụ **AWS Backup** để tự động sao lưu dữ liệu cho các tài nguyên quan trọng của hệ thống Genzite (như RDS) nhằm đảm bảo an toàn dữ liệu và khả năng phục hồi khi có sự cố.

## Bước 1: Tạo Backup Plan và Backup Vault

Backup Plan định nghĩa lịch trình và quy tắc sao lưu tự động, còn Backup Vault là nơi lưu trữ các bản sao lưu (Recovery points) một cách an toàn.

1. Truy cập dịch vụ **AWS Backup** trên AWS Console. Nhấn **Create backup plan**.
![Create backup plan](/images/AWS_backup/backup1.jpg)
2. Chọn **Build a new plan**. 
   - **Backup plan name**: Nhập `genzite-backup-plan`.
   - **Backup rule name**: Nhập `daily-backup`.
![Build a new plan](/images/AWS_backup/backup2.jpg)
3. Tại phần **Backup vault**, chọn tạo Vault mới (Create new vault):
   - **Vault name**: Nhập `genzite-backup-vault`.
   - Nhấn tạo Vault để hoàn tất.
![Create Vault](/images/AWS_backup/backup3.jpg)
4. Thiết lập lịch trình (Backup window):
   - Thay đổi **Time zone** sang `Asia/Saigon (UTC+07:00)` (hoặc múi giờ mong muốn).
![Schedule](/images/AWS_backup/backup4.jpg)
5. Cấu hình thời gian lưu trữ (Lifecycle):
   - **Total retention period**: Chọn `7 Days`.
   - Kiểm tra lại cấu hình và nhấn **Create plan**.
![Lifecycle](/images/AWS_backup/backup5.jpg)

## Bước 2: Chỉ định tài nguyên sao lưu (Assign Resources)

Sau khi tạo Backup Plan thành công, bạn sẽ được chuyển đến trang Assign resources để chỉ định tài nguyên áp dụng chính sách này.

1. Bắt đầu cấu hình chỉ định tài nguyên:
![Assign resources](/images/AWS_backup/backup6.jpg)
2. Cấu hình chung:
   - **Resource assignment name**: Nhập `genzite-resource`.
   - **IAM role**: Chọn **Default role**.
   - **Define resource selection**: Chọn **Include specific resource types**.
![Resource assignment details](/images/AWS_backup/backup7.jpg)
3. Chọn tài nguyên cụ thể:
   - **Resource type**: Chọn `RDS`.
   - **Database names**: Chọn `genzite-db`.
   - Cuối cùng, nhấn **Assign resources** để hoàn tất quá trình thiết lập tự động.
![Select resources](/images/AWS_backup/backup8.jpg)

## Bước 3: Tạo bản sao lưu thủ công (On-demand Backup)

Ngoài việc sao lưu tự động, bạn có thể tạo bản sao lưu ngay lập tức bất cứ lúc nào.

1. Điều hướng đến phần tạo On-demand backup. Cấu hình:
   - **Resource type**: `RDS`.
   - **Database name**: `genzite-db`.
   - **Total retention period**: `7 Days`.
![On-demand Settings](/images/AWS_backup/backup9.jpg)
2. Cấu hình Vault lưu trữ:
   - **Backup vault**: Chọn `genzite-backup-vault`.
   - **IAM role**: Chọn **Default role**.
   - Nhấn **Create on-demand backup**.
![On-demand Vault](/images/AWS_backup/backup10.jpg)
3. Kiểm tra tiến trình trong phần **Jobs**. Chờ đến khi trạng thái (Status) chuyển sang `Completed` là bản sao lưu của bạn đã sẵn sàng!
![Jobs completed](/images/AWS_backup/backup11.jpg)
