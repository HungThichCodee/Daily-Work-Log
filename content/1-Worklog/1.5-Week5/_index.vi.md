---
title: "Worklog Tuần 5"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

* CloudFormation: Tập dùng code để tự động tạo tài nguyên AWS thay vì ngồi click chuột thủ công.
* CDK: Nâng cấp lên xịn hơn, dùng các ngôn ngữ lập trình thật (như TypeScript, Python...) để xây dựng hạ tầng.
* VPC Flow Logs: Học cách theo dõi lưu lượng IP ra/vào xem mạng mẽo hoạt động thế nào.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2 | - Tìm hiểu AWS CloudFormation (Cơ bản):<br>&emsp; + Chuẩn bị tài nguyên: Tạo IAM User và IAM Role.<br>&emsp; + Tạo workspace và thiết lập môi trường trên VS Code.<br>&emsp; + Viết và triển khai CloudFormation Template cơ bản. | 18/05/2026 | 18/05/2026 | <https://000037.awsstudygroup.com/> |
| 3 | - Tìm hiểu AWS CloudFormation (Nâng cao):<br>&emsp; + Thực hành Custom Resources (tạo Lambda function).<br>&emsp; + Tìm hiểu về Mappings, Stacksets và phát hiện trôi dạt (Drift Detection).<br>&emsp; + Dọn dẹp tài nguyên CloudFormation. | 19/05/2026 | 19/05/2026 | <https://000037.awsstudygroup.com/> |
| 4 | - Bắt đầu với AWS CDK (Cơ bản):<br>&emsp; + Hiểu khái niệm CDK (hoạt động dựa trên CloudFormation) và dùng ngôn ngữ lập trình để định nghĩa kiến trúc.<br>&emsp; + Chuẩn bị IAM Role và cấu hình môi trường làm việc trên VS Code.<br>&emsp; + Tạo workspace cho CDK. | 20/05/2026 | 20/05/2026 | <https://000038.awsstudygroup.com/> |
| 5 | - Triển khai và Cập nhật AWS CDK:<br>&emsp; + Khởi tạo CDK Template để triển khai kiến trúc cơ bản lên AWS.<br>&emsp; + Thực hành cập nhật (Update) CDK Template.<br>&emsp; + Dọn dẹp tài nguyên CDK. | 21/05/2026 | 21/05/2026 | <https://000038.awsstudygroup.com/> |
| 6 | - Giám sát hạ tầng mạng với VPC Flow Logs:<br>&emsp; + Tìm hiểu tính năng theo dõi lưu lượng IP ra/vào mạng thông qua VPC Flow Logs.<br>&emsp; + Tạo và kích hoạt VPC Flow Logs, xuất dữ liệu log về Amazon CloudWatch Logs.<br>&emsp; + Giám sát hạ tầng mạng và dọn dẹp tài nguyên. | 22/05/2026 | 22/05/2026 | <https://000074.awsstudygroup.com/> |


### Kết quả đạt được tuần 5:

* CloudFormation:
  * Đã biết viết các file template cơ bản trên VS Code và chạy thử thành công để tự động tạo tài nguyên.

* CDK:
  * Khởi tạo được môi trường làm việc (workspace) và gõ code đẩy được cái kiến trúc cơ bản lên AWS.

* VPC Flow Logs:
  * Biết cách bật tính năng ghi nhật ký mạng (Flow Logs) đẩy về CloudWatch để xem, đặc biệt là không sợ làm mạng bị lag hay chậm đi.


