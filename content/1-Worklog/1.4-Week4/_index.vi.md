---
title: "Worklog Tuần 4"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---



### Mục tiêu tuần 4:

* Quản lý EC2 an toàn: Dùng Systems Manager (Session Manager, Patch Manager, Run Command) để kết nối máy chủ, quản lý bản vá và tự động hóa tác vụ.
* Phân phối nội dung tĩnh: Lưu trữ web trên S3 và thiết lập CloudFront để bảo mật, tăng tốc độ phân phối.
* Giám sát hệ thống: Cài đặt phần mềm Grafana để trực quan hóa và theo dõi các chỉ số tài nguyên.
* Tổ chức tài nguyên: Phân loại và tự động hóa quản lý lượng lớn tài nguyên bằng Tags và Resource Groups.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2 | - Tìm hiểu Amazon Systems Manager - Session Manager:<br>&emsp; + Chuẩn bị tài nguyên (VPC, EC2, IAM Role)<br>&emsp; + Kết nối tới các EC2 instances (Public & Private)<br>&emsp; + Quản lý log phiên làm việc (Session logs) & cấu hình Port Forwarding<br>&emsp; + Dọn dẹp tài nguyên | 11/05/2026 | 11/05/2026 | <https://000058.awsstudygroup.com/> |
| 3 | - Tìm hiểu AWS Systems Manager:<br>&emsp; + Chuẩn bị tài nguyên (VPC, EC2 Windows, IAM Role)<br>&emsp; + Cấu hình Patch Manager để quản lý bản vá<br>&emsp; + Thực thi lệnh qua Run Command trên nhiều server<br>&emsp; + Dọn dẹp tài nguyên | 12/05/2026 | 12/05/2026 | <https://000031.awsstudygroup.com/> |
| 4 | - Triển khai CloudFront với S3 Bucket Origin:<br>&emsp; + Tạo S3 bucket và tải lên tệp index.html<br>&emsp; + Cấu hình Amazon CloudFront để tăng tốc phân phối nội dung<br>&emsp; + Dọn dẹp tài nguyên | 13/05/2026 | 13/05/2026 | <https://000094.awsstudygroup.com/> |
| 5 | - Bắt đầu với Grafana cơ bản:<br>&emsp; + Chuẩn bị môi trường: VPC, Security Group, EC2 và IAM<br>&emsp; + Cài đặt Grafana trên Linux EC2<br>&emsp; + Thực hành giám sát tài nguyên (Monitoring)<br>&emsp; + Dọn dẹp tài nguyên | 14/05/2026 | 14/05/2026 | <https://000029.awsstudygroup.com/> |
| 6 | - Quản lý tài nguyên với Tags và Resource Groups:<br>&emsp; + Quản lý, thêm/xóa Tags thông qua Console và CLI<br>&emsp; + Lọc tài nguyên dựa trên Tags<br>&emsp; + Tạo Resource Group dựa trên Tags hoặc CloudFormation<br>&emsp; + Dọn dẹp tài nguyên | 15/05/2026 | 15/05/2026 | <https://000027.awsstudygroup.com/> |


### Kết quả đạt được tuần 4:

* Systems Manager:
  * Đã kết nối thành công EC2 (Public/Private), cấu hình Port Forwarding, lưu log phiên làm việc. Đồng thời dùng Patch Manager và Run Command để quản lý tập trung.

* S3 & CloudFront:
  * Tạo S3 bucket chứa file index.html và cấu hình thành công mạng phân phối nội dung (CDN) CloudFront.

* Grafana:
  * Chuẩn bị xong môi trường, cài đặt thành công Grafana trên Linux EC2 và thiết lập bảng điều khiển giám sát.

* Tags & Resource Groups:
  * Đã biết cách gắn siêu dữ liệu (Tags) thông qua Console/CLI và tạo Resource Groups (dựa trên Tags hoặc CloudFormation) để gom nhóm tài nguyên.


