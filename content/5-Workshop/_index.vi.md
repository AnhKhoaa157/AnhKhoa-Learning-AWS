---
title: "Workshop"
date: "2025-12-04T07:05:17Z"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Xây dựng Nền tảng Devteria Game Store trên AWS

#### Tổng quan

Workshop này hướng dẫn bạn xây dựng một **nền tảng e-commerce serverless** hoàn chỉnh cho phân phối game kỹ thuật số sử dụng các dịch vụ AWS cloud. Bạn sẽ học cách tạo một cửa hàng game có khả năng mở rộng, bảo mật, và tiết kiệm chi phí với quản lý kho hàng thời gian thực, xử lý đơn hàng tự động, và phân phối nội dung toàn cầu.

**Những gì bạn sẽ xây dựng:**
- Backend API serverless với Lambda + API Gateway
- Xác thực bảo mật với AWS Cognito (bật MFA)
- Cơ sở dữ liệu RDS PostgreSQL cho danh mục game và đơn hàng
- S3 + CloudFront cho phân phối nội dung toàn cầu nhanh chóng
- SQS/SNS cho xử lý đơn hàng bất đồng bộ
- Giám sát và cảnh báo CloudWatch
- CI/CD pipeline với CodePipeline

#### Kiến trúc

![Devteria Architecture](/images/5-Workshop/devteria-architecture.png)

**Luồng Request:**
```
Người dùng → CloudFront → S3 (React Frontend)
       ↓
Đăng nhập → Cognito (MFA) → JWT Token
       ↓
Duyệt Game → API Gateway → Lambda → RDS (Catalog)
       ↓
Mua hàng → Lambda → RDS (Order) → SQS (License Queue)
       ↓
SQS Trigger → Lambda (Tạo License) → SNS (Email)
       ↓
Tải xuống → CloudFront → S3 (Game Files)
```

#### Cấu trúc Dự án

```
devteria-gamestore/
├── frontend/                # React App
│   ├── src/
│   │   ├── components/      # UI components
│   │   ├── pages/           # Pages (Home, Catalog, Cart)
│   │   ├── services/        # API calls
│   │   └── App.js
│   └── package.json
├── backend/                 # Lambda Functions
│   ├── auth/                # Tích hợp Cognito
│   ├── catalog/             # Game CRUD
│   ├── orders/              # Xử lý đơn hàng
│   ├── licenses/            # Tạo license
│   └── shared/              # Utils chung
├── infrastructure/          # CloudFormation/Terraform
│   ├── network.yml          # VPC, Subnets
│   ├── database.yml         # RDS PostgreSQL
│   ├── storage.yml          # S3, CloudFront
│   ├── compute.yml          # Lambda, API Gateway
│   └── cicd.yml             # CodePipeline
└── buildspec.yml            # Cấu hình CodeBuild
```

#### Nội dung

1. [Tổng quan Workshop](5.1-Workshop-overview/)
2. [Chuẩn bị](5.2-Prerequiste/)
3. [Thiết lập Backend Infrastructure](5.3-S3-vpc/)
4. [Xây dựng Frontend & Deploy](5.4-S3-onprem/)
5. [Cấu hình CI/CD Pipeline](5.5-Policy/)
6. [Dọn dẹp Tài nguyên](5.6-Cleanup/)

#### Các Dịch vụ AWS Sử dụng

| Danh mục | Dịch vụ | Mục đích |
|----------|---------|----------|
| **Compute** | AWS Lambda | Hàm backend serverless |
| **API** | API Gateway | Quản lý RESTful API |
| **Xác thực** | Amazon Cognito | Xác thực người dùng & MFA |
| **Database** | Amazon RDS PostgreSQL | Dữ liệu quan hệ (games, orders, users) |
| **Storage** | Amazon S3 | File game & tài sản frontend |
| **CDN** | Amazon CloudFront | Phân phối nội dung toàn cầu |
| **Queue** | Amazon SQS | Xử lý đơn hàng bất đồng bộ |
| **Thông báo** | Amazon SNS | Cảnh báo email |
| **Giám sát** | Amazon CloudWatch | Logs, metrics, alarms |
| **CI/CD** | CodePipeline, CodeBuild | Triển khai tự động |
| **Bảo mật** | IAM, Secrets Manager | Kiểm soát truy cập & bí mật |

#### Thời gian & Chi phí Ước tính

| Chỉ số | Giá trị |
|--------|---------|
| **Thời lượng** | 4-5 giờ |
| **Cấp độ** | Trung cấp |
| **Chi phí** | ~$10-15 (trong workshop) |
| **Hàng tháng** | ~$50-100 (nếu giữ chạy) |

{{% notice tip %}}
Sử dụng **AWS Free Tier** cho Lambda (1M requests), API Gateway (1M calls), S3 (5GB), CloudFront (1TB), và Cognito (50K users) để giảm thiểu chi phí!
{{% /notice %}}

#### Kết quả Học tập

Sau khi hoàn thành workshop này, bạn sẽ:

✅ Xây dựng nền tảng e-commerce serverless production-ready  
✅ Triển khai xác thực JWT bảo mật với Cognito + MFA  
✅ Thiết kế kiến trúc API có khả năng mở rộng với Lambda + API Gateway  
✅ Thiết lập CDN toàn cầu với CloudFront cho phân phối nhanh  
✅ Tạo workflows bất đồng bộ với SQS/SNS  
✅ Deploy infrastructure as code với CloudFormation  
✅ Cấu hình CI/CD pipelines cho triển khai tự động  
✅ Giám sát ứng dụng với CloudWatch  
✅ Tối ưu chi phí với kiến trúc serverless  

#### Kiểm tra Điều kiện Tiên quyết

Trước khi bắt đầu, đảm bảo bạn có:
- ✅ Tài khoản AWS với quyền admin
- ✅ AWS CLI đã cài đặt & cấu hình
- ✅ Node.js 18+ (cho React frontend)
- ✅ Git đã cài đặt
- ✅ Kiến thức cơ bản về JavaScript/React
- ✅ Hiểu biết về REST APIs
- ✅ Quen thuộc với SQL (PostgreSQL)

Sẵn sàng? Cùng xây dựng! 🚀












