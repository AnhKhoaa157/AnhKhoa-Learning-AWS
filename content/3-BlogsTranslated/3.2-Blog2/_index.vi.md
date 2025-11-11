---
title: "Blog 2"
date: "2025-11-11T03:24:36Z"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---
# Viết prompt chính xác với Stability AI Image Services trên Amazon Bedrock

Thông tin nhanh:

- Tác giả: Suleman Patel, Isha Dua, Fabio Branco, Maxfield Hulker  
- Ngày đăng: 18 Sep 2025  
- Chủ đề: Amazon Bedrock, Generative AI, Stability AI Image Services

---

## Mục lục

1. Tóm tắt nhanh
2. Tổng quan giải pháp
3. Điều kiện tiên quyết
4. Cấu trúc prompt để tối đa kiểm soát  
	4.1. Chọn định dạng prompt phù hợp  
	4.2. Xây prompt theo mô-đun (modular)  
	4.3. Dùng negative prompt để tinh lọc  
	4.4. Dùng trọng số (weighting) để nhấn/giảm yếu tố
5. Hướng dẫn phong cách (stylistic guidance)  
	5.1. Trộn nhãn phong cách (style tag layering)  
	5.2. Gọi tên phong cách tham chiếu  
	5.3. Ảnh tham chiếu (image-to-image)  
	5.4. Điều khiển ánh sáng (lighting)  
	5.5. Posing và framing  
	5.6. Ví dụ tổng hợp
6. Thực hành và khắc phục sự cố
7. Kết luận
8. Tài nguyên liên quan

---

## 1. Tóm tắt nhanh

Amazon Bedrock hiện hỗ trợ Stability AI Image Services (một tập API cho tạo/sửa ảnh dựa trên các dòng Stable Diffusion/Stable Image), cho phép kiểm soát chi tiết quá trình tạo ảnh: inpainting, style transfer, recolor, xoá phông, xoá vật thể, style guide, v.v. Bài viết tập trung vào kỹ thuật viết prompt để đạt đầu ra chính xác, nhất quán cho mục đích chuyên nghiệp (marketing, brand guideline, product shot). Trọng tâm là cách cấu trúc prompt, kỹ thuật nâng cao (negative prompt, weighting), hướng dẫn phong cách (style tags, tham chiếu nghệ sĩ, ảnh tham chiếu), và thực hành/best practices.

## 2. Tổng quan giải pháp

- Stability AI Image Services được cung cấp qua Amazon Bedrock dưới dạng API.  
- Mục tiêu: chuyển ý tưởng thị giác thành tài sản hình ảnh dùng được (production-ready) bằng prompt rõ ràng, có cấu trúc và có trọng số.  
- Mã mẫu: GitHub stabilityai-sample-notebooks (AWS Samples).

## 3. Điều kiện tiên quyết

1. Tài khoản AWS và thông tin xác thực truy cập lập trình.  
2. Gán quyền Amazon Bedrock cho IAM user/role.  
3. Yêu cầu quyền truy cập các mô hình trên Bedrock.  
4. Tham khảo “Getting started with the API” trong tài liệu Bedrock.

## 4. Cấu trúc prompt để tối đa kiểm soát

### 4.1 Chọn định dạng prompt phù hợp

- Ngôn ngữ tự nhiên: dễ đọc, linh hoạt; phù hợp khám phá/đối thoại.  
- Dạng “tag-based”: chính xác, ngắn gọn, thuận tiện kiểm soát cấu trúc.  
- Dạng lai (hybrid): kết hợp cả hai và cho phép thêm trọng số/nhấn mạnh.

Gợi ý: Stable Diffusion 3.5 phản hồi tốt với prompt ngôn ngữ tự nhiên; UI kỹ thuật thường dùng tag-based; hybrid cho cân bằng giữa rõ ràng và kiểm soát.

### 4.2 Xây prompt theo mô-đun (modular)

Chia prompt thành các khối riêng để tránh mâu thuẫn, dễ tinh chỉnh và debug:

- Prefix: định giọng điệu/ý định (ví dụ: “fashion editorial portrait of”).  
- Subject: chủ thể, đặc điểm bề mặt (màu da, tóc, chất liệu…).  
- Modifiers: trang phục/phụ kiện/chất liệu.  
- Action/Pose: tư thế, chuyển động, góc nhìn.  
- Environment: bối cảnh, ánh sáng, không khí.  
- Style: mỹ học, mood (ví dụ: chiaroscuro, abstract).  
- Camera/Lighting: tiêu cự, setup đèn, DOF.

Lưu ý: thứ tự mô-đun ảnh hưởng trọng số thị giác; đặt “style” sớm có thể làm phong cách chi phối mạnh hơn.

### 4.3 Negative prompt để tinh lọc

Nêu rõ “không muốn” xuất hiện để giảm artifact và tăng độ chuyên nghiệp (ví dụ: “no watermark, no weird hands, no cartoon filters”).  
Tham khảo thêm các token thường dùng cho “low quality/noise”, “anatomy issues”, “style clashes”, “technical errors”, “general cleanup”.

### 4.4 Trọng số (weighting) để nhấn/giảm yếu tố

Sử dụng cú pháp (<term>:<weight>) hoặc ((<term>)) để tăng/giảm mức ảnh hưởng:  
- 0.0–1.0: giảm nhấn (de-emphasize).  
- 1.1–2.0: tăng nhấn (emphasize).  
Ví dụ: (character:1.8), (background:1.1).  
Có thể dùng trong negative prompt để điều chỉnh độ tránh (ví dụ: (blurry:0.2)).

## 5. Hướng dẫn phong cách (stylistic guidance)

### 5.1 Style tag layering (trộn nhãn phong cách)

Pha trộn các nhãn thẩm mỹ quen thuộc (editorial, analog film, anime, cyberpunk, brutalist…) theo bản sắc thương hiệu. Bảng gợi ý: Retro/Y2K, Clean modern, Bold streetwear, Hyperreal surrealism…

### 5.2 Gọi tên phong cách tham chiếu

Có thể “gọi” phong cách của một nghệ sĩ/genre như một tín hiệu thẩm mỹ, kết hợp với phrasing riêng và weighting để dẫn dắt chất liệu, ánh sáng, bố cục, tông màu.

### 5.3 Dùng ảnh tham chiếu (image-to-image)

Sử dụng ảnh tham chiếu để khóa pose/màu/bố cục. Các workflow hỗ trợ: Structure, Sketch, Style. Công cụ như ControlNet, IP-Adapter, CLIPlike captioning tăng kiểm soát. Ví dụ quy trình:  
1) Chọn ảnh editorial chất lượng cao.  
2) Dùng ControlNet (depth/canny/seg) để khóa pose/biên dạng.  
3) Thêm prompt để định phong cách và chi tiết.

### 5.4 Điều khiển ánh sáng (lighting)

Ánh sáng quyết định mood, chiều sâu, ngôn ngữ nhiếp ảnh. Gợi ý: High-contrast studio, Soft editorial, Colored gel, Natural bounce… chọn theo use case (beauty, streetwear, outdoor…).

### 5.5 Posing và framing

Chèn cues về tư thế/góc máy để tránh cứng, tăng tự nhiên: “looking off camera”, “hands in motion”, “seated with body turned”, “shot from low angle”...

### 5.6 Ví dụ tổng hợp

Ghép các thành phần: mô tả chủ thể, pose, môi trường, ánh sáng, tiêu cự, mục đích chiến dịch; kèm negative prompt để làm sạch artifact.

## 6. Thực hành và khắc phục sự cố (best practices & troubleshooting)

- Ghi log prompt; thay đổi một biến mỗi lần; lưu seed/base images; so sánh dạng grid.  
- Khi “style random”: làm rõ thuật ngữ, thêm weight, bỏ xung đột.  
- Khi “méo khuôn mặt”: thêm cues chân dung/ánh sáng; điều chỉnh pose.  
- Khi “quá tối”: xác định rõ nguồn sáng/hướng sáng.  
- Khi “lặp lại pose”: thay seed/đổi góc máy/hành động.  
- Khi “thiếu thực”: bổ sung negative như “cartoon, digital texture, distorted”.

## 7. Kết luận

Kỹ thuật prompting nâng cao biến ý tưởng thành ảnh chất lượng chuyên nghiệp. Stability AI Image Services trên Amazon Bedrock cung cấp các công cụ kiểm soát chi tiết (tạo và chỉnh sửa), hữu ích cho chiến dịch marketing, đảm bảo nhận diện thương hiệu và ảnh sản phẩm. Kết hợp hiểu biết kỹ thuật với ý định sáng tạo giúp đạt độ chính xác và nhất quán cao. Tham khảo thêm models/ví dụ trong Bedrock và GitHub mẫu.

## 8. Tài nguyên liên quan

- Getting started with the API (Amazon Bedrock)  
- Stability AI’s foundation models trên Amazon SageMaker JumpStart  
- GitHub: aws-samples/stabilityai-sample-notebooks
- [Xem bài gốc tại AWS](https://aws.amazon.com/vi/blogs/machine-learning/prompting-for-precision-with-stability-ai-image-services-in-amazon-bedrock/)

---


