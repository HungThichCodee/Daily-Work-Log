---
title: "Worklog Tuần 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---


### Mục tiêu tuần 3:

* Sử dụng AWS CLI để tương tác và quản lý tài nguyên thay vì chỉ dùng console.
* Tìm hiểu về dịch vụ bộ nhớ đệm trên AWS(Amazon ElastiCache - Redis).
* Tìm hiểu về cách tự động điều chỉnh số lượng máy chủ EC2 dựa trên nhu cầu và cân bằng tải giữa các máy chủ.
* Tìm hiểu dịch vụ phân giải tên miền Amazon Route 53.
* Làm quen với cơ sở dữ liệu NoSQL thông qua Amazon DynamoDB.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu AWS CLI: <br>&emsp; + Cài đặt AWS CLI và thiết lập cấu hình cơ bản  <br>&emsp; + Thực hành khởi tạo EC2 instance thông qua CLI   <br>&emsp; + Dọn dẹp tài nguyên                                                                                         | 04/05/2026   | 04/05/2026      | <https://000011.awsstudygroup.com/> |
| 3   | - Tìm hiểu về Amazon ElastiCache (Redis): <br>&emsp; + Tạo Subnet và Cluster <br>&emsp; +  Kết nối đến Node <br>&emsp; + Thao tác dữ liệu qua SDK <br>&emsp; + Dọn dẹp tài nguyên                                            | 05/05/2026   | 05/05/2026      | <https://000061.awsstudygroup.com/> |
| 4   | - Tìm hiểu Auto Scaling và ELB: <br>&emsp; + Tạo Launch Template <br>&emsp; + cấu hình Load Balancer  <br>&emsp; + Tạo Auto Scaling Group <br>&emsp; + Thử các chế độ Scaling <br> &emsp; + Dọn dẹp tài nguyên | 06/05/2026   | 06/05/2026      | <https://000006.awsstudygroup.com/> |
| 5   | - Tìm hiểu Route 53 & Hybrid DNS: <br>&emsp; + Tạo Inbound/Outbound Endpoints <br>&emsp; + Cấu hình Resolver Rules <br>&emsp; + Kiểm tra kết quả <br>&emsp; + Dọn dẹp tài nguyên                   | 07/05/2026   | 07/05/2026      | <https://000010.awsstudygroup.com/> |
| 6   | - Làm quen Amazon DynamoDB:  <br>&emsp; + Tìm hiểu Core components <br>&emsp; + Tạo Table & Access key <br>&emsp; + Đọc/Ghi/Query dữ liệu               <br>&emsp; + Dọn dẹp tài nguyên                                                                          | 08/05/2026   | 08/05/2026      | <https://000060.awsstudygroup.com/> |


### Kết quả đạt được tuần 3:

* Biết cách sử dụng AWS CLI để tương tác và quản lý tài nguyên.

* Biết cách sử dụng Auto Scaling và Load Balancing:
  * Triển khai Auto Scaling Group để hệ thống có thể tự động thêm hoặc bớt máy chủ EC2

* Tối ưu hóa Cơ sở dữ liệu và Bộ nhớ đệm: 
  * Nắm được kiến thức cốt lõi của dịch vụ cache Redis được quản lý hoàn toàn
  * Biết cách kết nối và thao tác dữ liệu với ElastiCache Redis thông qua SDK

*  Quản lý Tên miền và DNS
  * Nắm được các khái niệm cơ bản về DNS và Route 53, bao gồm Hosted Zone, Record, và DNS Resolution
  * Thực hành cấu hình Inbound/Outbound Resolver để tích hợp với DNS nội bộ

* Làm quen Amazon DynamoDB: 
  * Nắm được các thành phần cốt lõi của DynamoDB, bao gồm Table, Item, Attribute, Primary Key
  * Biết cách tạo Table và thực hiện các thao tác cơ bản như đọc/ghi/truy vấn dữ liệu

