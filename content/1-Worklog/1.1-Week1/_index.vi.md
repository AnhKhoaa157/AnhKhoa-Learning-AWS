---
title: "Worklog Tuần 1"
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---
{{% notice warning %}} 
⚠️ **Lưu ý:** Thông tin dưới đây chỉ để tham khảo. Vui lòng **không sao chép nguyên văn** cho báo cáo của bạn, bao gồm cả cảnh báo này.
{{% /notice %}}


### Mục tiêu tuần 1:

* Kết nối và làm quen với các thành viên của First Cloud Journey.
* Hiểu các dịch vụ AWS cơ bản, cách sử dụng console & CLI.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Làm quen với các thành viên FCJ <br> - Đọc và ghi chép quy định của đơn vị thực tập <br> - Thiết lập môi trường phát triển (Git, VS Code)      | 08/09/2025   | 08/09/2025      | Tài liệu nội bộ FCJ                        |
| 3   | - Tìm hiểu về AWS và các loại dịch vụ <br>&emsp; + Compute (EC2, Lambda) <br>&emsp; + Storage (S3, EBS) <br>&emsp; + Networking (VPC) <br>&emsp; + Database (RDS, DynamoDB) | 09/09/2025   | 09/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tạo tài khoản AWS Free Tier <br> - Tìm hiểu về AWS Console & AWS CLI <br> - **Thực hành:** <br>&emsp; + Tạo tài khoản AWS <br>&emsp; + Cài đặt & cấu hình AWS CLI <br>&emsp; + Thử nghiệm các lệnh CLI cơ bản | 10/09/2025   | 10/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Tìm hiểu cơ bản về EC2: <br>&emsp; + Các loại instance <br>&emsp; + AMI <br>&emsp; + EBS <br>&emsp; + Security Groups <br> - Các phương thức kết nối SSH đến EC2 <br> - Tìm hiểu về Elastic IP   | 11/09/2025   | 11/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Thực hành:** <br>&emsp; + Khởi chạy EC2 instance <br>&emsp; + Kết nối qua SSH <br>&emsp; + Gắn EBS volume <br>&emsp; + Kiểm tra các thao tác cơ bản | 12/09/2025   | 12/09/2025      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 1:

* **Hiệu các khái niệm cơ bản về AWS Cloud Computing:**
  * Tìm hiểu AWS là gì và vai trò của nó như một nhà cung cấp dịch vụ đám mây
  * Khám phá các nhóm dịch vụ chính:
    * Compute (EC2, Lambda)
    * Storage (S3, EBS)
    * Networking (VPC)
    * Database (RDS, DynamoDB)
    * Security (IAM, Security Groups)

* **Thiết lập tài khoản và môi trường AWS:**
  * Tạo tài khoản AWS Free Tier thành công
  * Cấu hình các thiết lập bảo mật cơ bản
  * Thiết lập cảnh báo thanh toán để giám sát chi phí

* **Làm quen với AWS Management Console:**
  * Học cách điều hướng giao diện AWS Console
  * Tìm và truy cập các dịch vụ AWS khác nhau
  * Hiểu các dashboard dịch vụ cơ bản

* **Cài đặt và cấu hình AWS CLI:**
  * Cài đặt AWS CLI phiên bản 2 trên máy local
  * Tạo thông tin xác thực IAM user
  * Cấu hình AWS CLI với:
    * Access Key ID
    * Secret Access Key
    * Default Region (ap-southeast-1)
  * Xác minh cấu hình với các lệnh cơ bản

* **Thực hành với các lệnh AWS CLI:**
  * `aws sts get-caller-identity` - Kiểm tra thông tin tài khoản
  * `aws ec2 describe-regions` - Liệt kê các AWS regions
  * `aws ec2 describe-instances` - Xem các EC2 instances
  * `aws s3 ls` - Liệt kê S3 buckets
  * `aws ec2 describe-key-pairs` - Quản lý SSH keys

* **Học các khái niệm cơ bản về EC2:**
  * Hiểu các loại EC2 instance khác nhau (t2, t3, m5)
  * Tìm hiểu về AMI (Amazon Machine Images)
  * Nghiên cứu EBS volumes và các loại của chúng
  * Hiểu Security Groups cho quy tắc firewall

* **Hoàn thành thực hành EC2:**
  * Khởi chạy EC2 instance (t2.micro)
  * Cấu hình Security Group cho truy cập SSH
  * Kết nối đến EC2 qua SSH
  * Tạo và gắn EBS volume
  * Kiểm tra các lệnh Linux cơ bản trên EC2

* **Đạt được các kỹ năng AWS nền tảng:**
  * Có thể sử dụng cả Console và CLI để quản lý tài nguyên
  * Hiểu các khái niệm và thuật ngữ AWS cơ bản
  * Sẵn sàng học các chủ đề nâng cao hơn ở Tuần 2
  * Nắm vững các nhóm dịch vụ cốt lõi và trường hợp sử dụng:
    * **Compute:** EC2 máy chủ ảo, Lambda serverless functions, ECS/EKS containers
    * **Storage:** S3 object storage, EBS block storage, EFS file systems, Glacier archival
    * **Networking:** VPC, Subnets, Route Tables, Internet Gateway, NAT Gateway, CloudFront CDN
    * **Database:** RDS quản lý cơ sở dữ liệu quan hệ, DynamoDB NoSQL, Aurora high-performance DB
    * **Security & Identity:** IAM users/roles/policies, Security Groups, KMS encryption
    * **Monitoring:** CloudWatch metrics, CloudTrail audit logs

* **Thiết lập thành công môi trường AWS:**
  * Tạo và cấu hình tài khoản AWS Free Tier với phương thức thanh toán đã xác minh
  * Bật Multi-Factor Authentication (MFA) để tăng cường bảo mật
  * Thiết lập cảnh báo thanh toán và giới hạn ngân sách để giám sát chi tiêu
  * Cấu hình tùy chọn tài khoản và cài đặt mặc định

* **Thành thạo AWS Management Console:**
  * Thành thạo điều hướng giao diện AWS Console
  * Học cách tìm kiếm và truy cập dịch vụ hiệu quả
  * Hiểu bố cục dashboard dịch vụ và các tính năng chung
  * Khám phá tài liệu dịch vụ và tài nguyên trợ giúp tích hợp

* **Cài đặt và cấu hình AWS CLI thành công:**
  * Tải và cài đặt AWS CLI phiên bản 2 trên máy local
  * Tạo IAM user với quyền truy cập lập trình
  * Cấu hình thông tin xác thực AWS CLI bao gồm:
    * Access Key ID
    * Secret Access Key
    * Default Region (ap-southeast-1 - Singapore)
    * Output format (JSON)
  * Xác minh cấu hình bằng lệnh `aws configure list`

* **Thực thi nhiều lệnh AWS CLI để thực hành:**
  * `aws sts get-caller-identity` - Xác minh danh tính tài khoản và user ARN
  * `aws ec2 describe-regions` - Lấy danh sách tất cả các AWS regions
  * `aws ec2 describe-availability-zones --region ap-southeast-1` - Kiểm tra các AZ khả dụng
  * `aws ec2 describe-instances` - Liệt kê các EC2 instances
  * `aws ec2 create-key-pair --key-name MyKeyPair` - Tạo SSH key pairs
  * `aws ec2 describe-key-pairs` - Quản lý và xem key pairs
  * `aws s3 ls` - Liệt kê S3 buckets
  * `aws ec2 describe-vpcs` - Xem cấu hình VPC

* **Đạt được trình độ thực hành EC2:**
  * Hiểu các loại EC2 instance và đặc điểm của chúng:
    * General Purpose (t2, t3, m5) - Cân bằng compute, memory và networking
    * Compute Optimized (c5, c6i) - Bộ xử lý hiệu năng cao
    * Memory Optimized (r5, x1) - Hiệu suất nhanh cho khối lượng công việc nhiều bộ nhớ
  * Tìm hiểu về Amazon Machine Images (AMI) và vai trò của chúng
  * Hiểu các loại EBS volume và đặc điểm hiệu suất
  * Thành thạo cấu hình Security Group và quy tắc inbound/outbound

* **Hoàn thành thành công bài lab thực hành EC2:**
  * Khởi chạy EC2 instance đầu tiên (t2.micro với Amazon Linux 2023 AMI)
  * Cấu hình Security Group cho phép truy cập SSH trên port 22
  * Kết nối từ xa đến EC2 instance qua SSH sử dụng private key
  * Tạo EBS volume 8GB gp3 trong cùng availability zone
  * Gắn EBS volume vào instance đang chạy
  * Format và mount EBS volume vào file system
  * Cấp phát và gán địa chỉ Elastic IP cho public IP cố định
  * Xác minh cấu hình instance và kết nối

* **Phát triển kỹ năng nền tảng:**
  * Khả năng quản lý tài nguyên AWS thông qua cả Console và CLI
  * Hiểu mô hình định giá AWS và giới hạn Free Tier
  * Kỹ năng khắc phục sự cố cơ bản cho vấn đề kết nối
  * Thực hành ghi chép và ghi chú cho các hoạt động đám mây
  * Chuẩn bị nền tảng kiến thức cho các chủ đề nâng cao ở Tuần 2


