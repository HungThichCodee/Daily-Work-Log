---
title: "3. Triển khai Frontend"
weight: 23
chapter: false
pre: "<b>5.2.3. </b>"
---

# 5.2.3 Triển khai Frontend

Đối với các ứng dụng frontend hiện đại (như React, Vue, hoặc Angular SPA), việc triển khai lên **Amazon S3 Bucket** và phân phối qua **Amazon CloudFront** là best practice trong ngành. Nó cung cấp khả năng caching ở edge location toàn cầu, khả năng mở rộng không giới hạn và tính năng chống DDoS tích hợp.



## Hướng dẫn từng bước

### 1. Build source code Frontend
Đầu tiên, hãy build ứng dụng của bạn ở local thành các file tĩnh.

```bash
cd frontend
pnpm install
pnpm run build
```
Quá trình này sẽ sinh ra một thư mục `dist/` hoặc `build/` chứa các file HTML, CSS và JS.

### 2. Tạo S3 Bucket
1. Truy cập **S3 Console**.
2. Nhấn **Create bucket**.
3. **Bucket name**: Chọn một tên duy nhất trên toàn cầu (ví dụ: `workshop-frontend-app-12345`).
4. **Block Public Access settings**: Để nguyên tùy chọn "Block all public access" (chúng ta sẽ dùng CloudFront OAC để truy cập bảo mật).
5. Nhấn **Create bucket**.

![Create Bucket 1](/images/S3_bucket/bucket_fontend/createbucketfontend1.png)
![Create Bucket 2](/images/S3_bucket/bucket_fontend/createbucketfontend2.png)
![Create Bucket 3](/images/S3_bucket/bucket_fontend/createbucketfontend3.png)

### 3. Upload File lên S3
1. Nhấn vào bucket vừa tạo.
2. Nhấn **Upload**.
3. Upload toàn bộ nội dung *bên trong* thư mục `dist/` hoặc `build/`.
4. Nhấn **Upload**.

### 3b. Tạo S3 Media Bucket
Trong hệ thống Genzite, Media Bucket dùng để lưu trữ ảnh/video do người dùng tải lên.
1. Quay lại trang chủ **S3 Console**.
2. Nhấn **Create bucket**.
3. **Bucket name**: Đặt tên (ví dụ: `genzite-media-bucket`).
4. **Object Ownership**: Chọn `ACLs enabled` (nếu muốn dùng public read).
5. **Block Public Access settings**: Bỏ check "Block all public access" để cho phép người dùng xem ảnh công khai. Xác nhận rủi ro.
6. Nhấn **Create bucket**.

![Create Media Bucket 1](/images/S3_bucket/bucket_media/createbucketmedia1.png)
![Create Media Bucket 2](/images/S3_bucket/bucket_media/createbucketmedia2.png)
![Create Media Bucket 3](/images/S3_bucket/bucket_media/createbucketmedia3.png)

Cấu hình CORS cho Media Bucket:
1. Mở Media Bucket > Tab **Permissions**.
2. Cuộn xuống phần **Cross-origin resource sharing (CORS)**, nhấn Edit.
3. Dán đoạn JSON cấu hình CORS sau đây (cho phép GET, PUT, POST) và lưu lại:


![Setup CORS Media](/images/S3_bucket/bucket_media/setupCORSmedia.png)

Cấu hình Bucket Policy để cho phép đọc công khai (Public Read):
1. Vẫn ở tab **Permissions**, cuộn lên **Bucket policy**, nhấn Edit.
2. Dán policy sau đây để cho phép hành động `s3:GetObject` từ mọi nguồn `*`. (Lưu ý: Nhớ thay `YOUR_BUCKET_NAME` bằng tên thật bucket của bạn):


![Setup Bucket Policy Media](/images/S3_bucket/bucket_media/setupbucketpolicymedia.png)

![Test Media](/images/S3_bucket/bucket_media/test_media_db.png)

### 4. Tạo CloudFront Distribution
1. Truy cập **CloudFront Console**.
2. Nhấn **Create Distribution**.
3. **Origin domain**: Chọn S3 bucket của bạn.
4. **Origin access**: Chọn **Origin access control settings (recommended)**.
   - Nhấn **Create control setting** và lưu lại.
5. **Default cache behavior**:
   - **Viewer protocol policy**: Chọn Redirect HTTP to HTTPS.
6. **Web Application Firewall (WAF)**: Chọn "Do not enable security protections" (để tiết kiệm chi phí).
7. **Default root object**: Nhập `index.html`.
8. Nhấn **Create distribution**.

### 5. Cập nhật S3 Bucket Policy
CloudFront sẽ tự động sinh ra một đoạn policy cho S3 bucket để cấp quyền đọc. 
1. Tại giao diện phân phối CloudFront, sau khi tạo xong, bạn sẽ thấy thông báo cập nhật policy, hãy nhấn **Copy policy**.
2. Quay lại S3 bucket (Frontend) của bạn > tab **Permissions**.
3. Chỉnh sửa **Bucket policy**, dán đoạn JSON vào và lưu lại. Đoạn policy đó sẽ có cấu trúc như sau (chỉ cho phép CloudFront đọc dữ liệu):


![Setup Bucket Policy Frontend](/images/S3_bucket/bucket_fontend/setupbucketpolicyfontend.png)

### 6. Kiểm tra ứng dụng Frontend
Khi CloudFront distribution chuyển trạng thái Deploy hoàn tất, hãy copy **Distribution domain name** (ví dụ: `d12345.cloudfront.net`) và dán vào trình duyệt. Ứng dụng frontend của bạn đã hoạt động!

![Test Frontend](/images/S3_bucket/bucket_fontend/test_fontend_db.png)

### 7. Cấu hình Custom Domain với Route 53 và ACM (Tùy chọn)

Để sử dụng tên miền riêng (Custom Domain) cho ứng dụng thay vì domain mặc định của CloudFront, bạn cần cấu hình chứng chỉ bảo mật bằng **AWS Certificate Manager (ACM)** và trỏ bản ghi DNS bằng **Amazon Route 53**.

1. **Xin cấp chứng chỉ ACM**: 
   - Truy cập giao diện **ACM Console** và yêu cầu cấp chứng chỉ public (Request public certificate) cho tên miền của bạn.
   - *Lưu ý quan trọng: Chứng chỉ dùng cho CloudFront bắt buộc phải được tạo ở Region **us-east-1 (N. Virginia)**.*

![Cấu hình ACM](images/acm.png)

2. **Cập nhật CloudFront**: 
   - Mở CloudFront Distribution của bạn, phần **Settings** chọn Edit. 
   - Thêm tên miền của bạn vào **Alternate domain name (CNAME)** và chọn Custom SSL certificate mà bạn vừa tạo ở ACM.

3. **Tạo bản ghi Route 53**: 
   - Truy cập **Route 53**, mở Hosted Zone của tên miền. 
   - Tạo một bản ghi mới (Create record), loại **A record**, bật công tắc **Alias** và trỏ (Route traffic to) tới CloudFront distribution của bạn.

![Cấu hình Route 53](images/route53.png)
