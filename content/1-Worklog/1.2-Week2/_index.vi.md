---
title: "Worklog Tuần 2"
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---
{{% notice warning %}} 
⚠️ **Lưu ý:** Thông tin dưới đây chỉ để tham khảo. Vui lòng **không sao chép nguyên văn** cho báo cáo của bạn, bao gồm cả cảnh báo này.
{{% /notice %}}


### Mục tiêu tuần 2:

* Tìm hiểu về dịch vụ lưu trữ AWS (S3) và các khái niệm cơ bản.
* Hiểu Virtual Private Cloud (VPC) cho networking.
* Nghiên cứu Identity and Access Management (IAM) cho bảo mật.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tạo S3 bucket để host static website <br> - Upload HTML/CSS demo files lên S3 | 15/09/2025   | 15/09/2025      | AWS Journey |
| 3   | - Bật tính năng Static Website Hosting trong S3 <br> - Cấu hình bucket policy cho public read access <br> - Test truy cập website qua link S3 | 16/09/2025   | 16/09/2025      | AWS Journey |
| 4   | - Tạo RDS MySQL instance (Free Tier) <br> - Cấu hình VPC security group cho phép kết nối DB <br> - Ghi chú endpoint & credentials | 17/09/2025   | 17/09/2025      | AWS Journey |
| 5   | - Tạo EC2 instance, cài đặt MySQL client <br> - Kết nối từ EC2 đến RDS qua command line <br> - Test tạo database & bảng đơn giản | 18/09/2025   | 18/09/2025      | AWS Journey |
| 6   | - Tìm hiểu về Route53, tạo hosted zone <br> - Thêm A/CNAME records để trỏ domain đến S3 static site <br> - Test truy cập website qua domain | 19/09/2025   | 19/09/2025      | AWS Journey |


### Kết quả đạt được tuần 2:

* **Tìm hiểu về Amazon S3 storage:**
  * Hiểu S3 là dịch vụ lưu trữ đối tượng cho files và dữ liệu
  * Học về các lớp lưu trữ khác nhau:
    * S3 Standard - cho dữ liệu truy cập thường xuyên
    * S3 Glacier - cho lưu trữ và sao lưu
  * Nghiên cứu bucket policies và kiểm soát truy cập
  * Tìm hiểu về versioning cho bảo vệ dữ liệu

* **Thực hành với S3:**
  * Tạo S3 buckets với tên duy nhất
  * Upload và download files sử dụng Console và CLI:
    * `aws s3 cp file.txt s3://my-bucket/`
    * `aws s3 ls s3://my-bucket/`
  * Cấu hình bucket policies cho kiểm soát truy cập
  * Bật versioning trên buckets
  * Thiết lập static website hosting đơn giản
  * Thử nghiệm các thao tác quản lý file

* **Hiểu các khái niệm cơ bản về VPC networking:**
  * Học VPC là mạng ảo trong AWS
  * Nghiên cứu CIDR blocks và IP addressing
  * Hiểu sự khác biệt giữa public và private subnets:
    * Public subnet - có truy cập internet
    * Private subnet - không có truy cập internet trực tiếp
  * Học về các thành phần VPC:
    * Internet Gateway - cho kết nối internet
    * Route Tables - cho định tuyến traffic
    * Security Groups - firewall ở mức instance
    * NACLs - firewall ở mức subnet

* **Thực hành với VPC:**
  * Tạo custom VPC với CIDR block
  * Tạo public và private subnets
  * Cấu hình route tables cho subnets
  * Gắn Internet Gateway vào VPC
  * Khởi chạy EC2 instance trong custom VPC
  * Cấu hình Security Groups cho kiểm soát truy cập
  * Kiểm tra kết nối mạng

* **Học các khái niệm cơ bản về IAM security:**
  * Hiểu IAM cho quản lý truy cập
  * Học về các thành phần IAM:
    * Users - tài khoản cá nhân
    * Groups - tập hợp users
    * Roles - cho các dịch vụ AWS
    * Policies - định nghĩa quyền
  * Nghiên cứu IAM best practices:
    * Sử dụng MFA cho bảo mật
    * Tuân theo nguyên tắc least privilege
    * Tạo users riêng thay vì dùng root

* **Thực hành với IAM:**
  * Tạo IAM users và groups
  * Gắn policies cho users và groups
  * Tạo custom policies cho permissions cụ thể
  * Thiết lập IAM roles cho EC2
  * Bật MFA cho users
  * Kiểm tra quyền và truy cập của users

* **Đạt được các kỹ năng AWS thực tế:**
  * Có thể quản lý lưu trữ với S3
  * Hiểu networking cơ bản với VPC
  * Biết cách kiểm soát truy cập với IAM
  * Sẵn sàng cho các chủ đề nâng cao hơn ở Tuần 3
  * Bật versioning và kiểm tra khôi phục phiên bản object
  * Thiết lập lifecycle policies để tự động chuyển đổi objects giữa các storage classes
  * Cấu hình static website hosting trên S3 với trang index và error tùy chỉnh
  * Kiểm tra cross-region replication (CRR) cho disaster recovery

* **Đạt được kiến thức toàn diện về VPC networking:**
  * Hiểu VPC như một mạng ảo được cách ly logic trong AWS cloud
  * Thành thạo ký hiệu CIDR và lập kế hoạch địa chỉ IP:
    * Dải IP private (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16)
    * Kích thước subnet và phân bổ CIDR block
  * Học sự khác biệt giữa public và private subnets:
    * **Public Subnet:** Có route đến Internet Gateway, instances nhận public IPs
    * **Private Subnet:** Không truy cập internet trực tiếp, sử dụng NAT cho traffic outbound
  * Hiểu các thành phần VPC:
    * **Internet Gateway (IGW):** Cho phép kết nối internet cho VPC
    * **NAT Gateway:** Cho phép instances trong private subnet truy cập internet
    * **Route Tables:** Kiểm soát định tuyến traffic giữa các subnets
    * **VPC Peering:** Kết nối VPCs một cách private
  * Thành thạo các lớp bảo mật VPC:
    * **Security Groups:** Stateful firewall ở mức instance
    * **Network ACLs:** Stateless firewall ở mức subnet

* **Hoàn thành thành công các bài lab thực hành VPC:**
  * Tạo custom VPC với CIDR block 10.0.0.0/16
  * Tạo nhiều subnets trên các availability zones:
    * Public subnet: 10.0.1.0/24 trong ap-southeast-1a
    * Private subnet: 10.0.2.0/24 trong ap-southeast-1a
    * Các subnet bổ sung trong ap-southeast-1b cho high availability
  * Cấu hình route tables:
    * Public route table với 0.0.0.0/0 → IGW
    * Private route table với 0.0.0.0/0 → NAT Gateway
  * Gắn Internet Gateway vào VPC
  * Tạo và cấu hình NAT Gateway trong public subnet
  * Khởi chạy EC2 instances trong cả public và private subnets
  * Cấu hình security groups cho web server (port 80, 443) và SSH access
  * Kiểm tra connectivity và traffic flow giữa các subnets

* **Đạt được chuyên môn về bảo mật IAM:**
  * Hiểu IAM như nền tảng của bảo mật AWS và kiểm soát truy cập
  * Thành thạo các thành phần IAM:
    * **Users:** Danh tính cá nhân với credentials dài hạn
    * **Groups:** Tập hợp users với permissions chung
    * **Roles:** Credentials tạm thời cho services hoặc federated users
    * **Policies:** Tài liệu JSON định nghĩa permissions
  * Học cấu trúc policy và logic đánh giá:
    * Effect (Allow/Deny)
    * Action (service:operation)
    * Resource (ARN)
    * Condition (ràng buộc tùy chọn)
  * Hiểu managed policies vs inline policies
  * Học các phương pháp bảo mật tốt nhất của AWS:
    * Bật MFA cho tất cả users
    * Sử dụng roles thay vì chia sẻ credentials
    * Áp dụng nguyên tắc least privilege
    * Xoay vòng credentials thường xuyên
    * Sử dụng CloudTrail cho audit logging

* **Hoàn thành thành công các bài lab thực hành IAM:**
  * Tạo IAM users với programmatic và console access
  * Tổ chức users thành groups (Developers, Admins, ReadOnly)
  * Gắn AWS managed policies (AdministratorAccess, PowerUserAccess, ReadOnlyAccess)
  * Tạo custom IAM policies với permissions cụ thể:
    ```json
    {
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:PutObject"],
      "Resource": "arn:aws:s3:::my-bucket/*"
    }
    ```
  * Tạo IAM roles cho EC2 instances để truy cập S3
  * Gắn roles vào EC2 instances đang chạy
  * Bật MFA cho root và IAM users
  * Kiểm tra và xác thực permission boundaries
  * Sử dụng IAM Policy Simulator để test policies trước khi áp dụng
  * Xem xét IAM Access Advisor để loại bỏ permissions không sử dụng

* **Phát triển kỹ năng AWS nâng cao:**
  * Khả năng thiết kế và triển khai kiến trúc đám mây an toàn, có khả năng mở rộng
  * Hiểu các nguyên tắc AWS Well-Architected Framework:
    * Operational Excellence
    * Security
    * Reliability
    * Performance Efficiency
    * Cost Optimization
  * Thành thạo sử dụng AWS CLI cho tự động hóa và scripting
  * Kiến thức về các phương pháp tốt nhất của AWS cho networking và security
  * Chuẩn bị nền tảng cho các chủ đề Tuần 3 (RDS, Lambda, và nhiều hơn nữa)

* Cung cấp và cấu hình thành công RDS database:
  * Triển khai db.t3.micro PostgreSQL instance theo Free Tier
  * Tạo custom VPC security group cho phép truy cập cổng 5432
  * Cấu hình DB subnet group trên nhiều availability zones
  * Đặt master username, password và tên database ban đầu
  * Kết nối từ EC2 instance sử dụng công cụ dòng lệnh `psql`
  * Tạo sample database schema với các bảng và mối quan hệ
  * Thực thi các câu lệnh SQL để chèn và truy xuất dữ liệu

* Khám phá DNS và quản lý tên miền với Route 53:
  * Hiểu hệ thống phân cấp DNS và các loại record
  * Tìm hiểu về hosted zones (public vs private)
  * Nghiên cứu routing policies: Simple, Weighted, Latency-based, Failover, Geolocation
  * Khám phá health checks và cơ chế DNS failover

* Triển khai thành công định tuyến tên miền với Route 53:
  * Tạo hosted zone cho cấu hình tên miền tùy chỉnh
  * Thêm A record (alias) trỏ đến S3 website endpoint
  * Cấu hình giá trị DNS TTL phù hợp
  * Xác minh DNS propagation sử dụng lệnh `nslookup` và `dig`
  * Truy cập thành công website tĩnh qua tên miền tùy chỉnh
  * Thiết lập health check để giám sát tình trạng website

* Tích hợp nhiều dịch vụ AWS thành kiến trúc gắn kết:
  * S3 cho static content hosting
  * RDS cho lưu trữ dữ liệu có cấu trúc
  * EC2 cho kết nối database client
  * Route 53 cho quản lý DNS và định tuyến tên miền
  * VPC security groups cho kiểm soát truy cập cấp mạng

* Phát triển kỹ năng tích hợp dịch vụ AWS và khắc phục sự cố:
  * Debug các vấn đề cấu hình security group
  * Giải quyết độ trễ DNS propagation
  * Sửa lỗi quyền truy cập S3 bucket policy
  * Tối ưu RDS parameter groups cho hiệu suất

* Xây dựng nền tảng cho các chủ đề nâng cao ở Tuần 3 bao gồm mạng phân phối nội dung và chiến lược caching.


