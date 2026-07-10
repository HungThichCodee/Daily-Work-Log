---
title: "Tối Ưu Hóa Inference Video Generative AI"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
## Tối Ưu Hóa Inference Video Generative AI: Cách Synthesia Đột Phá Hiệu Suất Trên Amazon EC2 G7e

Chào mọi người, sau khi nghiên cứu và tổng hợp tài liệu kỹ thuật từ AWS Architecture Blog về cách Synthesia tối ưu hóa quy trình AI, mình xin chia sẻ bài blog tóm tắt các điểm cốt lõi để cộng đồng cùng tham khảo.

## 1. Vai trò và bối cảnh nghiên cứu
Synthesia là một nền tảng video AI dành cho doanh nghiệp, cho phép tạo ra các avatar video tổng hợp từ hình ảnh và giọng nói của người thật mà không cần camera hay micro. Để vận hành các mô hình khuếch tán tiềm năng (latent diffusion models) phức tạp, Synthesia cần một hạ tầng phần cứng mạnh mẽ và linh hoạt. Nghiên cứu này tập trung vào việc giải quyết nút thắt cổ chai khi giải mã video (VAE Decoder), nơi việc lưu trữ dữ liệu vào ổ cứng thường làm GPU bị đình trệ, gây lãng phí tài nguyên tính toán.

## 2. Các điểm nổi bật về kỹ thuật
Điểm đột phá nhất trong nghiên cứu này là việc áp dụng kỹ thuật Asynchronous Frame Generation Pipeline (Luồng tạo khung hình bất đồng bộ) thay thế cho quy trình tuần tự truyền thống. 

Các đặc điểm kỹ thuật nổi bật bao gồm:
- **Sử dụng dòng instance Amazon EC2 G7e**: Trang bị GPU NVIDIA RTX PRO 6000 Blackwell với 96GB bộ nhớ GPU, tối ưu cho các mô hình Generative AI nặng về bộ nhớ.
- **Cơ chế luồng đôi (Dual CUDA Streams)**: Tách biệt Compute Stream (tính toán) và Copy Stream (sao chép dữ liệu) để cho phép GPU vừa xử lý khung hình mới, vừa chuyển dữ liệu khung hình cũ sang CPU đồng thời.
- **Chiến lược Double-Buffer và luồng xử lý riêng**: Sử dụng hai bộ đệm bộ nhớ và một luồng CPU Worker chuyên biệt để ghi dữ liệu vào ổ cứng, giúp luồng Python chính không bị chặn (blocking).
- **Hiệu quả vượt trội**: Tăng tỷ lệ sử dụng nhân GPU từ 82% lên 99.9%, giúp giảm độ trễ 8.2% và tăng thông lượng xử lý video.

## 3. Ứng dụng cho thực tế
Giải pháp này không chỉ là lý thuyết mà mang lại giá trị kinh tế và vận hành rất cụ thể:
- **Tiết kiệm chi phí**: Theo tính toán trên instance g7e.2xlarge, kỹ thuật này giúp tiết kiệm khoảng 896 USD cho mỗi 1,000 giờ video được giải mã.
- **Tối ưu hóa quy trình**: Áp dụng hiệu quả cho các mô hình video hiện đại như Wan 2.2 14B mà không cần thay đổi trọng số mô hình hay làm giảm chất lượng hình ảnh.
- **Xử lý video dài**: Nhờ cơ chế giải mã theo từng phân đoạn (chunk), hệ thống có thể xử lý các video có độ dài bất kỳ mà không bị giới hạn bởi dung lượng bộ nhớ GPU.

## 4. Khả năng mở rộng
Nghiên cứu khẳng định rằng kỹ thuật này có tính linh hoạt cực cao:
- **Không phụ thuộc kiến trúc**: Phương pháp này không chỉ dành riêng cho mô hình Wan mà có thể áp dụng cho bất kỳ quy trình tạo video theo phân đoạn nào có chuyển khung hình sang bộ nhớ máy chủ.
- **Không phụ thuộc phần cứng**: Mặc dù được thử nghiệm trên G7e, kỹ thuật này có thể mở rộng cho các loại GPU khác nhau.
- **Tiềm năng tương lai**: Hiệu suất có thể còn ấn tượng hơn nữa khi kết hợp với các mô hình đã được biên dịch (compiled models) để tận dụng tối đa sức mạnh phần cứng.

## 5. Hạn chế và lưu ý triển khai
- **Tiết kiệm lý thuyết**: Con số 896 USD/1.000 giờ là mức tối ưu lý tưởng, giả định hệ thống không gặp các nút thắt cổ chai nào khác ngoài khâu giải mã.
- **Độ phức tạp kỹ thuật**: Đòi hỏi cơ chế đồng bộ hóa khắt khe bằng CUDA Events để tránh lỗi dữ liệu khi nhiều luồng truy cập bộ nhớ cùng lúc.
- **Phạm vi tối ưu**: Chỉ giải quyết vấn đề ở bộ giải mã VAE và truyền dữ liệu, không làm tăng tốc giai đoạn khuếch tán (diffusion) của mô hình.
- **Bộ nhớ GPU**: Mức chiếm dụng tài nguyên vẫn tỷ lệ thuận với kích thước phân đoạn (chunk) mà bạn lựa chọn xử lý.

## 6. Hướng dẫn chi tiết (Deep Dive)

### Hiểu về nút thắt giải mã tuần tự
Các mô hình tạo video latent diffusion thực hiện quá trình khuếch tán trong không gian tiềm ẩn (latent space) của một variational auto-encoder (VAE) để giảm thiểu tài nguyên tính toán và bộ nhớ.

![Fig 1: High-level architecture of a VAE model](/images/3-BlogsPosted/3.1-Blog1/fig1.png)
*Hình 1: Kiến trúc tổng quan của mô hình VAE.*

Sau bước khuếch tán cuối cùng, video được tạo ra vẫn nằm trong không gian tiềm ẩn. Bước cuối là giải mã video này về dạng pixel để con người có thể xem được. Thông thường, video được chia thành các phân đoạn (chunk) theo thời gian, ví dụ 4 khung hình liên tiếp.

![Fig 2: Decoding one latent](/images/3-BlogsPosted/3.1-Blog1/fig2.png)
*Hình 2: Quá trình giải mã một latent thành 4 khung hình pixel và copy về CPU.*

Theo truyền thống, một tập hợp khung hình mới (Chunk N) được chuyển từ bộ nhớ GPU sang CPU một cách đồng bộ và phải được ghi xong vào ổ cứng trước khi CPU có thể khởi chạy lệnh CUDA cho Chunk N+1. Điều này gây ra tình trạng GPU bị đình trệ (stalling).

![Fig 3: Sequential Frame Generation Pipeline](/images/3-BlogsPosted/3.1-Blog1/fig3.png)
*Hình 3: Quy trình tạo khung hình tuần tự gây ra sự đình trệ cho GPU.*

### Luồng tạo khung hình bất đồng bộ (Asynchronous Pipeline)
Để tối ưu hóa và giảm thiểu GPU stalling, quy trình được sửa đổi sao cho tất cả các tác vụ trên CPU (như ghi file) chạy song song với luồng xử lý liên tục của GPU. Việc này sử dụng hai luồng CUDA: Compute Stream (tính toán) và Copy Stream (sao chép).

![Fig 4: High level diagram](/images/3-BlogsPosted/3.1-Blog1/fig4.png)
*Hình 4: Các thành phần chính trong bản cài đặt quy trình tạo khung hình bất đồng bộ.*

Để tránh các lệnh gọi làm chặn luồng trên CPU và tối đa hóa hiệu suất GPU, quy trình đưa vào 2 cơ chế:
1. **Worker CPU thread chuyên biệt**: Phụ trách đọc chunk từ RAM và ghi ra file, nhường luồng Python chính để tập trung launch kernel và lên lịch copy D2H.
2. **Double-Buffer (Bộ đệm kép)**: Sử dụng 2 buffer trên VRAM và 2 buffer trên RAM (Pinned Memory) để đảm bảo việc tính toán, copy và xử lý hoạt động đan xen an toàn trên các vùng nhớ riêng biệt.

![Fig 5: Schematic representation of Events, Streams, Buffers](/images/3-BlogsPosted/3.1-Blog1/fig5.png)
*Hình 5: Sơ đồ tương tác giữa Events, Streams, Buffers và Worker. (Events đóng vai trò đồng bộ hóa để tránh ghi đè dữ liệu).*

Nhờ quy trình này, GPU kernel utilization có thể tăng vọt từ 82% lên 99.9%, hoàn toàn loại bỏ thời gian chờ lãng phí của GPU khi xử lý giải mã video. Điều này được thể hiện rõ qua biểu đồ profiling dưới đây:

![Fig 6: Synchronous pipeline profiling](/images/3-BlogsPosted/3.1-Blog1/fig6.png)
*Hình 6: Profiling của quy trình tuần tự. GPU bị đình trệ khi chờ CPU ghi Chunk N vào ổ cứng.*

![Fig 7: Asynchronous pipeline profiling](/images/3-BlogsPosted/3.1-Blog1/fig7.png)
*Hình 7: Trong quy trình bất đồng bộ, luồng tính toán (Compute Stream) hoàn toàn không bị ngắt quãng.*

Bảng dưới đây thể hiện kết quả benchmark thời gian giải mã cho 10 lần chạy liên tiếp giữa hai quy trình:

| Metric   | Quy trình Tuần tự (time s / video) | Quy trình Bất đồng bộ (time s / video) |
| -------- | ---------------------------------- | -------------------------------------- |
| **min**  | 21.98                              | 20.16                                  |
| **mean** | 21.99                              | 20.17                                  |
| **P99**  | 22.01                              | 20.20                                  |

*Bảng 1: Kết quả benchmark cho 10 lần chạy giải mã liên tiếp của quy trình Tuần tự và Bất đồng bộ.*

## Kết luận
Việc chuyển đổi từ quy trình xử lý tuần tự sang bất đồng bộ là một bước tiến quan trọng trong việc vận hành Generative AI. Bằng cách tận dụng tối đa khả năng song song của GPU trên Amazon EC2 G7e, chúng ta không chỉ tăng tốc độ sản xuất video mà còn tối ưu hóa chi phí vận hành một cách đáng kể. Đây là bài học quý giá cho bất kỳ ai đang xây dựng các ứng dụng AI quy mô lớn.

Hy vọng những tóm tắt này giúp các bạn có cái nhìn nhanh và sâu sắc về cách các "ông lớn" như Synthesia đang tối ưu hóa hạ tầng của họ!

*Tác giả bài viết gốc: Moises Hernandez, Hanno Bever, Emanuele Levi, và Pierre Lienhart.*
*Tài liệu gốc: [How Synthesia optimizes Generative AI video inference on Amazon EC2 G7e instances](https://aws.amazon.com/vi/blogs/architecture/how-synthesia-optimizes-generative-ai-video-inference-on-amazon-ec2-g7e-instances/)*