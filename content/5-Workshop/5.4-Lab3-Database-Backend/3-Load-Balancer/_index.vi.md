---
title: "3. Cấu hình Load Balancer"
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---


Vì máy chủ EC2 Backend nằm trong **Private Subnet** (không có Public IP), các ứng dụng Frontend từ internet không thể gọi trực tiếp vào API của chúng ta. 

Giải pháp chuẩn AWS là sử dụng một **Application Load Balancer (ALB)** đặt ở Public Subnet. ALB sẽ nhận HTTP/HTTPS request từ ngoài internet, sau đó "chuyển tiếp" (forward) vào cho máy chủ EC2 ở bên trong một cách an toàn.

## Bước 1: Tạo Target Group

Target Group là một nhóm chứa các máy chủ (EC2) mà ALB sẽ điều phối traffic tới. Chúng ta sẽ tạo 2 Target Group: một cho Backend và một cho Frontend.

### 1.1. Tạo Target Group cho Backend
1. Mở dịch vụ **EC2**, cuộn xuống menu bên trái phần **Load Balancing**, chọn **Target Groups**.
2. Nhấn **Create target group**.
3. **Choose a target type**: Chọn **Instances**.
4. **Target group name**: `genzite-backend-tg`.
![Config Target group](./images/5.4.3.1.png)
5. **Protocol**: `HTTP`. **Port**: `3000` (Port mà Backend API đang chạy).
6. **VPC**: Chọn `genzite-vpc`.
7. **Health checks**: Để mặc định (Protocol: HTTP, Path: `/`).
   *(Lưu ý: API cần có route trả về status code 200 ở đường dẫn `/` để Health check báo Healthy).*
![Config Target group](./images/5.4.3.2.png)
8. Nhấn **Next**.
9. Tại màn hình **Register targets**, chọn máy chủ `genzite-backend` ở danh sách bên dưới.
10. Sửa port thành `3000` và nhấn **Include as pending below**.
11. Cuộn xuống và chọn **Create target group**.

### 1.2. Tạo Target Group cho Frontend
1. Từ màn hình **Target Groups**, tiếp tục nhấn **Create target group** và tạo tương tự như **genzite-backend-tg**.
2. **Choose a target type**: Chọn **Instances**.
3. **Target group name**: `frontend-tg`.
4. **Protocol**: `HTTP`. **Port**: `5173` (Port mà Frontend đang chạy).
5. **VPC**: Chọn `genzite-vpc`.
6. **Health checks**: Để mặc định (Protocol: HTTP, Path: `/`).
7. Nhấn **Next**.
8. Tại màn hình **Register targets**, chọn máy chủ `genzite-backend` ở danh sách bên dưới.
9. Đảm bảo port là `5173` và nhấn **Include as pending below**.
10. Cuộn xuống và chọn **Create target group**.

## Bước 2: Khởi tạo Application Load Balancer

1. Truy cập dịch vụ **EC2**, ở menu bên trái chọn **Load Balancers**.
2. Nhấn **Create load balancer**.
3. Chọn **Application Load Balancer** và nhấn **Create**.
4. **Load balancer name**: `genzite-alb`.
5. **Scheme**: Chọn **Internet-facing**.
![Config Target group](./images/5.4.3.5.png)
6. **Network mapping**:
   - **VPC**: Chọn `genzite-vpc`.
   - **Mappings**: Chọn 2 **Availability Zones** và tương ứng chọn 2 **Public Subnets**.
![Config Target group](./images/5.4.3.6.png)
7. **Security groups**:
   - Chọn `genzite-alb-sg`. (Cấu hình Inbound rule cho phép truy cập HTTP/HTTPS).
8. **Listeners and routing**:
   - **Protocol**: `HTTP`. **Port**: `80`.
   - **Default action**: Chọn Target group `frontend-tg` (để dẫn vào Frontend).
   ![Config Target group](./images/5.4.3.7.png)
9. Các phần còn lại giữ nguyên và nhấn **Create load balancer**.

## Bước 3: Cấu hình Rule chuyển tiếp API

Để các request gọi tới `/api/*` được chuyển vào Backend thay vì Frontend, ta sẽ thêm một Rule cho ALB.

1. Chọn vào Load Balancer `genzite-alb` vừa tạo.
2. Ở tab **Listeners and rules**, click vào phần `1 rule` (hoặc nhấn trực tiếp vào listener HTTP:80).
3. Chọn vào rule **Default** và bấm **Add rule**.
4. Tại phần **Conditions**, bấm **Add condition**, chọn **Path** và điền giá trị là `/api/*`.
5. Kéo xuống phần **Actions**, chọn **Forward to** và chọn Target group `genzite-backend-tg`.
6. Tại **Rule priority**, đặt Priority là `1`.
7. Bấm **Add rule** để lưu.
   ![Config Target group](./images/5.4.3.8.png)

Sau khi tạo thành công, ALB của bạn đã sẵn sàng điều hướng các truy cập giao diện vào Frontend, và các truy cập dữ liệu vào Backend!

---
**Hoàn thành Lab 3!** Bạn đã sở hữu một hạ tầng Backend hoàn chỉnh gồm Database an toàn, máy chủ EC2 bảo mật và ALB điều phối thông minh. Hãy chuyển sang **Lab 4** để tích hợp trí tuệ nhân tạo (Gemini API).
