---
title: "Xây dựng Cửa hàng Game Full-Stack"
date: "2025-12-06T00:00:00Z"
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

#### Tổng quan

Trong workshop này, bạn sẽ xây dựng một **nền tảng e-commerce full-stack** hoàn chỉnh cho phân phối game kỹ thuật số sử dụng các dịch vụ AWS cloud. Đây là một ứng dụng thực tế sẵn sàng cho production, thể hiện việc tích hợp nhiều dịch vụ AWS với Spring Boot backend và React frontend.

**Những gì bạn sẽ xây dựng:**
- Spring Boot REST API backend với JWT authentication
- React SPA frontend với UI/UX hiện đại
- AWS RDS MySQL database cho lưu trữ dữ liệu
- AWS S3 cho hình ảnh game và avatar người dùng
- AWS SES cho thông báo email
- Tích hợp thanh toán (MoMo, VNPay)
- Xác thực OAuth2 Google
- Phân quyền đa vai trò (ADMIN, USER, MOD)

#### Kiến trúc

![Game Store Architecture](/images/5-Workshop/5.7-GameStore/architecture.png)

**Technology Stack:**
```
Frontend: React 18 + TypeScript + Vite + Axios
Backend: Spring Boot 3.4 + Spring Security + JWT
Database: AWS RDS MySQL 8.0
Storage: AWS S3 (ap-southeast-1)
Email: AWS SES
Hosting: AWS EC2 (Ubuntu)
Payment: MoMo & VNPay Gateway
```

**Luồng Request:**
```
Người dùng → React App (CloudFront/S3)
       ↓
Đăng nhập → Spring Security → JWT Token
       ↓
Duyệt Game → REST API → RDS (Danh mục Games)
       ↓
Mua hàng → Cổng Thanh toán (MoMo/VNPay)
       ↓
Xác nhận → Lambda (Tạo Key) → SES (Email)
       ↓
Tải xuống → S3 (CloudFront Distribution)
```

#### Tính năng

**Tính năng Người dùng:**
- Duyệt danh mục game với bộ lọc và tìm kiếm
- Xem thông tin chi tiết game với screenshots
- Thêm game vào giỏ hàng và danh sách yêu thích
- Thanh toán an toàn với nhiều phương thức
- Quản lý hồ sơ cá nhân với upload avatar
- Nạp tiền ví qua MoMo
- Lịch sử đơn hàng và theo dõi giao dịch
- Thông báo email cho đơn hàng

**Tính năng Admin:**
- Quản lý game (CRUD operations)
- Quản lý người dùng với phân quyền
- Theo dõi đơn hàng và phân tích
- Quản lý danh mục
- Dashboard với thống kê
- Thao tác hàng loạt

**Tính năng Bảo mật:**
- Xác thực dựa trên JWT
- Kiểm soát truy cập dựa trên vai trò (RBAC)
- Mã hóa mật khẩu (BCrypt)
- Cấu hình CORS
- Ngăn chặn SQL injection
- Bảo vệ XSS
- Bảo vệ CSRF

#### Nội dung Workshop

Workshop này được chia thành các phần sau:

1. [**Yêu cầu & Thiết lập**](5.7.1-prerequisites/) - Tài khoản AWS, cài đặt công cụ
2. [**Thiết lập Database (RDS)**](5.7.2-database/) - Tạo RDS MySQL instance
3. [**Thiết lập Storage (S3)**](5.7.3-storage/) - Cấu hình S3 bucket cho hình ảnh
4. [**Dịch vụ Email (SES)**](5.7.4-email/) - Thiết lập AWS SES cho thông báo
5. [**Phát triển Backend**](5.7.5-backend/) - Deploy Spring Boot API
6. [**Phát triển Frontend**](5.7.6-frontend/) - Deploy ứng dụng React
7. [**Tích hợp Thanh toán**](5.7.7-payment/) - Thiết lập MoMo & VNPay
8. [**Cấu hình Bảo mật**](5.7.8-security/) - Triển khai authentication
9. [**Testing & Validation**](5.7.9-testing/) - Kiểm thử end-to-end
10. [**Dọn dẹp**](5.7.10-cleanup/) - Xóa tài nguyên AWS

#### Thời gian Ước tính
- **Tổng thời gian:** 4-6 giờ
- **Chi phí:** $5-10 cho workshop (giả sử bạn dọn dẹp tài nguyên sau đó)

#### Mục tiêu Học tập

Kết thúc workshop này, bạn sẽ:
- ✅ Hiểu AWS RDS cho cơ sở dữ liệu quan hệ
- ✅ Học S3 cho object storage và CloudFront cho CDN
- ✅ Triển khai AWS SES cho transactional emails
- ✅ Deploy ứng dụng Spring Boot trên EC2
- ✅ Cấu hình security với Spring Security và JWT
- ✅ Tích hợp cổng thanh toán (MoMo, VNPay)
- ✅ Xây dựng REST APIs sẵn sàng cho production
- ✅ Tạo React frontends responsive
- ✅ Triển khai role-based access control
- ✅ Xử lý file uploads lên S3
- ✅ Quản lý environment variables và secrets

{{% notice info %}}
Workshop này sử dụng tài nguyên AWS thực tế sẽ phát sinh chi phí. Đảm bảo làm theo hướng dẫn dọn dẹp ở cuối để tránh chi phí không mong muốn.
{{% /notice %}}

{{% notice warning %}}
**Cân nhắc Production:** Workshop này tập trung vào chức năng cốt lõi. Để triển khai production, cân nhắc thêm:
- Amazon CloudWatch cho giám sát
- AWS Secrets Manager cho credentials
- Amazon CloudFront cho global CDN
- AWS Lambda cho serverless functions
- Amazon SQS cho message queuing
- AWS WAF cho web application firewall
- Auto Scaling cho high availability
{{% /notice %}}

#### Bắt đầu thôi! →
