---
title: "Worklog Tuần 8"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---



### Mục tiêu tuần 8:

* Định hướng kiến trúc: Hiểu rõ cách các component trong hệ thống tương tác với nhau từ ngoài Internet vào tận Database.
* Phân rã dịch vụ: Nhận diện và liệt kê đầy đủ tất cả các dịch vụ AWS cần thiết từ sơ đồ.
* Mô hình hóa: Tự tay vẽ lại và làm rõ luồng đi của dữ liệu (Data flow) để trình bày dự án.
* Dự toán ngân sách: Lên được bảng ước tính chi phí vận hành hàng tháng sát với thực tế.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2 | - Nghiên cứu và định hướng dự án:<br>&emsp; + Đọc hiểu luồng xác thực người dùng (Cognito).<br>&emsp; + Phân tích luồng truy cập web tĩnh (CloudFront & S3).<br>&emsp; + Phân tích luồng xử lý dữ liệu động (Route 53 & ALB).<br>&emsp; + Định hướng các yêu cầu về bảo mật và sao lưu. | 08/06/2026 | 08/06/2026 | - |
| 3 | - Liệt kê các dịch vụ AWS liên quan:<br>&emsp; + Mạng & Phân phối: VPC, IGW, NAT Gateway, ALB, Route 53, CloudFront.<br>&emsp; + Bảo mật & Định danh: WAF, Amazon Cognito.<br>&emsp; + Compute & Lưu trữ: EC2, EBS, S3 (Frontend & Media).<br>&emsp; + Data & Quản lý: RDS, Media/Cache, AWS Backup, CloudWatch. | 09/06/2026 | 09/06/2026 | - |
| 4 | - Vẽ sơ đồ luồng kiến trúc (Phần 1 - Mạng & Frontend):<br>&emsp; + Vẽ luồng người dùng (User) gọi đến Route 53 và Cognito.<br>&emsp; + Vẽ luồng tĩnh: CloudFront kết hợp WAF và S3 Frontend.<br>&emsp; + Quy hoạch VPC, chia Public/Private Subnet. | 10/06/2026 | 10/06/2026 | - |
| 5 | - Vẽ sơ đồ luồng kiến trúc (Phần 2 - Backend & Quản lý):<br>&emsp; + Vẽ luồng động: ALB đi vào Private Subnet (EC2).<br>&emsp; + Kết nối EC2 với DB (RDS), Cache, EBS và S3 Media.<br>&emsp; + Hoàn thiện luồng giám sát (CloudWatch) và Backup. | 11/06/2026 | 11/06/2026 | - |
| 6 | - Tính toán chi phí dự án:<br>&emsp; + Xác định thông số cấu hình dự kiến cho từng dịch vụ.<br>&emsp; + Sử dụng AWS Pricing Calculator để nhập số liệu.<br>&emsp; + Xuất bảng dự toán chi phí hàng tháng (Monthly Cost). | 12/06/2026 | 12/06/2026 | - |


### Kết quả đạt được tuần 8:

* Bản phân tích:
  * Nắm vững vai trò của từng cụm (Frontend tĩnh, Backend động, Xác thực, Giám sát).

* Danh sách dịch vụ:
  * Có sẵn layout chi tiết các dịch vụ (VPC, EC2, RDS, Cognito, WAF...) để chuẩn bị triển khai.

* Sơ đồ hoàn chỉnh:
  * Sơ đồ kiến trúc luồng (Architecture Flow) đã được vẽ lại rõ ràng, tách bạch từng thành phần.

* Bảng tính chi phí:
  * Hoàn thành file dự toán chi phí (Estimate Cost) để báo cáo/đánh giá tính khả thi.


