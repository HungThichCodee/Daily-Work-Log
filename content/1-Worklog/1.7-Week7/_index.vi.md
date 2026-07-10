---
title: "Worklog Tuần 7"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---


### Mục tiêu tuần 7:

* Security Hub: Gom nhóm và quản lý cảnh báo bảo mật tập trung.
* IAM Condition: Giới hạn quyền truy cập chặt chẽ theo khung giờ và địa chỉ IP.
* IAM Boundary: Đặt mức trần quyền hạn để chặn lỗi leo thang đặc quyền.
* VPC Endpoints: Tạo đường mạng riêng tư để truy cập S3 mà không qua Internet.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2 | - Thực hành AWS Security Hub:<br>&emsp; + Bật tính năng AWS Security Hub.<br>&emsp; + Gom các cảnh báo bảo mật về chung một bảng điều khiển.<br>&emsp; + Xem điểm số bảo mật của hệ thống.<br>&emsp; + Dọn dẹp tài nguyên. | 01/06/2026 | 01/06/2026 | <https://000018.awsstudygroup.com/> |
| 3 | - Quản lý quyền với IAM Role & Condition:<br>&emsp; + Tạo các nhóm (Group) và người dùng (User) cơ bản.<br>&emsp; + Cấu hình mượn quyền (Switch role) cho Admin.<br>&emsp; + Cài đặt khóa quyền chỉ cho phép truy cập đúng IP.<br>&emsp; + Khóa quyền chỉ cho phép truy cập theo khung giờ.<br>&emsp; + Dọn dẹp tài nguyên. | 02/06/2026 | 02/06/2026 | <https://000044.awsstudygroup.com/> |
| 4 | - Chặn quyền với IAM Permission Boundary:<br>&emsp; + Tạo các chính sách giới hạn quyền hạn (Restriction Policy).<br>&emsp; + Gắn ranh giới quyền này cho User.<br>&emsp; + Thử đăng nhập và test xem User có bị chặn quyền không.<br>&emsp; + Dọn dẹp tài nguyên. | 03/06/2026 | 03/06/2026 | <https://000030.awsstudygroup.com/> |
| 5 | - Kết nối S3 riêng tư (Phần 1 - VPC):<br>&emsp; + Tạo cổng kết nối Gateway Endpoint.<br>&emsp; + Kết nối đến S3 trực tiếp từ trong mạng VPC.<br>&emsp; + Đảm bảo dữ liệu không bị tuồn ra ngoài Internet.<br>&emsp; + Dọn dẹp tài nguyên. | 04/06/2026 | 04/06/2026 | - |
| 6 | - Kết nối S3 riêng tư (Phần 2 - On-premises):<br>&emsp; + Chuẩn bị và giả lập mạng nội bộ (On-premises).<br>&emsp; + Tạo cổng kết nối Interface Endpoint.<br>&emsp; + Dùng DNS để truy cập S3 an toàn từ mạng nội bộ.<br>&emsp; + Dọn dẹp tài nguyên. | 05/06/2026 | 05/06/2026 | - |


### Kết quả đạt được tuần 7:

* Security Hub:
  * Bật dịch vụ thành công và xem được điểm số bảo mật.

* IAM Condition:
  * Cấu hình thành công việc ép người dùng mượn quyền (switch role) đúng IP và giờ.

* IAM Boundary:
  * Gắn thành công giới hạn quyền tối đa, User không thể tự ý vượt quyền.

* VPC Endpoints:
  * Tạo thành công các cổng mạng nội bộ (Gateway và Interface) kết nối an toàn tới S3.


