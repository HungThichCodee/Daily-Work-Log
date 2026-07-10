---
title: "Hiện Đại Hóa Pipeline ML Cho Robot Nông Nghiệp Cùng Aigen"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---
## Hiện Đại Hóa Pipeline Machine Learning Cho Robot Nông Nghiệp Cùng Aigen và Amazon SageMaker AI

Chào mọi người, sau khi nghiên cứu và tổng hợp tài liệu kỹ thuật từ AWS Architecture Blog về cách Aigen hiện đại hóa pipeline machine learning cho đội robot nông nghiệp, mình xin chia sẻ bài tóm tắt các điểm cốt lõi để cộng đồng cùng tham khảo.

## 1. Vai trò và bối cảnh nghiên cứu
Aigen là công ty xây dựng robot nông nghiệp tự hành, sử dụng AI thị giác máy tính để tự động nhận diện và loại bỏ cỏ dại kháng thuốc diệt cỏ mà không cần hóa chất, vận hành bằng năng lượng tái tạo, đồng thời cung cấp dữ liệu thực địa theo thời gian thực giúp nông dân ra quyết định tốt hơn. Khi đội robot mở rộng quy mô, hạ tầng on-premise (tại chỗ) của Aigen trở thành nút thắt cổ chai cho việc scale pipeline xây dựng mô hình. Bốn thách thức chính mà nghiên cứu này tập trung giải quyết:
- **Kết nối hạn chế**: Internet không ổn định ở vùng nông thôn gây khó khăn cho giao tiếp giữa robot và cloud.
- **Chi phí gán nhãn dữ liệu cao**: Gán nhãn thủ công hàng nghìn mẫu dữ liệu mới mỗi ngày tốn kém và mất nhiều thời gian.
- **Năng lực tính toán hạn chế**: Huấn luyện mô hình edge chuyên biệt và fine-tune foundation model trên hạ tầng tại chỗ (máy RTX 3090) bị giới hạn bởi khả năng song song và sức mạnh GPU.
- **Vấn đề khả năng mở rộng**: Việc huấn luyện mô hình và gán nhãn batch inference phải tranh nhau cùng cụm máy RTX 3090, gây trễ cho cả nhóm khoa học dữ liệu và nhóm gán nhãn.

## 2. Các điểm nổi bật về kỹ thuật
Đột phá của Aigen nằm ở việc chuyển từ hạ tầng on-premise sang kiến trúc cloud-native dựa trên AWS, với các đặc điểm kỹ thuật nổi bật:

- **Kiến trúc mô hình 4 tầng**: Foundation models (SAM2, Grounding DINO...) → Expert models (Vision Transformer/CNN, chục triệu tham số) → Student models (FP32, dưới 1.5 triệu tham số, tối ưu bằng quantization-aware training) → Edge models (1–1.2 triệu tham số, lượng tử hóa INT8, chạy trên NPU 2.3 TOPS chỉ ~1.5W).

![Aigen Model Architecture](/images/3-BlogsPosted/3.2-Blog2/fig1.png)
*Hình 1: Kiến trúc mô hình phân tầng của Aigen*

- **Edge Computing qua AWS IoT Core**: Robot dùng AWS IoT Core để đẩy dữ liệu (video, telemetry, metadata) lên Amazon S3 an toàn ngay cả khi kết nối yếu.
- **Pipeline dữ liệu tự động**: Dữ liệu được xử lý qua ETL, sau đó gán nhãn tự động bằng một ensemble các vision foundation model (Grounding DINO, Owl-ViT, SAM2, CLIPSeg) kết hợp các mô hình thị giác chuyên biệt tự xây.
- **Active Learning**: Hệ thống tự chọn ra những mẫu dữ liệu “thông tin nhất” (nơi mô hình còn yếu hoặc dữ liệu đa dạng) để con người review, thay vì phải gán nhãn toàn bộ hàng triệu ảnh mỗi mùa.
- **Huấn luyện trên Amazon SageMaker AI**: Dùng Distributed Data Parallel (DDP) trên cluster nhiều GPU, tách hẳn tài nguyên huấn luyện khỏi tài nguyên gán nhãn, giúp tăng thông lượng và giảm thời gian chờ.

![Aigen modernized architecture](/images/3-BlogsPosted/3.2-Blog2/fig2.png)
*Hình 2: Kiến trúc hiện đại hóa của Aigen*

## 3. Kết quả kinh doanh đạt được
- **Tiết kiệm chi phí**: Giảm chi phí gán nhãn từ khoảng 2.00 USD xuống còn 0.089 USD mỗi ảnh, tương đương giảm 22.5 lần.
- **Tăng tốc pipeline gán nhãn**: Thời gian gán nhãn trung bình giảm từ 14 phút 57 giây (thủ công) xuống chỉ 41 giây với SageMaker batch inference, rút thời gian đưa mô hình cây trồng mới ra thị trường từ hàng tháng xuống hàng tuần.
- **Tăng tốc thử nghiệm**: Năng lực thử nghiệm tăng từ 5 lần/tuần (on-premise) lên hàng trăm lần/tuần với SageMaker AI, tương đương tăng thông lượng 20 lần.
- **Thúc đẩy đổi mới**: GPU mạnh trên SageMaker AI cho phép huấn luyện và fine-tune các mô hình Vision Transformer tiên tiến mà hạ tầng tại chỗ trước đây không thể đáp ứng.

## 4. Bài học và khả năng mở rộng
- **Không cần tự quản lý hạ tầng GPU**: SageMaker AI loại bỏ nhu cầu Aigen tự xây và duy trì hạ tầng GPU auto-scaling, giúp tập trung nguồn lực vào phát triển mô hình.
- **Streamline toàn bộ ML lifecycle**: Từ chuẩn bị dữ liệu đến deploy mô hình, tận dụng được cả tính năng có sẵn và quy trình tùy chỉnh như pre-labeling.
- **Fine-tune liên tục linh hoạt**: Hạ tầng managed cho phép fine-tune hàng ngày khi cây trồng phát triển theo mùa, hoặc khi mở rộng sang khách hàng/cánh đồng mới với điều kiện đất, ánh sáng, giống cây khác nhau.

## 5. Hạn chế và lưu ý triển khai
- **Vẫn cần con người trong vòng lặp**: Mô hình tạo pre-label tự động, nhưng vẫn cần annotator review và sửa lỗi (human-in-the-loop), chưa hoàn toàn tự động 100%.
- **Phụ thuộc vào chất lượng ensemble foundation model**: Hiệu quả gán nhãn tự động phụ thuộc vào việc các foundation model (SAM2, Grounding DINO...) có phù hợp với domain nông nghiệp cụ thể hay không.
- **Quản lý nhiều mô hình theo mùa vụ**: Mỗi loại cây trồng (cà chua, bông, củ cải đường, đậu nành...) cần một student model riêng, nên việc quản lý vòng đời nhiều mô hình chuyên biệt đòi hỏi quy trình MLOps chặt chẽ.
- **Chi phí chuyển đổi ban đầu**: Dù tiết kiệm dài hạn, việc chuyển từ on-premise sang cloud-native đòi hỏi đầu tư ban đầu để thiết kế lại toàn bộ pipeline.

## Kết luận
Việc chuyển đổi từ pipeline on-premise sang kiến trúc cloud-native trên AWS, kết hợp gán nhãn tự động bằng ensemble foundation model và huấn luyện song song trên Amazon SageMaker AI, đã giúp Aigen vượt qua các giới hạn về kết nối, chi phí và năng lực tính toán. Đây là một case study tiêu biểu cho thấy generative AI và các managed ML service có thể hiện đại hóa pipeline robotics, hướng tới nông nghiệp bền vững và hiệu quả hơn.

*Tác giả bài viết gốc: Purna Sanyal, Yuri Brigance, và Usman M. Khan (Aigen).*
*Tài liệu gốc: [How Aigen transformed agricultural robotics for sustainable farming with Amazon SageMaker AI](https://aws.amazon.com/vi/blogs/architecture/how-aigen-transformed-agricultural-robotics-for-sustainable-farming-with-amazon-sagemaker-ai/)*