---
title: "Blog 2 - ALB cho SAP workloads trên AWS"
date: 2025-01-07
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Nâng cao Trải nghiệm Người dùng và Bảo mật của Application Load Balancer cho SAP workloads trên AWS

**Tác giả**: Ferry Mulyadi, Mohit Biyani, và Krishnakumar Ramadoss  
**Ngày đăng**: 01/07/2025  
**Danh mục**: Elastic Load Balancing, How-To, SAP on AWS, Technical How-to, Thought Leadership  
**Nguồn**: AWS for SAP Blog

---

## Tổng quan

Bài viết trình bày cách **nâng cao trải nghiệm người dùng và bảo mật** khi sử dụng Application Load Balancer (ALB) cho các khối lượng công việc SAP trên AWS. Nhiều khách hàng chạy SAP trên AWS và xây dựng các phần mở rộng ứng dụng cloud-native để duy trì lõi ERP sạch và tăng tốc đổi mới.

### Trọng tâm Chính

Một điểm trọng tâm là **principal propagation**, tức việc mang danh tính đã xác thực từ ALB đến hệ thống SAP mà không cần xác thực lại, cho phép trải nghiệm **Single Sign-On (SSO)**.

Bài viết giải thích cách **mTLS (Mutual Transport Layer Security)** và chứng chỉ X.509 hỗ trợ principal propagation, bao gồm hai chế độ mTLS trên ALB:

- **Passthrough mode**: chuyển toàn bộ chuỗi chứng chỉ đến backend
- **Verify mode**: ALB xác thực client trước, giảm tải cho backend

**Khuyến nghị**: Sử dụng **mTLS verify mode** cho SAP trên AWS vì nó đảm bảo bảo mật và hiệu quả xử lý.

---

## Principal Propagation là gì?

**Principal propagation** cho phép người dùng đăng nhập một lần để truy cập nhiều hệ thống một cách an toàn, loại bỏ nhu cầu đăng nhập nhiều lần.

### Cách hoạt động

Khi một người dùng được xác thực (ví dụ thông qua chứng chỉ client tại ALB), danh tính đó được nhận diện và tin tưởng một cách nhất quán bởi các hệ thống downstream như SAP. Điều này:

- Tránh các lời nhắc đăng nhập dư thừa
- Duy trì bối cảnh người dùng an toàn trên toàn cảnh quan
- Cho phép **Single Sign-On (SSO)**
- Đảm bảo các hành động được thực hiện dưới danh tính người dùng, không phải người dùng hệ thống
- Cải thiện bảo mật bằng cách dựa vào xác thực dựa trên token

---

## Lợi ích Chính

### 1. Trải nghiệm Người dùng Tốt hơn với SSO
Người dùng chỉ cần xác thực một lần để truy cập nhiều hệ thống và ứng dụng, loại bỏ sự bực bội của các lời nhắc đăng nhập lặp đi lặp lại.

### 2. Tăng cường Bảo mật
Loại bỏ nhu cầu lưu trữ nhiều thông tin đăng nhập và giảm các điểm tiếp xúc xác thực, tăng cường tư thế bảo mật.

### 3. Tuân thủ và Quản trị
Duy trì tuân thủ tốt hơn với các yêu cầu quy định thông qua việc theo dõi hoạt động người dùng toàn diện và trách nhiệm giải trình.

### 4. Hiệu quả Quản trị
Các nhóm IT có thể quản lý quyền truy cập người dùng, quyền hạn và thông tin đăng nhập từ một điểm kiểm soát duy nhất.

### 5. Tích hợp Hệ thống
Principal propagation phục vụ như một cây cầu giữa các hệ thống và nền tảng khác nhau, duy trì bối cảnh người dùng nhất quán.

### 6. Giảm Chi phí
Giảm các ticket help desk, triển khai bảo mật đơn giản hóa, và sử dụng tài nguyên hiệu quả hơn.

---

## mTLS Authentication hỗ trợ Principal Propagation

**Mutual Transport Layer Security (mTLS)** thiết lập một kết nối mã hóa an toàn, hai chiều giữa client và server. Không giống như TLS tiêu chuẩn, mTLS yêu cầu cả hai bên đều trình bày chứng chỉ số.

### Quá trình xác thực mTLS:

1. Client yêu cầu kết nối đến server
2. Server trình bày chứng chỉ của nó
3. Client xác minh chứng chỉ của server
4. Client trình bày chứng chỉ của nó để server xác minh
5. Kết nối an toàn được thiết lập

---

## mTLS với Application Load Balancer

ALB hỗ trợ xác thực mTLS với hai chế độ: **passthrough** và **verify mode**.

### mTLS Passthrough Mode

Trong chế độ này, ALB chuyển tiếp toàn bộ chuỗi chứng chỉ của client đến các target backend thông qua HTTP header `X-Amzn-Mtls-Clientcert`.

**Cân nhắc:**
- ALB không thêm header nếu chứng chỉ client không có
- Các target backend chịu trách nhiệm về xác thực client
- ALB kết thúc TLS client-ALB và khởi tạo TLS ALB-backend mới
- Cho phép sử dụng bất kỳ thuật toán routing ALB nào

### mTLS Verify Mode

Để kích hoạt chế độ verify, tạo một **trust store** chứa bundle chứng chỉ CA sử dụng:
- AWS Certificate Manager (ACM)
- AWS Private CA
- Import chứng chỉ của riêng bạn

**Lợi ích:**
- ALB xử lý việc xác minh chứng chỉ client
- Hiệu quả chặn các yêu cầu không được ủy quyền
- Giảm tải xử lý mTLS từ backend
- Cải thiện hiệu quả tổng thể hệ thống
- ALB truyền metadata chứng chỉ client thông qua HTTP Headers

---

## Khuyến nghị

 **Khuyến nghị triển khai chế độ mTLS verify** cho SAP trên AWS vì:
- Giảm tải việc xác minh và xác thực càng sớm càng tốt (tại lớp ALB)
- Được hỗ trợ cho RISE with SAP on AWS
- Cải thiện bảo mật và hiệu suất tổng thể

---

## Kiến trúc Tổng thể

Kiến trúc liên quan đến:
- **ALB**: Xác thực mTLS
- **SAP Web Dispatcher**: Chuyển tiếp request
- **SAP Systems** (S/4HANA, ABAP Stack): Xử lý nghiệp vụ
- **AWS Direct Connect** hoặc Internet: Kết nối
- **Amazon S3**: Lưu trữ CRLs (Certificate Revocation Lists)

Kiến trúc này đảm bảo xác thực đầu cuối giữa client và server một cách an toàn, với principal propagation xuyên suốt.