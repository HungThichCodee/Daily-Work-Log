---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Genzite: Nền tảng No-Code tạo giao diện web bằng AI
## Giải pháp hạ tầng AWS Cloud-Native để sinh & triển khai Frontend tự động từ ngôn ngữ tự nhiên

### 1. Tóm tắt điều hành
Genzite là nền tảng AI No-Code cho phép người dùng không có kiến thức lập trình tự tạo và vận hành một trang web giao diện hoàn chỉnh chỉ bằng cách mô tả yêu cầu bằng ngôn ngữ tự nhiên. Thay vì cung cấp các mẫu (template) cứng nhắc, Genzite sử dụng AI (Google Gemini) để tự động phân tích prompt và sinh ra cấu trúc trang web dưới dạng JSON, sau đó render thành giao diện React thực tế ngay trong trình duyệt.

Hệ thống được triển khai hoàn toàn trên AWS với kiến trúc tách biệt rõ ràng: **Frontend React SPA** được lưu trữ trên **Amazon S3** và phân phối toàn cầu qua **Amazon CloudFront**, trong khi **API backend NestJS** chạy trên **EC2** đảm nhận việc điều phối AI, quản lý hàng đợi tác vụ và lưu trữ dữ liệu vào **Amazon RDS PostgreSQL**. Bảo mật tài khoản được xử lý bởi **Amazon Cognito**. Tác vụ sinh web bằng AI được xử lý **bất đồng bộ** qua **BullMQ + Amazon ElastiCache Redis**, tiến trình trả về client theo thời gian thực qua **SSE stream**. Các sự kiện nội bộ giữa các service được truyền qua **Apache Kafka**.

Phạm vi triển khai tập trung vào luồng cốt lõi: **Người dùng nhập prompt → Job được đẩy vào BullMQ → AI Worker gọi Gemini sinh JSON layout → Hệ thống lưu & render giao diện → Người dùng chỉnh sửa canvas**.

---

### 2. Tuyên bố vấn đề
#### Vấn đề hiện tại
Việc xây dựng một trang web tùy chỉnh đòi hỏi kiến thức lập trình chuyên sâu về HTML, CSS, JavaScript và chi phí thuê nhân sự không nhỏ. Các công cụ kéo thả phổ biến (WordPress, Wix, Squarespace) tuy dễ dùng nhưng bị ràng buộc bởi các bố cục cố định, không thể tùy biến sâu và không tích hợp được khả năng sinh giao diện tự động từ mô tả văn bản.

Người dùng không kỹ thuật hiện nay ít có công cụ nào cho phép họ:
- Mô tả một trang web bằng lời nói và nhận lại giao diện đã hoàn chỉnh
- Chỉnh sửa từng thành phần giao diện trực tiếp trên canvas
- Tự quản lý nội dung trang web mà không cần viết code

#### Giải pháp
Genzite giải quyết bài toán này bằng luồng xử lý bốn bước:

1. **Sinh layout bằng AI (Bất đồng bộ)**: Người dùng nhập prompt, API nhận yêu cầu và lập tức đẩy một **Job** vào hàng đợi **BullMQ** (chạy trên **Amazon ElastiCache Redis**), trả về `jobId` ngay lập tức mà không bắt người dùng chờ. **AI Worker** chạy ngầm pop job ra, gọi **Google Gemini API** để sinh JSON layout, lưu kết quả và phát sự kiện hoàn thành.
2. **Theo dõi tiến trình (SSE)**: Frontend mở kết nối **Server-Sent Events** dựa trên `jobId`. Khi AI Worker hoàn tất, SSE stream đẩy thông báo `100% Done` về client.
3. **Render & Chỉnh sửa**: Frontend nhận JSON layout và render thành canvas giao diện. Người dùng kéo, thả, chỉnh sửa từng widget trực quan.
4. **Sự kiện liên service (Kafka)**: Khi một trang web mới được tạo, **Site Service** phát sự kiện `SiteCreated` lên **Apache Kafka**. Các service khác (như Notification Service) lắng nghe event này để gửi email chào mừng mà không làm chậm luồng chính.

Toàn bộ tài nguyên tĩnh của ứng dụng React được lưu trên **S3** và phân phối qua **CloudFront** nhằm đảm bảo tốc độ tải trang nhanh toàn cầu. Dữ liệu JSON layout được lưu bền vững trong **RDS PostgreSQL** thông qua API backend trên **EC2**.

#### Lợi ích
- **Không cần code**: Người dùng hoàn toàn không cần biết lập trình để tạo ra một trang web có bố cục chuyên nghiệp.
- **Tốc độ**: Từ prompt đến giao diện hoàn chỉnh trong vòng dưới 5 phút.
- **Chi phí vận hành thấp**: Nhờ tận dụng S3 + CloudFront để phục vụ tài nguyên tĩnh và chỉ dùng EC2 tối giản cho phần API, chi phí hạ tầng MVP ước tính chỉ từ **~$30–$50/tháng**.

---

### 3. Kiến trúc giải pháp

#### Dịch vụ AWS sử dụng

| Dịch vụ AWS | Vai trò trong hệ thống |
|---|---|
| **Amazon Route 53** | Quản lý tên miền và phân giải DNS tới CloudFront |
| **Amazon CloudFront** | Cache và phân phối React SPA toàn cầu, tích hợp SSL/TLS từ ACM |
| **Amazon S3** | Lưu trữ toàn bộ tài nguyên tĩnh của ứng dụng React (JS, CSS, HTML) |
| **Amazon Cognito** | Xác thực tài khoản người dùng, cấp phát JWT Token |
| **Application Load Balancer (ALB)** | Nhận API request, giải mã SSL, chuyển tiếp tới EC2 |
| **Amazon EC2** | Chạy NestJS API server xử lý sinh layout và lưu dữ liệu |
| **Amazon RDS PostgreSQL** | Lưu trữ JSON layout và thông tin trang web của người dùng |
| **Amazon ElastiCache Redis** | Chạy hàng đợi BullMQ cho AI Worker và cache kết quả prompt trùng lặp |
| **NAT Gateway** | Cho phép EC2 trong private subnet gọi ra Gemini API an toàn |
| **AWS Certificate Manager** | Cấp phát và gia hạn chứng chỉ SSL/TLS tự động |

#### Thiết kế thành phần

Hệ thống backend bao gồm **4 service cốt lõi**, giao tiếp với nhau qua **Apache Kafka** (event bus) và xử lý tác vụ nặng qua **BullMQ**:

- **Identity Service**: Tích hợp Amazon Cognito để đăng ký, đăng nhập và cấp phát JWT. Quản lý workspace của từng người dùng.
- **Site Service**: Lưu/đọc cấu hình trang web (JSON layout gồm danh sách Page và Widget) vào/từ PostgreSQL. Phát sự kiện `SiteCreated` lên Kafka sau khi lưu thành công.
- **AI Service**: Nhận prompt → đẩy **Job vào hàng đợi BullMQ** → AI Worker gọi **Google Gemini API** sinh JSON layout → lưu kết quả → phát SSE `completed` về Frontend. Cache prompt hash trên Redis để tránh gọi lại AI với prompt trùng.
- **Notification Service**: Lắng nghe sự kiện `SiteCreated` từ Kafka để tự động gửi email chào mừng cho người dùng mà không làm ảnh hưởng đến luồng tạo web chính.

---

### 4. Triển khai kỹ thuật

#### Yêu cầu kỹ thuật

| Thành phần | Công nghệ |
|---|---|
| **Frontend** | React 18, Vite, TypeScript, Tailwind CSS v4 |
| **Canvas Editor** | Hệ thống widget kéo-thả (15+ loại widget) |
| **Backend API** | NestJS, Prisma ORM, PostgreSQL |
| **Message Queue** | BullMQ + Amazon ElastiCache Redis (xử lý AI job bất đồng bộ) |
| **Event Bus** | Apache Kafka (giao tiếp giữa các service) |
| **Xác thực** | Amazon Cognito (JWT) |
| **AI Engine** | Google Gemini 2.0 Flash API |
| **Hạ tầng** | AWS EC2, S3, RDS, ElastiCache, CloudFront, ALB, Route 53 |

#### Luồng sinh web bằng AI (Core Flow)

```
Người dùng nhập prompt
        │
        ▼
Frontend gửi POST /api/v1/ai/generate
        │
        ▼
AI Service đẩy Job vào BullMQ (ElastiCache Redis)
        │
        ▼
API trả về HTTP 202 Accepted + jobId (không bắt chờ)
        │
        ▼
Frontend mở SSE stream (/api/v1/ai/stream/:jobId)
        │
        ▼  [Background: AI Worker pop job từ BullMQ]
 AI Worker gọi Google Gemini API sinh JSON Layout
        │
        ▼
Site Service lưu JSON vào RDS PostgreSQL
        │  → phát SiteCreated event lên Kafka
        │     → Notification Service gửi email chào mừng
        ▼
SSE stream đẩy '100% Done' + layout về Frontend
        │
        ▼
Frontend render canvas giao diện
        │
        ▼
Người dùng chỉnh sửa widget trực tiếp
```

#### Cấu trúc JSON Layout (ví dụ)

```json
{
  "siteName": "Coffee Shop Website",
  "subdomain": "coffee-shop",
  "pages": [
    {
      "slug": "/",
      "title": "Home",
      "widgets": [
        { "type": "HeroBanner", "props": { "title": "Welcome to Our Coffee Shop", "bgColor": "#3E2723" } },
        { "type": "ProductGrid", "props": { "columns": 3, "items": [] } },
        { "type": "ContactForm", "props": { "email": "hello@coffee.com" } }
      ]
    }
  ]
}
```

#### Các giai đoạn triển khai

1. **Giai đoạn 1 – Thiết kế hạ tầng & Xác thực (Tuần 1–2)**
   - Thiết lập VPC, Security Group, Public/Private subnet
   - Khởi tạo S3 bucket, CloudFront distribution với OAI
   - Cấu hình Amazon Cognito User Pool và App Client
   - Deploy NestJS trên EC2, kết nối RDS PostgreSQL

2. **Giai đoạn 2 – Tích hợp AI & Canvas Editor (Tuần 3–4)**
   - Xây dựng AI Service: nhận prompt, gọi Gemini API, trả JSON
   - Xây dựng Site Service: CRUD JSON layout trong PostgreSQL
   - Phát triển Canvas Editor trên React (render widget từ JSON)
   - Tích hợp Identity Service (Cognito JWT) vào toàn bộ flow

3. **Giai đoạn 3 – Kiểm thử & Ra mắt (Tuần 5–6)**
   - Kiểm thử end-to-end toàn bộ luồng: đăng nhập → nhập prompt → render canvas → lưu
   - Rà soát Security Group (chỉ ALB truy cập EC2, chỉ EC2 truy cập RDS)
   - Deploy React build lên S3, cấu hình CloudFront cache behavior
   - Cấu hình Route 53 trỏ tên miền vào CloudFront

---

### 5. Lộ trình & Mốc triển khai

| Mốc | Thời gian | Kết quả đầu ra |
|---|---|---|
| **Thiết kế kiến trúc** | Tháng 0 | Sơ đồ hệ thống, ERD database, phân bổ VPC |
| **Hạ tầng lõi & Xác thực** | Tuần 1–2 | EC2 + RDS + Cognito + CloudFront hoạt động |
| **AI Generation & Canvas** | Tuần 3–4 | Luồng prompt → JSON → render canvas hoàn chỉnh |
| **Kiểm thử & Ra mắt** | Tuần 5–6 | Hệ thống chính thức vận hành, domain configured |
| **Tối ưu hóa** | Sau ra mắt | Theo dõi CloudFront cache hit, tối ưu RDS index |

---

### 6. Ước tính ngân sách (Tính theo tháng ~ 730 giờ)

Dựa trên sơ đồ kiến trúc hệ thống, dưới đây là ước tính chi phí hàng tháng (áp dụng bảng giá Region `ap-southeast-1` - Singapore) cho 2 cấu hình: **Cấu hình MVP tối giản** (tiết kiệm, phù hợp chạy demo đồ án) và **Cấu hình đầy đủ** (chuẩn production như trong kiến trúc bạn vẽ).

| Thành phần AWS | Cấu hình MVP Tối giản / Đồ án | Cấu hình Đầy đủ (Theo sơ đồ kiến trúc) |
|---|---|---|
| **Máy chủ (EC2 & EBS)** | Single `t4g.small` ở Public Subnet (~$12–$15/tháng) | Single `t4g.small` ở Private Subnet (~$12–$15/tháng) |
| **Cân bằng tải (ALB)** | ❌ Truy cập trực tiếp IP EC2 (~$0) | ✅ ALB + Chứng chỉ ACM (~$20–$25/tháng) |
| **NAT Gateway** | ❌ Không dùng (~$0) | ✅ Bắt buộc để EC2 trong Private ra Internet (~$42–$48/tháng) |
| **Cơ sở dữ liệu (RDS)** | Single-AZ PostgreSQL `db.t4g.micro` (~$18–$20/tháng) | Single-AZ PostgreSQL `db.t4g.micro` (~$18–$20/tháng) |
| **Bộ nhớ đệm (Redis)** | ❌ Cài Redis thẳng lên EC2 (~$0) | ✅ ElastiCache Redis độc lập (~$15–$18/tháng) |
| **Bảo mật (WAF)** | ❌ Không dùng (~$0) | ✅ AWS WAF lọc Web Traffic (~$6–$10/tháng) |
| **Lưu trữ tĩnh (S3)** | ✅ S3 Frontend & Media (~$1–$3/tháng) | ✅ S3 Frontend & Media (~$1–$3/tháng) |
| **CloudFront & Route53** | ✅ CloudFront Free Tier + DNS (~$0–$1/tháng) | ✅ CloudFront + Route53 Hosted Zone (~$2–$5/tháng) |
| **Các dịch vụ khác** | Cognito, IAM, Backup (~$0) | Cognito, AWS Backup, CloudWatch (~$2–$5/tháng) |
| **Tổng cộng ước tính** | **~$31–$40 / tháng** | **~$104–$128 / tháng** |

- **Chi tiết dự toán chính xác**: [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=58e0506ec76a24dacd2cc6990c65981eba461c97)

---

### 7. Đánh giá rủi ro

#### Ma trận rủi ro

| Rủi ro | Xác suất | Mức ảnh hưởng | Phương án giảm thiểu |
|---|---|---|---|
| **Gemini API trả về JSON sai cấu trúc** | Cao | Cao | Xây dựng JSON Schema validator tại AI Service. Nếu JSON lỗi, tự động gọi lại Gemini lần 2 với prompt sửa lỗi. |
| **Vượt giới hạn gọi Gemini API (Rate Limit)** | Trung bình | Cao | Cache kết quả prompt trùng lặp. Giới hạn tần suất gọi API theo người dùng tại NestJS. |
| **Hiệu năng RDS giảm khi dữ liệu lớn** | Thấp | Trung bình | Tạo index trên các cột `site_id`, `user_id`. Giới hạn kích thước JSON layout tối đa mỗi trang. |
| **Độ trễ phản hồi AI cao (5–15 giây)** | Cao | Trung bình | Toàn bộ luồng AI chạy bất đồng bộ qua BullMQ. Frontend hiển thị progress qua SSE stream thay vì chờ HTTP response. |
| **Mất kết nối SSE giữa chừng** | Thấp | Trung bình | Client tự động reconnect SSE. Kết quả job được cache trên Redis để lấy lại khi reconnect. |

#### Kế hoạch dự phòng
- Nếu Google Gemini gặp sự cố, AI Worker tự động retry job (tối đa 3 lần) theo cơ chế backoff của BullMQ trước khi báo lỗi cho người dùng.
- Nếu ElastiCache Redis gặp sự cố, hệ thống có thể tạm thời chuyển sang xử lý đồng bộ trực tiếp để đảm bảo tính sẵn sàng.
- Hạ tầng AWS được mô tả dưới dạng tài liệu/script (VPC, Security Group, S3 Policy) để có thể rebuild nhanh nếu cần thiết.

---

### 8. Kết quả kỳ vọng

- **Sinh giao diện tự động**: Người dùng nhập một câu mô tả trang web và nhận lại canvas giao diện hoàn chỉnh trong vòng 5 phút.
- **Chỉnh sửa trực quan**: Canvas editor cho phép kéo thả và chỉnh sửa từng widget mà không cần viết bất kỳ dòng code nào.
- **Triển khai AWS chuẩn**: Toàn bộ hệ thống hoạt động trên kiến trúc AWS đảm bảo bảo mật (private subnet, Cognito auth, HTTPS), hiệu năng (CloudFront CDN) và độ ổn định (RDS managed database).
- **Chi phí tối ưu**: Cấu hình MVP vận hành ổn định với chi phí dưới $50/tháng, phù hợp cho giai đoạn demo và đánh giá ban đầu.
