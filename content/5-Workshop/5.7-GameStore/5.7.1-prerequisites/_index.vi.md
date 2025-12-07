---
title: "Yêu cầu & Thiết lập"
date: "2025-12-06T00:00:00Z"
weight: 1
chapter: false
pre: " <b> 5.7.1. </b> "
---

#### Yêu cầu trước khi bắt đầu

Trước khi bắt đầu workshop này, đảm bảo bạn có:

**Yêu cầu Tài khoản AWS:**
- Tài khoản AWS đang hoạt động với quyền administrator
- Quen thuộc với AWS Console
- Hiểu các khái niệm cloud cơ bản

**Công cụ Phát triển:**
- **Java Development Kit (JDK) 17+**
  ```bash
  java -version
  # Phải hiển thị: java version "17" trở lên
  ```

- **Node.js 18+ và npm**
  ```bash
  node --version  # v18.0.0 trở lên
  npm --version   # v9.0.0 trở lên
  ```

- **Maven 3.8+**
  ```bash
  mvn --version
  ```

- **Git**
  ```bash
  git --version
  ```

- **VS Code hoặc IntelliJ IDEA** (khuyến nghị)

**AWS CLI (Tùy chọn nhưng khuyến nghị):**
```bash
# Cài đặt AWS CLI
pip install awscli --upgrade --user

# Cấu hình AWS CLI
aws configure
# AWS Access Key ID: YOUR_ACCESS_KEY
# AWS Secret Access Key: YOUR_SECRET_KEY
# Default region: ap-southeast-1
# Default output format: json
```

#### Quyền IAM

Tạo IAM user với các quyền sau:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "rds:*",
                "s3:*",
                "ses:*",
                "ec2:*",
                "iam:PassRole",
                "cloudformation:*",
                "cloudwatch:*",
                "logs:*"
            ],
            "Resource": "*"
        }
    ]
}
```

**Các bước tạo IAM user:**

1. Vào **IAM Console** → **Users** → **Add users**
2. User name: `game-store-workshop`
3. Access type: ✅ **Programmatic access** + ✅ **AWS Management Console access**
4. Permissions: **Attach existing policies directly**
   - `AmazonRDSFullAccess`
   - `AmazonS3FullAccess`
   - `AmazonSESFullAccess`
   - `AmazonEC2FullAccess`
5. **Download credentials CSV** - Lưu trữ an toàn!

#### Ước tính Chi phí

**Chi phí Tài nguyên AWS (khu vực ap-southeast-1):**

| Dịch vụ | Cấu hình | Chi phí Ước tính |
|---------|---------|------------------|
| RDS MySQL | db.t3.micro, 20GB | ~$15/tháng |
| S3 Storage | 5GB lưu trữ + requests | ~$0.50/tháng |
| EC2 Instance | t2.micro (Free Tier) | Miễn phí/$8/tháng |
| SES | 1000 emails | Miễn phí (trong giới hạn) |
| Data Transfer | 10GB outbound | ~$1/tháng |
| **Tổng** | | **~$16-25/tháng** |

{{% notice tip %}}
**Tiết kiệm Chi phí:**
- Sử dụng tài nguyên đủ điều kiện **Free Tier** (RDS db.t3.micro trong 12 tháng)
- Dừng EC2 instances khi không sử dụng
- Xóa tài nguyên sau khi hoàn thành workshop
- Sử dụng **RDS Single-AZ** cho development (không phải production)
{{% /notice %}}

#### Clone Repository

```bash
# Clone repository game store
git clone https://github.com/AnhKhoaa157/Workshop-AWS.git
cd Workshop-AWS

# Kiểm tra cấu trúc
ls -la
# Bạn sẽ thấy:
# - Back-End/    (ứng dụng Spring Boot)
# - Front-End/   (ứng dụng React)
# - README.md
```

#### Xác minh Cài đặt

Chạy các lệnh sau để xác minh thiết lập của bạn:

```bash
# Kiểm tra Java
java -version

# Kiểm tra Maven
mvn --version

# Kiểm tra Node.js
node --version

# Kiểm tra npm
npm --version

# Kiểm tra Git
git --version

# Kiểm tra AWS CLI (tùy chọn)
aws --version
```

#### Chọn AWS Region

**Region Khuyến nghị:** `ap-southeast-1` (Singapore)

**Lý do:**
- ✅ Độ trễ thấp cho người dùng Đông Nam Á
- ✅ Đầy đủ dịch vụ (RDS, S3, SES, EC2)
- ✅ Giá cả cạnh tranh
- ✅ Tài liệu và hỗ trợ tốt

**Để đặt region:**
```bash
# Qua AWS CLI
export AWS_DEFAULT_REGION=ap-southeast-1

# Hoặc trong AWS Console
# Góc trên bên phải → Chọn "Asia Pacific (Singapore) ap-southeast-1"
```

#### Tổng quan Cấu trúc Dự án

```
Workshop-AWS/
├── Back-End/                    # Spring Boot Backend
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/
│   │   │   │   └── com/se182393/baidautien/
│   │   │   │       ├── controller/      # REST Controllers
│   │   │   │       ├── service/         # Business Logic
│   │   │   │       ├── repository/      # Data Access
│   │   │   │       ├── entity/          # JPA Entities
│   │   │   │       ├── dto/             # Data Transfer Objects
│   │   │   │       ├── configuration/   # Spring Config
│   │   │   │       └── enums/           # Enums (Role, etc)
│   │   │   └── resources/
│   │   │       ├── application.yaml     # Cấu hình Local
│   │   │       └── application-ec2.yaml # Cấu hình Production
│   │   └── test/                        # Unit tests
│   ├── pom.xml                          # Maven dependencies
│   └── README.md
│
├── Front-End/                   # React Frontend
│   ├── src/
│   │   ├── api/                 # API client
│   │   ├── components/          # React components
│   │   ├── pages/               # Page components
│   │   ├── context/             # React Context
│   │   ├── utils/               # Utility functions
│   │   └── styles/              # CSS files
│   ├── package.json
│   ├── vite.config.ts
│   └── README.md
│
└── README.md                    # Tài liệu dự án
```

#### Tổng quan Kiến trúc

```
┌─────────────┐
│  Người dùng │
└──────┬──────┘
       │
       ├──────────────────┐
       │                  │
       ▼                  ▼
┌─────────────┐    ┌─────────────┐
│   React     │    │  Spring     │
│   Frontend  │◄───┤  Boot API   │
└─────────────┘    └──────┬──────┘
                          │
                ┌─────────┼─────────┐
                │         │         │
                ▼         ▼         ▼
         ┌──────────┐ ┌─────┐ ┌─────┐
         │ RDS MySQL│ │ S3  │ │ SES │
         └──────────┘ └─────┘ └─────┘
```

**Các thành phần:**
1. **Frontend (React):** Giao diện người dùng hosted trên S3/EC2
2. **Backend (Spring Boot):** REST API cho business logic
3. **Database (RDS):** MySQL cho lưu trữ dữ liệu
4. **Storage (S3):** Hình ảnh, avatars, game files
5. **Email (SES):** Emails giao dịch (đơn hàng, xác minh)

#### Thiết lập Môi trường

Tạo environment files cho cấu hình:

**File `.env` Backend:**
```bash
# Điều hướng đến thư mục Back-End
cd Back-End

# Tạo file .env
cat > .env << 'EOF'
# Database
RDS_ENDPOINT=your-rds-endpoint.rds.amazonaws.com
RDS_PORT=3306
RDS_DATABASE=identity_service
RDS_USERNAME=admin
RDS_PASSWORD=your-password

# AWS S3
AWS_REGION=ap-southeast-1
AWS_ACCESS_KEY=YOUR_ACCESS_KEY
AWS_SECRET_KEY=YOUR_SECRET_KEY
AWS_S3_BUCKET_NAME=game-store-images-2025

# AWS SES
AWS_SES_FROM_EMAIL=your-verified-email@example.com

# JWT
JWT_SIGNER_KEY=your-secret-key-min-32-chars

# Email
MAIL_USERNAME=your-email@gmail.com
MAIL_PASSWORD=your-app-password

# MoMo Payment
MOMO_PARTNER_CODE=MOMOLRJZ20181206
MOMO_ACCESS_KEY=mTCKt9W3eU1m39TW
MOMO_SECRET_KEY=SetA5RDnLHvt51AULf51DyauxUo3kDU6
EOF
```

**File `.env` Frontend:**
```bash
# Điều hướng đến thư mục Front-End
cd Front-End

# Tạo file .env
cat > .env << 'EOF'
VITE_API_URL=http://localhost:8080/identity
VITE_GOOGLE_CLIENT_ID=your-google-oauth-client-id
EOF
```

{{% notice warning %}}
**Bảo mật:** Không bao giờ commit file `.env` vào Git! Thêm chúng vào `.gitignore`.
{{% /notice %}}

#### Sẵn sàng Bắt đầu!

Bây giờ bạn đã có:
- ✅ Thiết lập tài khoản AWS
- ✅ Cài đặt công cụ phát triển
- ✅ Clone repository
- ✅ Cấu hình môi trường

Bạn đã sẵn sàng để tiến hành phần tiếp theo: **Thiết lập Database (RDS)** →

{{% notice info %}}
**Thời gian Ước tính:** 30-45 phút để hoàn thành tất cả yêu cầu tiên quyết
{{% /notice %}}
