---
title: "Lab 4: Giám sát với CloudWatch"
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

## Tổng quan

Trong phần này, chúng ta sẽ cấu hình và sử dụng **Amazon CloudWatch** để giám sát các tài nguyên của hệ thống Genzite, đặc biệt là xem log của EC2 Backend và theo dõi các chỉ số hoạt động (metrics).

> **Lưu ý phân quyền:** Tương tự việc quản lý quyền IAM trước đó, việc thiết lập giám sát trên CloudWatch là tác vụ thuộc phạm vi chuyên môn của **User C (Application & Storage)**.

## Bước 1: Khởi tạo và xem Dashboard
1. Mở AWS Management Console, tìm dịch vụ **CloudWatch**.
2. Ở menu bên trái, truy cập vào các công cụ như Log groups hoặc Metrics.

## Bước 2: Xem Log của ứng dụng
1. Ở menu bên trái, chọn **Logs** -> **Log groups**.
2. Tìm Log Group của máy chủ EC2 (được đẩy thông qua CloudWatch Agent).
3. Click vào Log stream tương ứng để kiểm tra các luồng log (Lỗi, thông tin Request) đang đổ về.

## Bước 3: Tạo Alarm (Cảnh báo tự động)
1. Ở menu bên trái, chọn **Alarms** -> **In alarm**.
2. Nhấn **Create alarm**.
3. Chọn metric là **CPUUtilization** (Mức sử dụng CPU) của EC2 Backend.
4. Đặt ngưỡng cảnh báo (ví dụ: `> 80%` trong 5 phút liên tục).
5. (Tùy chọn) Cấu hình gửi thông báo qua Amazon SNS để gửi email cho quản trị viên khi hệ thống quá tải.

![Create CloudWatch Alarm](/images/CloudWatch/create_cloudwatch.png)

---
**Chúc mừng!** Bạn đã thiết lập thành công "trung tâm quan trắc" cho Genzite. Nhờ CloudWatch, đội ngũ có thể dễ dàng phát hiện và xử lý sự cố.
