---
title: "Thiết lập Cơ sở Dữ liệu (RDS MySQL)"
date: "2025-12-06T00:00:00Z"
weight: 2
chapter: false
pre: " <b> 5.7.2. </b> "
---

#### Tổng quan

Trong phần này, chúng ta sẽ thiết lập cơ sở dữ liệu Amazon RDS MySQL cho ứng dụng Game Store. RDS cung cấp dịch vụ cơ sở dữ liệu quan hệ được quản lý với sao lưu tự động, vá lỗi và tính khả dụng cao.

**Những gì chúng ta sẽ cấu hình:**
- Instance RDS MySQL 8.0 (db.t3.micro)
- Security groups cho truy cập cơ sở dữ liệu
- Khởi tạo cơ sở dữ liệu
- Kiểm tra kết nối từ môi trường phát triển local

#### Kiến trúc

```
┌──────────────┐         ┌──────────────┐
│  Spring Boot │────────►│  RDS MySQL   │
│   Backend    │  3306   │  db.t3.micro │
└──────────────┘         └──────────────┘
                                │
                         ┌──────┴───────┐
                         │ Security     │
                         │ Group        │
                         │ (Port 3306)  │
                         └──────────────┘
```

#### Bước 1: Tạo RDS Subnet Group

Trước khi tạo RDS instance, chúng ta cần subnet group:

1. **Điều hướng đến RDS Console**
   - Truy cập: https://console.aws.amazon.com/rds/
   - Region: **ap-southeast-1** (Singapore)

2. **Tạo Subnet Group**
   - Menu bên trái → **Subnet groups** → **Create DB subnet group**
   - Name: `game-store-db-subnet`
   - Description: `Subnet group for Game Store database`
   - VPC: Chọn **default VPC** của bạn
   - Availability Zones: Chọn **ap-southeast-1a** và **ap-southeast-1b**
   - Subnets: Chọn tất cả subnets có sẵn
   - Click **Create**

#### Bước 2: Tạo Security Group

1. **Điều hướng đến EC2 Console**
   - Truy cập: https://console.aws.amazon.com/ec2/
   - Menu bên trái → **Security Groups** → **Create security group**

2. **Cấu hình Security Group**
   - Name: `game-store-rds-sg`
   - Description: `Security group for Game Store RDS MySQL`
   - VPC: Chọn **default VPC** của bạn

3. **Thêm Inbound Rule**
   - Click **Add rule**
   - Type: **MySQL/Aurora**
   - Protocol: **TCP**
   - Port range: **3306**
   - Source: **0.0.0.0/0** (cho development - hạn chế trong production!)
   - Description: `MySQL access from anywhere`

4. **Outbound Rules**
   - Giữ mặc định (All traffic)

5. Click **Create security group**

{{% notice warning %}}
**Bảo mật:** Cho production, hạn chế nguồn chỉ cho các địa chỉ IP cụ thể hoặc VPC CIDR của bạn. Không bao giờ expose database công khai trong production!
{{% /notice %}}

#### Bước 3: Tạo RDS MySQL Instance

1. **Điều hướng đến RDS Dashboard**
   - Click **Create database**

2. **Chọn Database Creation Method**
   - Chọn: ⚪ **Standard create**

3. **Engine Options**
   - Engine type: **MySQL**
   - Version: **MySQL 8.0.39** (hoặc phiên bản 8.0.x mới nhất)

4. **Templates**
   - Chọn: ⚪ **Free tier** (nếu đủ điều kiện)
   - Hoặc: ⚪ **Dev/Test** (cho hiệu suất tốt hơn)

5. **Settings**
   - DB instance identifier: `game-store-db`
   - Master username: `admin`
   - Master password: `GameStore2025!` (hoặc mật khẩu bảo mật của bạn)
   - Confirm password: `GameStore2025!`

{{% notice tip %}}
**Yêu cầu Mật khẩu:** Tối thiểu 8 ký tự, ít nhất một chữ hoa, một chữ thường, một số và một ký tự đặc biệt.
{{% /notice %}}

6. **Instance Configuration**
   - DB instance class: **db.t3.micro** (Đủ điều kiện Free Tier)
   - Storage type: **General Purpose SSD (gp3)**
   - Allocated storage: **20 GB**
   - ☑️ **Disable** storage autoscaling (để kiểm soát chi phí)

7. **Connectivity**
   - Compute resource: **Don't connect to an EC2 compute resource**
   - VPC: **Default VPC**
   - DB subnet group: **game-store-db-subnet** (đã tạo trước đó)
   - Public access: **Yes** (cho development)
   - VPC security group: **Choose existing**
   - Chọn: **game-store-rds-sg**
   - Availability Zone: **ap-southeast-1a**

8. **Database Authentication**
   - Authentication: **Password authentication**

9. **Additional Configuration**
   - Initial database name: `identity_service`
   - DB parameter group: **default.mysql8.0**
   - Backup retention: **7 days** (điều chỉnh theo nhu cầu)
   - ☑️ **Enable automated backups**
   - Backup window: **Preferred time** (ví dụ: 03:00-04:00 UTC)
   - ☑️ **Enable encryption** (khuyến nghị)
   - Monitoring: ☑️ **Enable Enhanced Monitoring** (tùy chọn)
   - Log exports: ☑️ **Error log**, ☑️ **Slow query log**

10. **Maintenance**
    - ☑️ **Enable auto minor version upgrade**
    - Maintenance window: **Preferred time** (ví dụ: Sun 04:00-05:00 UTC)

11. **Ước tính Chi phí Hàng tháng**
    - Xem xét ước tính chi phí
    - Click **Create database**

{{% notice info %}}
**Thời gian Tạo:** Việc tạo RDS instance mất khoảng 5-10 phút. Trạng thái sẽ thay đổi từ "Creating" → "Available".
{{% /notice %}}

#### Bước 4: Lấy RDS Endpoint

1. **Đợi DB Instance Available**
   - RDS Dashboard → **Databases**
   - Đợi cho đến khi trạng thái hiển thị **Available**

2. **Copy Endpoint**
   - Click vào **game-store-db**
   - Trong tab **Connectivity & security**
   - Copy giá trị **Endpoint**
   - Ví dụ: `game-store-db.c9xyz123456.ap-southeast-1.rds.amazonaws.com`
   - Port: `3306`

3. **Lưu Chi tiết Kết nối**
   ```
   Endpoint: game-store-db.c9xyz123456.ap-southeast-1.rds.amazonaws.com
   Port: 3306
   Database: identity_service
   Username: admin
   Password: GameStore2025!
   ```

#### Bước 5: Kiểm tra Kết nối

**Tùy chọn A: Sử dụng MySQL CLI**

```bash
# Cài đặt MySQL client (nếu chưa cài)
# Windows: Download từ https://dev.mysql.com/downloads/mysql/
# macOS: brew install mysql-client
# Linux: sudo apt-get install mysql-client

# Kiểm tra kết nối
mysql -h game-store-db.c9xyz123456.ap-southeast-1.rds.amazonaws.com \
      -P 3306 \
      -u admin \
      -p

# Nhập password khi được nhắc: GameStore2025!

# Bạn sẽ thấy:
# mysql> 

# Liệt kê databases
SHOW DATABASES;

# Chọn database
USE identity_service;

# Thoát
EXIT;
```

**Tùy chọn B: Sử dụng MySQL Workbench**

1. Download MySQL Workbench: https://dev.mysql.com/downloads/workbench/
2. Mở MySQL Workbench
3. Click **+** để thêm kết nối mới
4. Cấu hình:
   - Connection Name: `Game Store RDS`
   - Hostname: `game-store-db.c9xyz123456.ap-southeast-1.rds.amazonaws.com`
   - Port: `3306`
   - Username: `admin`
   - Password: Click **Store in Vault** → Nhập `GameStore2025!`
5. Click **Test Connection** → Sẽ hiển thị "Successfully connected"
6. Click **OK** để lưu

**Tùy chọn C: Sử dụng DBeaver (Khuyến nghị)**

1. Download DBeaver: https://dbeaver.io/download/
2. New Database Connection → MySQL
3. Điền thông tin kết nối
4. Test Connection → Sẽ thành công
5. Click **Finish**

#### Bước 6: Khởi tạo Database Schema

Ứng dụng Spring Boot của chúng ta sử dụng **JPA/Hibernate** với `spring.jpa.hibernate.ddl-auto=update`, nên các bảng sẽ được tạo tự động khi backend kết nối lần đầu.

Tuy nhiên, bạn có thể xác minh hoặc khởi tạo thủ công:

```sql
-- Kết nối đến RDS
mysql -h YOUR_RDS_ENDPOINT -u admin -p identity_service

-- Xác minh database
SHOW DATABASES;

-- Sử dụng database
USE identity_service;

-- Liệt kê tables (ban đầu sẽ trống)
SHOW TABLES;

-- Sau khi chạy ứng dụng Spring Boot một lần, bạn sẽ thấy:
-- +----------------------------+
-- | Tables_in_identity_service |
-- +----------------------------+
-- | cart                       |
-- | cart_item                  |
-- | category                   |
-- | game                       |
-- | invalidated_token          |
-- | orders                     |
-- | order_detail               |
-- | payment                    |
-- | role                       |
-- | transaction                |
-- | user                       |
-- | user_role                  |
-- +----------------------------+
```

#### Bước 7: Cấu hình Ứng dụng Spring Boot

1. **Cập nhật `application.yaml`**

```yaml
spring:
  datasource:
    url: jdbc:mysql://game-store-db.c9xyz123456.ap-southeast-1.rds.amazonaws.com:3306/identity_service
    username: admin
    password: GameStore2025!
    driver-class-name: com.mysql.cj.jdbc.Driver
  
  jpa:
    hibernate:
      ddl-auto: update  # Tự động tạo/cập nhật tables
    show-sql: true      # Log SQL queries
    properties:
      hibernate:
        dialect: org.hibernate.dialect.MySQLDialect
        format_sql: true
```

2. **Hoặc sử dụng Environment Variables** (Khuyến nghị)

```yaml
spring:
  datasource:
    url: jdbc:mysql://${RDS_ENDPOINT:localhost}:${RDS_PORT:3306}/${RDS_DATABASE:identity_service}
    username: ${RDS_USERNAME:root}
    password: ${RDS_PASSWORD:password}
```

Sau đó set environment variables:
```bash
# Windows PowerShell
$env:RDS_ENDPOINT="game-store-db.c9xyz123456.ap-southeast-1.rds.amazonaws.com"
$env:RDS_PORT="3306"
$env:RDS_DATABASE="identity_service"
$env:RDS_USERNAME="admin"
$env:RDS_PASSWORD="GameStore2025!"
```

#### Bước 8: Kiểm tra Kết nối Backend

1. **Điều hướng đến Thư mục Backend**
   ```bash
   cd Back-End
   ```

2. **Set Environment Variables**
   ```bash
   # Windows PowerShell
   $env:RDS_ENDPOINT="game-store-db.c9xyz123456.ap-southeast-1.rds.amazonaws.com"
   $env:RDS_PORT="3306"
   $env:RDS_DATABASE="identity_service"
   $env:RDS_USERNAME="admin"
   $env:RDS_PASSWORD="GameStore2025!"
   ```

3. **Chạy Ứng dụng Spring Boot**
   ```bash
   mvn spring-boot:run
   ```

4. **Xác minh Kết nối trong Logs**
   Tìm kiếm:
   ```
   Hikari Pool-1 - Starting...
   Hikari Pool-1 - Start completed.
   HHH000204: Processing PersistenceUnitInfo [name: default]
   HHH000412: Hibernate ORM core version ...
   HHH000400: Using dialect: org.hibernate.dialect.MySQLDialect
   ```

5. **Kiểm tra Database Tables**
   ```bash
   mysql -h YOUR_RDS_ENDPOINT -u admin -p identity_service

   SHOW TABLES;
   # Bây giờ sẽ thấy tất cả entity tables được tạo bởi Hibernate
   ```

#### Bước 9: Xác minh Database Operations

Kiểm tra CRUD operations qua API:

```bash
# Tạo test user (qua Postman hoặc curl)
curl -X POST http://localhost:8080/identity/users \
  -H "Content-Type: application/json" \
  -d '{
    "username": "testuser",
    "password": "Test123!",
    "email": "test@example.com"
  }'

# Xác minh trong database
mysql -h YOUR_RDS_ENDPOINT -u admin -p identity_service

SELECT * FROM user;
# Sẽ hiển thị test user
```

#### RDS Monitoring & Maintenance

**CloudWatch Metrics:**
- RDS Dashboard → Chọn `game-store-db` → Tab **Monitoring**
- Các metrics quan trọng:
  - CPU Utilization
  - Database Connections
  - Read/Write IOPS
  - Free Storage Space

**Automated Backups:**
- RDS Dashboard → `game-store-db` → Tab **Maintenance & backups**
- Backups được thực hiện hàng ngày trong backup window
- Retention: 7 ngày (có thể cấu hình)

**Manual Snapshots:**
```bash
# Qua AWS CLI
aws rds create-db-snapshot \
    --db-instance-identifier game-store-db \
    --db-snapshot-identifier game-store-db-snapshot-$(date +%Y%m%d)
```

#### Xử lý Sự cố

**Vấn đề: Không thể kết nối đến RDS**

✅ **Kiểm tra Security Group:**
```bash
# Xác minh security group cho phép port 3306
aws ec2 describe-security-groups \
    --group-names game-store-rds-sg \
    --query 'SecurityGroups[0].IpPermissions'
```

✅ **Kiểm tra RDS Public Accessibility:**
- RDS Dashboard → `game-store-db` → **Connectivity & security**
- Publicly accessible: **Yes**

✅ **Kiểm tra Network Connectivity:**
```bash
# Kiểm tra xem port có thể kết nối được không
Test-NetConnection -ComputerName game-store-db.c9xyz123456.ap-southeast-1.rds.amazonaws.com -Port 3306
```

**Vấn đề: "Access denied for user 'admin'"**

✅ Kiểm tra credentials khớp chính xác
✅ Reset password trong RDS Console → **Modify** → **New master password**

**Vấn đề: "Unknown database 'identity_service'"**

✅ Database không được tạo trong quá trình thiết lập RDS
✅ Tạo thủ công:
```sql
mysql -h YOUR_RDS_ENDPOINT -u admin -p
CREATE DATABASE identity_service;
```

#### Mẹo Tối ưu Chi phí

1. **Sử dụng Free Tier:** db.t3.micro đủ điều kiện Free Tier trong 12 tháng
2. **Dừng khi không sử dụng:** 
   ```bash
   aws rds stop-db-instance --db-instance-identifier game-store-db
   ```
3. **Giảm backup retention:** 1-3 ngày cho dev/test
4. **Tắt Multi-AZ:** Single-AZ đủ cho development
5. **Giám sát storage:** Xóa logs và dữ liệu cũ

#### Tổng kết

Bạn đã thành công:
- ✅ Tạo RDS MySQL instance (db.t3.micro)
- ✅ Cấu hình security groups cho truy cập database
- ✅ Kiểm tra kết nối bằng MySQL client
- ✅ Khởi tạo database schema
- ✅ Kết nối Spring Boot backend với RDS
- ✅ Xác minh CRUD operations

**Bước tiếp theo:** Cấu hình S3 Storage cho upload hình ảnh →

{{% notice info %}}
**Thời gian Ước tính:** 15-20 phút cho thiết lập và kiểm tra RDS
{{% /notice %}}
