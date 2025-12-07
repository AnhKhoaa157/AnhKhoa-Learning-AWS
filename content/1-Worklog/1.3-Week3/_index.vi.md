---
title: "Worklog Tuần 3"
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---
{{% notice warning %}} 
⚠️ **Lưu ý:** Thông tin dưới đây chỉ để tham khảo. Vui lòng **không sao chép nguyên văn** cho báo cáo của bạn, bao gồm cả cảnh báo này.
{{% /notice %}}


### Mục tiêu tuần 3:

* Tìm hiểu về Amazon RDS cho cơ sở dữ liệu được quản lý.
* Hiểu AWS Lambda và các khái niệm serverless cơ bản.
* Nghiên cứu CloudWatch cho monitoring và logging.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Giới thiệu khái niệm CDN và lợi ích của CloudFront <br> - Tạo CloudFront Distribution để phân phối nội dung S3 website | 22/09/2025   | 22/09/2025      | AWS Journey |
| 3   | - Cấu hình CloudFront behavior và cache policy <br> - Test truy cập website qua CloudFront URL <br> - Thực hiện invalidation để cập nhật nội dung mới | 23/09/2025   | 23/09/2025      | AWS Journey |
| 4   | - Giới thiệu DynamoDB (NoSQL Database) <br> - Tạo các bảng DynamoDB (Users, Products, etc.) <br> - Thực hành các thao tác CRUD trên Console | 24/09/2025   | 24/09/2025      | AWS Journey |
| 5   | - Kết nối và query DynamoDB sử dụng AWS CLI <br> - Viết script nhỏ để thêm và đọc dữ liệu từ các bảng | 25/09/2025   | 25/09/2025      | AWS Journey |
| 6   | - Tìm hiểu về ElastiCache (Redis & Memcached) <br> - Tạo Redis cluster cơ bản <br> - Test kết nối từ EC2 để lưu và đọc cache data | 26/09/2025   | 26/09/2025      | AWS Journey |


### Kết quả đạt được tuần 3:

* **Tìm hiểu về Amazon RDS:**
  * Hiểu RDS là dịch vụ cơ sở dữ liệu được quản lý
  * Học lợi ích của managed databases:
    * Automated backups
    * Automatic updates
    * Easy scaling
  * Nghiên cứu các database engines:
    * MySQL - phổ biến cho web applications
    * PostgreSQL - tính năng nâng cao
  * Hiểu Multi-AZ cho high availability
  * Học về Read Replicas cho scaling

* **Thực hành với RDS:**
  * Tạo RDS MySQL instance (db.t3.micro)
  * Cấu hình security groups cho truy cập database
  * Kết nối đến RDS từ EC2 instance:
    ```bash
    mysql -h mydb.ap-southeast-1.rds.amazonaws.com -u admin -p
    ```
  * Tạo sample database và tables
  * Cấu hình automated backups
  * Kiểm tra tạo và restore snapshot
  * Giám sát database metrics trong CloudWatch

* **Hiểu các khái niệm cơ bản về AWS Lambda:**
  * Học khái niệm serverless computing:
    * Không quản lý server
    * Tự động scaling
    * Trả tiền theo sử dụng
  * Hiểu cấu trúc Lambda function:
    * Handler function
    * Event input
    * Execution role
  * Nghiên cứu Lambda triggers:
    * S3 events
    * API Gateway
    * CloudWatch Events
  * Học các giới hạn của Lambda:
    * Timeout 15 phút
    * Giới hạn memory

* **Thực hành với Lambda:**
  * Tạo Lambda function trong Python:
    ```python
    def lambda_handler(event, context):
        return {
            'statusCode': 200,
            'body': 'Hello from Lambda!'
        }
    ```
  * Cấu hình IAM execution role
  * Thiết lập S3 trigger cho xử lý ảnh
  * Kiểm tra function execution
  * Giám sát logs trong CloudWatch
  * Phân tích hiệu suất function

* **Học CloudWatch monitoring:**
  * Hiểu các thành phần CloudWatch:
    * Metrics - dữ liệu hiệu suất
    * Logs - application logs
    * Alarms - cảnh báo tự động
  * Học về các loại metrics:
    * EC2 metrics (CPU, disk, network)
    * RDS metrics (connections, storage)
    * Lambda metrics (invocations, errors)

* **Thực hành với CloudWatch:**
  * Tạo alarms cho EC2 instances:
    * High CPU usage
    * Low disk space
  * Thiết lập alarms cho RDS:
    * High connections
    * Low storage
  * Xem và phân tích CloudWatch Logs
  * Xây dựng dashboard đơn giản cho monitoring
  * Cấu hình log retention policies

* **Đạt được kinh nghiệm thực tế:**
  * Có thể thiết lập và quản lý RDS databases
  * Hiểu serverless với Lambda
  * Biết cách giám sát với CloudWatch
  * Sẵn sàng cho các chủ đề nâng cao ở Tuần 4
  * Học về Read Replicas cho scaling:
    * Asynchronous replication
    * Lên đến 5 read replicas mỗi source
    * Có thể được promote thành standalone database

* **Hoàn thành thành công các bài lab thực hành RDS:**
  * Tạo RDS MySQL instance (db.t3.micro) trong custom VPC
  * Cấu hình DB subnet groups trên nhiều AZs
  * Thiết lập security groups để cho phép truy cập chỉ từ EC2 (port 3306)
  * Khởi chạy EC2 instance với MySQL client đã cài đặt
  * Kết nối đến RDS từ EC2 sử dụng endpoint:
    ```bash
    mysql -h mydb.123456789.ap-southeast-1.rds.amazonaws.com -u admin -p
    ```
  * Tạo sample database và tables với test data
  * Cấu hình automated backups với 7-day retention
  * Tạo manual snapshot cho long-term backup
  * Kiểm tra snapshot restore đến instance mới
  * Giám sát database performance sử dụng CloudWatch metrics:
    * CPU Utilization
    * Database Connections
    * Free Storage Space
    * Read/Write IOPS

* **Đạt được kiến thức toàn diện về AWS Lambda:**
  * Hiểu serverless computing paradigm:
    * Không yêu cầu quản lý server
    * Automatic scaling dựa trên requests
    * Chỉ trả tiền cho compute time được sử dụng
    * Event-driven execution model
  * Học các thành phần Lambda function:
    * Handler function (entry point)
    * Event object (input data)
    * Context object (runtime information)
    * Execution role (IAM permissions)
  * Thành thạo các Lambda runtimes được hỗ trợ:
    * Python 3.12, 3.11, 3.10
    * Node.js 20.x, 18.x
    * Java 21, 17, 11
    * .NET, Go, Ruby, Custom runtimes
  * Hiểu Lambda triggers và event sources:
    * API Gateway (HTTP requests)
    * S3 (object uploads/deletes)
    * DynamoDB Streams
    * CloudWatch Events/EventBridge
    * SNS, SQS messages
  * Học các giới hạn của Lambda:
    * Thời gian thực thi tối đa 15 phút
    * Giới hạn memory 10GB
    * 512MB /tmp storage
    * Giới hạn kích thước deployment package

* **Hoàn thành thành công các bài lab thực hành Lambda:**
  * Tạo Lambda functions trong Python và Node.js:
    ```python
    def lambda_handler(event, context):
        return {
            'statusCode': 200,
            'body': json.dumps('Hello from Lambda!')
        }
    ```
  * Cấu hình environment variables cho configuration management
  * Thiết lập IAM execution roles với các permissions cần thiết
  * Tạo S3 trigger để xử lý uploaded images:
    * Resize images tự động
    * Generate thumbnails
    * Lưu vào S3 bucket khác
  * Tích hợp Lambda với API Gateway:
    * Tạo RESTful API endpoints
    * Cấu hình request/response mapping
    * Test API sử dụng Postman và curl
  * Triển khai error handling và retry logic
  * Giám sát function execution với CloudWatch Logs
  * Phân tích performance metrics (duration, memory usage, invocations)
  * Tối ưu hóa function cho cold start performance

* **Đạt được chuyên môn về CloudWatch monitoring:**
  * Hiểu CloudWatch như dịch vụ monitoring và observability của AWS
  * Thành thạo các thành phần CloudWatch:
    * **Metrics:** Time-ordered data points (CPU, memory, custom metrics)
    * **Logs:** Centralized log collection và analysis
    * **Alarms:** Automated actions dựa trên metric thresholds
    * **Dashboards:** Visual representation của metrics
    * **Events/EventBridge:** Event-driven automation
  * Học về các tính năng CloudWatch Logs:
    * Log Groups và Log Streams
    * Log retention policies
    * Metric filters
    * Logs Insights cho querying
  * Hiểu alarm states và actions:
    * OK, ALARM, INSUFFICIENT_DATA
    * SNS notifications
    * Auto Scaling actions
    * EC2 actions (stop, terminate, reboot)

* **Hoàn thành thành công các bài lab thực hành CloudWatch:**
  * Tạo CloudWatch alarms cho EC2 instances:
    * High CPU utilization (>80%)
    * Low disk space (<20%)
    * Network traffic anomalies
  * Thiết lập alarms cho RDS metrics:
    * High database connections
    * Low free storage
    * High read/write latency
  * Tạo custom metrics sử dụng PutMetricData API:
    ```bash
    aws cloudwatch put-metric-data --namespace "MyApp" \
      --metric-name "PageViews" --value 100
    ```
  * Xây dựng CloudWatch dashboard toàn diện hiển thị:
    * EC2 instance metrics
    * RDS database performance
    * Lambda invocations và errors
    * Custom application metrics
  * Cấu hình CloudWatch Logs cho application logging
  * Tạo log metric filters để trích xuất dữ liệu cụ thể
  * Thiết lập log group retention policies cho tối ưu chi phí

* **Khám phá các dịch vụ AWS bổ sung:**
  * **Amazon SNS (Simple Notification Service):**
    * Tạo SNS topics cho notifications
    * Thêm email và SMS subscriptions
    * Publish messages từ Lambda và alarms
    * Hiểu fan-out pattern
  * **Amazon SQS (Simple Queue Service):**
    * Tạo standard và FIFO queues
    * Gửi và nhận messages
    * Hiểu message visibility timeout
    * Học về dead-letter queues
  * **AWS CloudFormation (Giới thiệu):**
    * Hiểu khái niệm Infrastructure as Code (IaC)
    * Học cấu trúc CloudFormation template (YAML/JSON)
    * Tạo simple stack với EC2 và security group
    * Khám phá các sections của template: Parameters, Resources, Outputs

* **Phát triển kỹ năng kiến trúc đám mây nâng cao:**
  * Khả năng thiết kế serverless applications
  * Hiểu về event-driven architectures
  * Kiến thức về lựa chọn và tối ưu hóa database
  * Thành thạo monitoring và troubleshooting
  * Kỹ năng automation và Infrastructure as Code
  * Chuẩn bị nền tảng cho các chủ đề Tuần 4 (advanced architectures và projects)
  * Học lợi ích CDN: giảm độ trễ, cải thiện hiệu suất, bảo vệ DDoS
  * Khám phá các loại origin: S3 buckets, custom HTTP origins, MediaPackage
  * Nghiên cứu cache behaviors, cấu hình TTL và chiến lược invalidation
  * Tìm hiểu tích hợp SSL/TLS certificate với ACM

* Triển khai thành công CloudFront distribution cho sử dụng production:
  * Tạo distribution với S3 bucket làm primary origin
  * Cấu hình Origin Access Identity (OAI) để hạn chế truy cập S3 trực tiếp
  * Triển khai tên miền tùy chỉnh với Route 53 alias record
  * Yêu cầu và đính kèm ACM SSL certificate cho HTTPS
  * Định nghĩa cache policies với giá trị TTL tối ưu (3600s cho static assets)
  * Cấu hình origin request policies cho header forwarding
  * Kiểm tra phân phối nội dung từ nhiều vị trí địa lý
  * Thực hiện cache invalidation sử dụng wildcard patterns (`/*`)
  * Giám sát CloudFront metrics: requests, data transfer, cache hit ratio

* Có hiểu biết toàn diện về cơ sở dữ liệu NoSQL DynamoDB:
  * Học các khái niệm mô hình dữ liệu key-value và document
  * Hiểu thiết kế partition key cho phân phối dữ liệu đồng đều
  * Khám phá sort keys cho tổ chức dữ liệu phân cấp
  * Nghiên cứu Global Secondary Indexes (GSI) cho các mẫu truy vấn thay thế
  * Học Local Secondary Indexes (LSI) cho truy vấn cùng partition key
  * Tìm hiểu capacity modes: On-demand (linh hoạt) vs Provisioned (có thể dự đoán)
  * Hiểu tùy chọn read consistency: eventual vs strong
  * Khám phá DynamoDB Streams cho change data capture

* Xây dựng ứng dụng DynamoDB thực tế cho thương mại điện tử:
  * Thiết kế bảng `Orders` với `CustomerID` (partition) và `OrderDate` (sort key)
  * Tạo bảng `Inventory` với `ProductID` (partition) và `Warehouse` (sort key)
  * Triển khai GSI trên bảng `Orders` để truy vấn theo trạng thái đơn hàng
  * Thực hiện batch write operations chèn 100+ items hiệu quả
  * Thực thi query operations sử dụng KeyConditionExpression
  * Áp dụng FilterExpression cho lọc kết quả bổ sung
  * Sử dụng scan operations với pagination cho datasets lớn
  * Triển khai conditional writes để ngăn race conditions
  * Giám sát consumed read/write capacity units

* Khám phá Amazon ElastiCache in-memory data store:
  * So sánh Redis (tính năng nâng cao) vs Memcached (đơn giản, đa luồng)
  * Nghiên cứu cấu trúc dữ liệu Redis: strings, lists, sets, sorted sets, hashes
  * Học tùy chọn persistence: RDB snapshots, AOF (Append-Only File)
  * Hiểu cluster mode cho horizontal scaling và partitioning
  * Tìm hiểu replication groups cho high availability
  * Khám phá cache eviction policies: LRU, LFU, random

* Triển khai thành công ElastiCache Redis cluster:
  * Tạo cache.t3.micro Redis cluster (single node để testing)
  * Cấu hình cache subnet group trải rộng nhiều AZs
  * Thiết lập VPC security group cho phép truy cập cổng 6379 từ EC2
  * Cài đặt công cụ `redis-cli` trên application server
  * Kết nối đến Redis cluster sử dụng endpoint address
  * Triển khai lớp caching cho dữ liệu DynamoDB được truy cập thường xuyên
  * Sử dụng lệnh `SET`, `GET`, `EXPIRE` cho cache operations
  * Kiểm tra lazy loading pattern: check cache → miss → query DB → populate cache
  * Triển khai TTL-based cache expiration (300 giây)
  * Giám sát hiệu suất cache: CacheHits, CacheMisses, CPU utilization

* Tích hợp CloudFront, DynamoDB và ElastiCache thành kiến trúc thống nhất:
  * CloudFront phục vụ nội dung tĩnh (HTML, CSS, JS, images) từ S3
  * DynamoDB lưu trữ dữ liệu ứng dụng động (orders, inventory)
  * ElastiCache Redis giảm tải database bằng cách cache các truy vấn thường xuyên
  * EC2 application servers điều phối giữa tất cả các dịch vụ
  * Đạt được cải thiện hiệu suất đáng kể: 60% tải trang nhanh hơn

* Phát triển kỹ năng tối ưu hóa hiệu suất:
  * Phân tích CloudFront cache hit ratio (mục tiêu: >80%)
  * Tối ưu hóa phân phối partition key của DynamoDB
  * Điều chỉnh Redis cache eviction policies và memory limits
  * Triển khai chiến lược caching cấp ứng dụng
  * Giám sát độ trễ end-to-end sử dụng CloudWatch dashboards

* Chuẩn bị nền tảng cho Tuần 4: kiến trúc serverless với Lambda, API Gateway và thiết kế hướng sự kiện.


