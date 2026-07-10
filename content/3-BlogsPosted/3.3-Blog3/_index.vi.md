---
title: "Định Danh Chi Phí GPU Trong Amazon EKS"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---
## Định Danh Và Phân Bổ Chi Phí GPU Trong Amazon EKS Để Tối Ưu FinOps

Chào anh em AWS Study Group VN! Trong quá trình tìm hiểu về tối ưu hóa chi phí hạ tầng (FinOps) cho các hệ thống AI/ML, mình có đọc được một bài viết khá thú vị từ AWS Blog về một giải pháp giải quyết bài toán: **Định danh và phân bổ chi phí GPU trên Amazon EKS (GPU Cost Attribution)**. 

Điều mình thấy đáng chú ý là khi các doanh nghiệp chạy đua cấu hình hạ tầng cho AI, chi phí GPU thường tăng phi mã nhưng việc quản lý, bóc tách "bill" cho từng team lại cực kỳ mập mờ do đặc thù tài nguyên GPU khó chia nhỏ hơn CPU/RAM truyền thống.

## 1. Bài toán đau đầu về quản lý chi phí GPU
Khi triển khai các mô hình Machine Learning hoặc Inference trên AWS, chúng ta thường dựng các cluster Amazon EKS lớn và dùng chung cho nhiều phòng ban để tối ưu tài nguyên. Ví dụ trong một doanh nghiệp:
- **Team A (Research)**: Chạy thử nghiệm mô hình mới.
- **Team B (Production AI)**: Chạy inference phục vụ người dùng thật.
- **Team C (Data Science)**: Xử lý các tập dữ liệu lớn.

Tất cả các workload này đều chia sẻ chung một pool node GPU (như các instance P4 hay G5). Bài toán lúc này dành cho Đội ngũ vận hành/FinOps là: *Làm sao biết chính xác Pod nào, Namespace nào thuộc Team nào đang tiêu tốn bao nhiêu tiền GPU?*

Nếu không có một cơ chế định danh chi phí rõ ràng, doanh nghiệp sẽ đối mặt với:
- **Chi phí ẩn**: Phải chia bill theo kiểu "bổ đầu người" hoặc ước lượng thủ công, không phản ánh đúng thực tế sử dụng. 
- **Lãng phí tài nguyên (Over-provisioning)**: Các team có xu hướng khai báo dư thừa tài nguyên "cho chắc cú", dẫn đến GPU chạy không tải nhưng tiền vẫn phải trả. 
- **Khó tối ưu**: Không chỉ mặt đặt tên được đâu là phần tài nguyên đang bị lãng phí để cắt giảm.

## 2. Giải pháp đề xuất từ AWS
Điểm hay của bài viết là AWS đưa ra một mô hình phân tách chi phí GPU thành 3 lớp rất tường minh dựa trên các công cụ Observability mã nguồn mở:
- **Allocated Cost (Chi phí cấp phát)**: Tính dựa trên lượng tài nguyên mà Pod yêu cầu (Kubernetes resource requests).
- **Effective Cost (Chi phí thực tế)**: Tính dựa trên mức độ sử dụng GPU thực tế của Pod đó (Actual utilization).
- **Waste Cost (Chi phí lãng phí)**: Khoảng chênh lệch giữa lượng GPU được cấp phát và lượng GPU thực sự dùng. Đây chính là phần dữ liệu "vàng" để DevOps tối ưu cấu hình.

Về mặt kỹ thuật, kiến trúc này tận dụng tính năng Multi-Instance GPU (MIG) của NVIDIA để chia nhỏ GPU vật lý, sau đó dùng các công cụ thu thập metric từ mức phần cứng lên đến mức Kubernetes Pod:

![Architecture flow](/images/3-BlogsPosted/3.3-Blog3/fig1.png)
*Hình 1: Luồng kiến trúc thu thập metric*

- **NVIDIA DCGM Exporter**: Thu thập chỉ số phần cứng GPU (hiệu suất, bộ nhớ). 
- **Kube-State-Metrics**: Cung cấp ngữ cảnh của Kubernetes (Pod nào thuộc Namespace/Team nào qua các nhãn Labels). 
- **OpenTelemetry Collector**: Đóng vai trò làm pipeline chuẩn hóa, gộp hai nguồn metric trên lại và đẩy về Prometheus. 
- **Amazon Managed Grafana**: Trực quan hóa toàn bộ dữ liệu lên Dashboard để theo dõi realtime chi phí của từng Business Unit (BU).

![Grafana Dashboard](/images/3-BlogsPosted/3.3-Blog3/fig2.png)
*Hình 2: Dashboard trên Grafana hiển thị phân bổ chi phí*

## 3. Những dịch vụ AWS xuất hiện trong kiến trúc
- **Amazon Elastic Kubernetes Service (Amazon EKS)**
- **AWS Distro for OpenTelemetry (ADOT)**
- **Amazon Managed Service for Prometheus**
- **Amazon Managed Grafana**
- **AWS IAM** (sử dụng IRSA để phân quyền bảo mật cho các service thu thập metric)

## 4. Bài học rút ra (Key Learnings)
Điều mình thấy thú vị nhất từ bài viết này là quản lý chi phí cho kỷ nguyên AI/ML không đơn thuần là bật AWS Budgets lên xem tổng tiền cuối tháng.
- **FinOps cần gắn liền với Observability**: Muốn tối ưu chi phí chính xác, bạn buộc phải có số liệu giám sát kỹ thuật ở mức độ chi tiết (Granular visibility) đến từng Pod. 
- **Tận dụng Managed Open Source**: Việc AWS cung cấp các bản Managed cho Prometheus và Grafana giúp giảm đáng kể gánh nặng vận hành pipeline dữ liệu metric cho đội DevOps, giúp họ chỉ tập trung vào phân tích dashboard.
- **Thay đổi tư duy cấp phát**: Nhìn vào biểu đồ "Waste Cost", đội vận hành có cơ sở dữ liệu thuyết phục để yêu cầu các team Data Science tối ưu lại cấu hình định mức tài nguyên, tránh lãng phí "tiền đô" cho các node GPU đắt đỏ.

## Kết luận
Giải pháp kiến trúc này cung cấp một công cụ tuyệt vời để định danh và phân bổ rõ ràng chi phí GPU cho từng nhóm sử dụng trên Amazon EKS, mang lại khả năng hiển thị chi tiết (visibility) và thúc đẩy việc tối ưu hóa thực tiễn (FinOps). 

*Tác giả bài viết gốc: Siva Guruvareddiar.*
*Tài liệu gốc: [GPU cost attribution in Amazon EKS using Amazon Managed Service for Prometheus, Amazon Managed Grafana, and OpenTelemetry](https://aws.amazon.com/vi/blogs/mt/gpu-cost-attribution-in-amazon-eks-using-amazon-managed-service-for-prometheus-amazon-managed-grafana-and-opentelemetry/)*