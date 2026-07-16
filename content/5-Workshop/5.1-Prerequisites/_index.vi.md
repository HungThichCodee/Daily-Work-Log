---
title: "Chuẩn bị"
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---


Để việc thực hành bài Workshop diễn ra suôn sẻ, bạn cần chuẩn bị một số yêu cầu cơ bản về tài khoản và môi trường AWS.

## 1. Tài khoản AWS

Bạn cần có một tài khoản AWS để có thể tạo các tài nguyên như VPC, EC2, IAM, v.v. Nếu chưa có, bạn có thể đăng ký tài khoản AWS Free Tier theo hướng dẫn từ trang chủ của AWS.

> [!WARNING]
> Mặc dù bài Lab được thiết kế cố gắng sử dụng các dịch vụ nằm trong hạn mức Free Tier, việc cấu hình sai hoặc sử dụng một số tài nguyên không có Free Tier (như NAT Gateway) có thể phát sinh một chút chi phí. Bạn hãy chú ý làm theo bước dọn dẹp (Cleanup) ở cuối Workshop để xoá tài nguyên sau khi thực hành xong.

## 2. Chọn Region

Trong toàn bộ bài Lab này, chúng tôi sẽ sử dụng Region **US East (N. Virginia) `us-east-1`**. 

Bạn hãy đảm bảo chọn đúng Region này ở thanh công cụ góc trên bên phải của AWS Console trước khi bắt đầu tạo bất kỳ tài nguyên nào để tránh lỗi không liên kết được các tài nguyên với nhau ở các phần sau.

## 3. Trình duyệt web

Khuyến nghị sử dụng các trình duyệt web hiện đại (Google Chrome, Firefox, Microsoft Edge, Safari) ở phiên bản mới nhất để giao diện AWS Management Console hoạt động tốt nhất.

## 4. Tài khoản IAM (Khuyến nghị)

Theo Best Practice về bảo mật của AWS, bạn **không nên** sử dụng tài khoản Root để thực hành. Hãy tạo một IAM User với quyền **AdministratorAccess** và đăng nhập bằng tài khoản IAM đó để tiến hành bài lab.

---
**Sẵn sàng chưa?** Nếu bạn đã đăng nhập thành công vào AWS Console với Region `us-east-1`, hãy chuyển sang phần tiếp theo để bắt đầu **Cấu hình VPC**.
