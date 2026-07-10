---
title: "Worklog Tuần 6"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---



### Mục tiêu tuần 6:

* AWS Backup: Học cách dùng Machine Learning để phát hiện mã độc hoặc bất thường trong dữ liệu sao lưu.
* Amazon DLM: Tự động hóa việc giữ các bản sao lưu dài hạn (trên 90 ngày) để tiết kiệm chi phí và tuân thủ quy định.
* IAM Boundary: Giới hạn quyền tối đa của tài khoản để ngăn chặn rủi ro hacker leo thang đặc quyền.
* Service Quotas: Biết cách yêu cầu AWS nới lỏng các giới hạn tài nguyên mặc định khi hệ thống cần.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2 | - Phân tích AWS Backup:<br>&emsp; + Đánh giá luồng hoạt động của AWS Backup.<br>&emsp; + Kết hợp với EventBridge, Lambda, DynamoDB.<br>&emsp; + Mục đích tự động hóa xử lý sự kiện.<br>&emsp; + Phân tích cơ chế trích xuất block thay đổi. | 25/05/2026 | 25/05/2026 | <https://000089.awsstudygroup.com/> |
| 3 | - Phân tích Machine Learning:<br>&emsp; + Ứng dụng CloudWatch Machine Learning.<br>&emsp; + Phát hiện bất thường từ dữ liệu block.<br>&emsp; + Bảo vệ hệ thống lưu trữ dữ liệu.<br>&emsp; + Chống tấn công mạng và ransomware. | 26/05/2026 | 26/05/2026 | <https://000089.awsstudygroup.com/> |
| 4 | - Phân tích Amazon DLM:<br>&emsp; + Lợi ích của EBS Snapshots Archive.<br>&emsp; + Giữ dữ liệu dài hạn trên 90 ngày.<br>&emsp; + Thiết lập chính sách tự động (DLM).<br>&emsp; + Giảm rủi ro mất dữ liệu do con người. | 27/05/2026 | 27/05/2026 | <https://000088.awsstudygroup.com/> |
| 5 | - Phân tích IAM Boundary:<br>&emsp; + Khái niệm Permission Boundary.<br>&emsp; + Giới hạn quyền tối đa của User/Group.<br>&emsp; + Kết hợp với Identity-based policy.<br>&emsp; + Ngăn chặn lỗi leo thang đặc quyền. | 28/05/2026 | 28/05/2026 | <https://000030.awsstudygroup.com/> |
| 6 | - Phân tích Service Quotas:<br>&emsp; + Đánh giá hạn mức tài nguyên (quotas).<br>&emsp; + Tìm hiểu giá trị mặc định của AWS.<br>&emsp; + Đánh giá nhu cầu thực tế của hệ thống.<br>&emsp; + Quy trình yêu cầu hỗ trợ tăng hạn mức. | 29/05/2026 | 29/05/2026 | <https://000063.awsstudygroup.com/> |


### Kết quả đạt được tuần 6:

* AWS Backup:
  * Hiểu cách hệ thống tự động so sánh các bản snapshot để tìm ra số block dữ liệu bị thay đổi.

* Amazon DLM:
  * Biết cách tạo chính sách tự động lưu/xóa, loại bỏ được các script rườm rà dễ gây lỗi mất dữ liệu.

* IAM Boundary:
  * Nắm rõ cách kết hợp Permission Boundary với các chính sách (Policy) để khóa chặt quyền hạn.

* Service Quotas:
  * Hiểu quy trình tra cứu hạn mức và các bước gửi yêu cầu cho AWS Support để xin thêm tài nguyên.


