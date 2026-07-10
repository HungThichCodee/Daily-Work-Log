---
title: "Worklog Tuần 11"
date: 2024-01-01
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---



### Mục tiêu tuần 11:

* CloudFormation & FSx: Biết được lý thuyết triển khai hạ tầng tự động (IaC) và cách quản lý hệ thống lưu trữ tệp Windows Server.
* Vận hành Container: Hiểu rõ vòng đời quản lý ứng dụng Docker trên cụm ECS và các chiến lược cập nhật phiên bản mượt mà.
* Tự động hóa CI/CD: Đánh giá luồng triển khai CI/CD liên tục từ mã nguồn xuống container để giảm thiểu lỗi thủ công.
* Giám sát & Bảo mật: Nắm được cách trực quan hóa dữ liệu giám sát và truy cập máy chủ an toàn không cần mở port SSH.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2 | - Phân tích AWS CloudFormation:<br>&emsp; + Tìm hiểu khái niệm Cơ sở hạ tầng dưới dạng mã (IaC).<br>&emsp; + Phân tích cách dùng code để mô hình hóa và tự động tạo tài nguyên AWS.<br>&emsp; + Đánh giá các tính năng nâng cao: Custom Resources, Mappings, Stacksets. | 29/06/2026 | 29/06/2026 | <https://000037.awsstudygroup.com/> |
| 3 | - Nghiên cứu Amazon FSx for Windows:<br>&emsp; + Phân tích kiến trúc lưu trữ tệp Windows Server chạy trên giao thức SMB.<br>&emsp; + Tìm hiểu cơ chế nhân bản dữ liệu tự động (Multi-AZ).<br>&emsp; + Đánh giá tính năng chống trùng lặp dữ liệu và phân bổ dung lượng. | 30/06/2026 | 30/06/2026 | <https://000025.awsstudygroup.com/> |
| 4 | - Phân tích kiến trúc Amazon ECS:<br>&emsp; + Tìm hiểu cách ECS quản lý và chạy Docker container trên cụm EC2.<br>&emsp; + Phân tích Task Definition và cách tích hợp với Application Load Balancer.<br>&emsp; + Đánh giá chiến lược triển khai không gián đoạn (Blue/Green, Rolling). | 01/07/2026 | 01/07/2026 | <https://000016.awsstudygroup.com/> |
| 5 | - Tìm hiểu CI/CD Pipeline cho ECS:<br>&emsp; + Phân tích luồng CI/CD giúp tự động hóa quá trình triển khai.<br>&emsp; + Đánh giá tích hợp giữa ECS với Gitlab, Github Actions và CodeBuild.<br>&emsp; + Nghiên cứu cách đẩy log của container về S3 thông qua Firelens. | 02/07/2026 | 02/07/2026 | <https://000017.awsstudygroup.com/> |
| 6 | - Đánh giá Giám sát & Quản trị hệ thống:<br>&emsp; + Phân tích khả năng trực quan hóa dữ liệu giám sát của Grafana.<br>&emsp; + Tìm hiểu cách kết nối an toàn vào EC2 bằng System Manager - Session Manager.<br>&emsp; + Đánh giá cơ chế lưu trữ lịch sử phiên hoạt động vào Amazon S3. | 03/07/2026 | 03/07/2026 | <https://000029.awsstudygroup.com/><br><https://000058.awsstudygroup.com/> |


### Kết quả đạt được tuần 11:

* CloudFormation & FSx:
  * Hiểu cách tự động hóa xây dựng tài nguyên AWS và nắm rõ các thành phần kiến trúc lưu trữ tệp Windows độ sẵn sàng cao.

* ECS Container:
  * Phân biệt rõ vai trò của Cluster, Task Definition, Service và luồng chia tải với Load Balancer.

* Luồng CI/CD:
  * Nắm vững lý thuyết triển khai tự động từ các kho lưu trữ mã nguồn (Gitlab/Github/CodeBuild) xuống thẳng các container.

* Quản lý & Giám sát:
  * Hiểu cơ chế phân tích log trực quan của Grafana và quy trình thiết lập VPC Endpoint để quản lý EC2 Private an toàn qua Session Manager.


