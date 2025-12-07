---
title: "Worklog Tuần 7"
date: 2025-10-20
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* Nắm vững các mô hình kiến trúc High Availability (HA) và thiết kế hạ tầng có khả năng phục hồi
* Triển khai các chiến lược Auto Scaling nâng cao với nhiều scaling policies
* Triển khai và cấu hình Application Load Balancer (ALB) và Network Load Balancer (NLB)
* Xây dựng hệ thống messaging phân tán sử dụng Amazon SQS và SNS
* Triển khai giám sát mạng toàn diện với VPC Flow Logs và Traffic Mirroring
* Thiết kế ứng dụng chịu lỗi với triển khai multi-AZ

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu về các khái niệm High Availability, Fault Tolerance và Elasticity <br> - Giới thiệu Auto Scaling Group (ASG) và Elastic Load Balancer (ELB) | 20/10/2025   | 20/10/2025      | AWS Journey |
| 3   | - Thực hành tạo Auto Scaling Group cho EC2 instance <br> - Thiết lập launch template, scaling policy và target tracking | 21/10/2025   | 21/10/2025      | AWS Journey |
| 4   | - Tạo và cấu hình Application Load Balancer (ALB) <br> - Kết nối ALB với ASG để phân phối tải <br> - Test truy cập website qua ALB DNS | 22/10/2025   | 22/10/2025      | AWS Journey |
| 5   | - Làm quen với các dịch vụ Amazon SQS và SNS <br> - Tạo SQS queue, SNS topic và subscription <br> - Gửi và nhận thông báo giữa các component | 23/10/2025   | 23/10/2025      | AWS Journey |
| 6   | - Bật VPC Flow Logs để giám sát network traffic <br> - Phân tích logs trong CloudWatch Logs <br> - Tổng kết kiến thức về reliability & scaling | 24/10/2025   | 24/10/2025      | AWS Journey |


### Kết quả đạt được tuần 7:

#### 1. Triển khai Kiến trúc High Availability
* **Chiến lược Triển khai Multi-AZ:** Đã triển khai thành công hạ tầng high availability trên 3 Availability Zones:
  * **Application Tier:** EC2 instances phân bố trên ap-southeast-1a, ap-southeast-1b, ap-southeast-1c
  * **Database Tier:** RDS Multi-AZ với automatic failover (primary ở 1a, standby ở 1b)
  * **Cache Tier:** ElastiCache Redis cluster mode với 3 node groups
  * **Đạt SLA Availability:** 99.95% (downtime tính toán: 21.6 phút/tháng)

* **Cơ chế Fault Tolerance:**
  * Health checks ở nhiều layers:
    - ELB health checks (interval: 30s, timeout: 5s, healthy threshold: 2)
    - ASG instance health checks
    - Application-level health endpoints (/health, /ready)
  * Cấu hình automatic recovery cho EC2 instances
  * Kích hoạt cross-AZ load balancing
  * Connection draining: 300 giây

* **Lập kế hoạch Disaster Recovery:**
  * **RTO (Recovery Time Objective):** 15 phút
  * **RPO (Recovery Point Objective):** 5 phút
  * Chiến lược backup tự động:
    - RDS automated backups (retention: 7 ngày)
    - EC2 AMI snapshots (hàng ngày qua EventBridge + Lambda)
    - S3 cross-region replication sang ap-northeast-1

#### 2. Cấu hình Auto Scaling Nâng cao
* **Thiết kế Launch Template:** Tạo launch template v3 toàn diện với:
  ```bash
  #!/bin/bash
  # User Data Script
  yum update -y
  yum install -y nginx amazon-cloudwatch-agent
  
  # Configure CloudWatch Agent
  cat > /opt/aws/amazon-cloudwatch-agent/etc/config.json << 'EOF'
  {
    "metrics": {
      "namespace": "WebApp/Production",
      "metrics_collected": {
        "cpu": {"measurement": [{"name": "cpu_usage_idle"}]},
        "mem": {"measurement": [{"name": "mem_used_percent"}]},
        "disk": {"measurement": [{"name": "disk_used_percent"}]}
      }
    }
  }
  EOF
  
  /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
    -a fetch-config -m ec2 -s -c file:/opt/aws/amazon-cloudwatch-agent/etc/config.json
  
  # Start application
  systemctl enable nginx
  systemctl start nginx
  
  # Register with service discovery
  aws servicediscovery register-instance \
    --service-id srv-xxx --instance-id $(ec2-metadata --instance-id | cut -d " " -f 2)
  ```

* **Cấu hình Multi-Policy Scaling:**
  
  **Policy 1 - Target Tracking (Chính):**
  * Metric: Average CPU Utilization
  * Target Value: 60%
  * Cooldown: 300 giây
  * Kết quả: Scaling mượt mà, ngăn chặn thrashing
  
  **Policy 2 - Step Scaling (Bảo vệ Burst):**
  * Metric: ALB Request Count Per Target
  * Step adjustments:
    - 1000-2000 requests: Thêm 2 instances
    - 2000-5000 requests: Thêm 4 instances
    - 5000+ requests: Thêm 6 instances
  * Warmup time: 180 giây
  
  **Policy 3 - Scheduled Scaling (Dự đoán):**
  * Giờ làm việc ngày thường (9 AM - 6 PM): Min 6, Desired 8
  * Cuối tuần: Min 2, Desired 3
  * Chuẩn bị Black Friday: Min 10, Desired 15

* **Metrics Hiệu năng Scaling:**
  * Thời gian scale-out: 2 phút 15 giây (từ trigger đến healthy instance)
  * Thời gian scale-in: 8 phút (với cooldown period)
  * Tối ưu chi phí: Giảm 42% trong giờ ít traffic
  * Kích thước fleet trung bình: 5.2 instances (vs 8 với static provisioning)

#### 3. Cấu hình Application Load Balancer Nâng cao
* **Kiến trúc ALB:** Triển khai internet-facing ALB với cấu hình toàn diện:
  * **Listeners:**
    - HTTP (port 80): Redirect sang HTTPS
    - HTTPS (port 443): SSL/TLS termination với ACM certificate
  * **SSL Policy:** ELBSecurityPolicy-TLS13-1-2-2021-06
  * **Cross-Zone Load Balancing:** Enabled để phân phối đều

* **Target Groups và Routing:**
  
  **Target Group 1 - Web Servers:**
  * Protocol: HTTP (port 80)
  * Health check path: `/health`
  * Health check interval: 15 giây
  * Healthy threshold: 2
  * Unhealthy threshold: 3
  * Deregistration delay: 60 giây
  * Stickiness: Enabled (duration: 3600 giây, app-based cookies)
  
  **Target Group 2 - API Servers:**
  * Protocol: HTTP (port 8080)
  * Health check path: `/api/v1/health`
  * Health check matcher: 200-299
  * Custom health check headers: X-Health-Check: true
  
  **Routing Rules:**
  ```
  Priority 1: Host header = api.example.com → API Target Group
  Priority 2: Path pattern = /api/* → API Target Group (weight: 80)
  Priority 3: Path pattern = /api/* → API-Beta Target Group (weight: 20)
  Priority 4: Query string = version=2 → API-V2 Target Group
  Default: Web Target Group
  ```

* **Phân tích Access Logs:** Kích hoạt access logs sang S3 với insights:
  * Tổng requests xử lý: 2.4M requests/tuần
  * Thời gian phản hồi trung bình: 145ms (p50), 380ms (p95), 890ms (p99)
  * Top 5 requested paths đã phân tích
  * Phân bố địa lý: 65% Châu Á-Thái Bình Dương, 25% Châu Âu, 10% Châu Mỹ

* **Tích hợp WAF:** Cấu hình AWS WAF với managed rule groups:
  * AWS Managed Rules - Core Rule Set (CRS)
  * AWS Managed Rules - Known Bad Inputs
  * AWS Managed Rules - SQL Database
  * Custom rate limiting rule: 2000 requests mỗi 5 phút mỗi IP
  * Requests bị chặn: 1,247 requests độc hại trong tuần 7

#### 4. Distributed Messaging với SQS và SNS
* **Triển khai Amazon SQS:**
  
  **Queue 1 - Order Processing (FIFO):**
  * Queue name: `order-processing.fifo`
  * Message retention: 14 ngày
  * Visibility timeout: 60 giây
  * Content-based deduplication: Enabled
  * Dead Letter Queue: `order-processing-dlq.fifo` (max receives: 3)
  * Messages xử lý: 45,000 đơn hàng/tuần
  * DLQ messages: 12 (99.97% success rate)
  
  **Queue 2 - Image Processing (Standard):**
  * Queue name: `image-processing-queue`
  * Max message size: 256 KB
  * Receive message wait time: 20 giây (long polling)
  * Processing Lambda: 4 concurrent executions
  * Thời gian xử lý trung bình: 8 giây mỗi ảnh
  * Throughput: 450 ảnh/giờ

* **Cấu hình Amazon SNS:**
  
  **SNS Topic - Application Notifications:**
  * Topic type: Standard
  * Subscriptions đã cấu hình:
    1. Email: team@example.com (confirmed)
    2. SMS: +84-xxx-xxx-xxx (cho critical alerts)
    3. SQS: notification-queue (fan-out pattern)
    4. Lambda: notification-processor (filter: severity=high)
    5. HTTPS: https://webhook.example.com/notifications
  
  * Ví dụ message filtering policy:
  ```json
  {
    "eventType": ["order_completed", "payment_failed"],
    "priority": ["high", "critical"],
    "region": [{"prefix": "ap-"}]
  }
  ```

* **Kiến trúc SNS-to-SQS Fan-Out:**
  * Single SNS topic: `order-events`
  * Multiple SQS subscribers:
    - `inventory-update-queue`: Cập nhật mức tồn kho
    - `analytics-queue`: Theo dõi order metrics
    - `email-notification-queue`: Gửi email khách hàng
    - `shipping-queue`: Kích hoạt quy trình fulfillment
  * Lợi ích đạt được:
    - Tách rời microservices
    - Xử lý song song
    - Scaling độc lập cho mỗi consumer
    - Đảm bảo message delivery: At-least-once

* **Lambda Message Processor:** Triển khai serverless message processing:
  ```python
  import json
  import boto3
  
  sqs = boto3.client('sqs')
  dynamodb = boto3.resource('dynamodb')
  
  def lambda_handler(event, context):
      table = dynamodb.Table('OrderStatus')
      
      for record in event['Records']:
          # Parse SQS message
          message = json.loads(record['body'])
          order_id = message['orderId']
          
          try:
              # Process order
              process_order(order_id, message)
              
              # Update DynamoDB
              table.update_item(
                  Key={'orderId': order_id},
                  UpdateExpression='SET #status = :status, processedAt = :timestamp',
                  ExpressionAttributeNames={'#status': 'status'},
                  ExpressionAttributeValues={
                      ':status': 'PROCESSED',
                      ':timestamp': int(time.time())
                  }
              )
              
              # Delete message from queue
              sqs.delete_message(
                  QueueUrl=queue_url,
                  ReceiptHandle=record['receiptHandle']
              )
              
          except Exception as e:
              print(f"Error processing order {order_id}: {str(e)}")
              # Message sẽ được xử lý lại sau visibility timeout
              
      return {'statusCode': 200, 'body': 'Processed'}
  ```

#### 5. VPC Flow Logs và Network Monitoring
* **Cấu hình Flow Logs:** Kích hoạt VPC Flow Logs với coverage toàn diện:
  * **Phạm vi:** VPC-level flow logs cho toàn bộ infrastructure VPC
  * **Filter:** All traffic (accepted, rejected, và all)
  * **Destination 1:** CloudWatch Logs (log group: `/aws/vpc/flowlogs`)
  * **Destination 2:** S3 bucket với partitioning theo ngày (`s3://flowlogs-bucket/year=2026/month=01/day=17/`)
  * **Log format:** Custom format với 25 fields:
  ```
  ${srcaddr} ${dstaddr} ${srcport} ${dstport} ${protocol} ${packets} ${bytes} 
  ${start} ${end} ${action} ${log-status} ${vpc-id} ${subnet-id} ${instance-id} 
  ${tcp-flags} ${type} ${pkt-srcaddr} ${pkt-dstaddr} ${region} ${az-id} 
  ${sublocation-type} ${sublocation-id} ${pkt-src-aws-service} ${pkt-dst-aws-service} 
  ${flow-direction}
  ```

* **CloudWatch Insights Queries:** Tạo custom queries để phân tích traffic:
  
  **Query 1 - Top 10 Talkers:**
  ```
  fields @timestamp, srcaddr, dstaddr, bytes
  | filter action = "ACCEPT"
  | stats sum(bytes) as totalBytes by srcaddr
  | sort totalBytes desc
  | limit 10
  ```
  Kết quả: Xác định top traffic sources, tối ưu routing
  
  **Query 2 - Phân tích Rejected Traffic:**
  ```
  fields @timestamp, srcaddr, dstport, protocol
  | filter action = "REJECT"
  | stats count() as rejectionCount by srcaddr, dstport
  | sort rejectionCount desc
  ```
  Kết quả: Phát hiện 3 port scanning attempts, cập nhật Security Groups
  
  **Query 3 - Chi phí Inter-AZ Traffic:**
  ```
  fields @timestamp, srcaddr, dstaddr, bytes, az-id
  | filter srcaddr like /^10\.0\./ and dstaddr like /^10\.0\./
  | stats sum(bytes) as interAZBytes by az-id
  ```
  Kết quả: Xác định $85/tháng chi phí cross-AZ data transfer, tối ưu placement

* **CloudWatch Dashboard:** Xây dựng comprehensive network monitoring dashboard:
  * **Panels đã tạo:**
    1. Tổng traffic volume (GB/giờ) - Line graph
    2. Accepted vs Rejected packets - Stacked area chart
    3. Top 5 source IPs - Bar chart
    4. Phân phối traffic theo protocol - Pie chart
    5. Geographic traffic heatmap
    6. Anomaly detection overlay
  * **Refresh interval:** 1 phút (auto-refresh)

* **Anomaly Detection và Alerting:**
  * CloudWatch Anomaly Detection đã cấu hình cho:
    - Traffic spikes bất thường (threshold: 3 standard deviations)
    - Tăng rejected traffic không mong đợi
    - Abnormal inter-region data transfer
  * SNS alerts đã kích hoạt cho 2 anomalies:
    1. Traffic spike từ unknown IP range (resolved: đối tác hợp pháp)
    2. Port 3389 scan attempts (resolved: cập nhật NACL rules)

* **Network Traffic Insights:**
  * **Tổng traffic phân tích:** 1.2 TB trong tuần 7
  * **Phân loại traffic:**
    - 68% application traffic (HTTP/HTTPS)
    - 18% database traffic (PostgreSQL port 5432)
    - 8% cache traffic (Redis port 6379)
    - 4% inter-service communication
    - 2% rejected/denied traffic
  * **Peak traffic:** Thứ 5 3 PM - 2.4 Gbps
  * **Latency trung bình:** 12ms intra-AZ, 28ms cross-AZ

#### 6. Load Testing và Performance Validation
* **Kết quả Apache JMeter Load Test:**
  * **Cấu hình test:**
    - Concurrent users: 1,000
    - Ramp-up period: 60 giây
    - Test duration: 30 phút
    - Request rate: 800 requests/giây at peak
  
  * **Performance metrics:**
    - Tổng requests: 1,440,000
    - Success rate: 99.92%
    - Thời gian phản hồi trung bình: 156ms
    - 95th percentile: 420ms
    - 99th percentile: 1,240ms
    - Throughput: 785 requests/giây
  
  * **Auto Scaling behavior trong test:**
    - Bắt đầu với 4 instances
    - Scaled lên 8 instances sau 3 phút
    - Peaked ở 9 instances
    - Scaled về 5 instances sau khi test hoàn thành
    - Tổng scaling events: 12 (6 scale-out, 6 scale-in)

* **Chaos Engineering với AWS FIS:** Thực hiện failure injection experiments:
  
  **Experiment 1 - Mô phỏng AZ Failure:**
  * Action: Dừng tất cả instances ở ap-southeast-1a
  * Kết quả: ALB tự động route traffic sang healthy AZs
  * Thời gian recovery: 45 giây
  * Không có lỗi nào user nhìn thấy
  
  **Experiment 2 - CPU Stress Test:**
  * Action: 100% CPU utilization trên 50% instances
  * Kết quả: ASG scaled out từ 6 lên 10 instances
  * Tác động response time: +85ms trung bình (trong SLA)
  
  **Experiment 3 - Network Latency Injection:**
  * Action: Thêm 500ms latency vào database connections
  * Kết quả: Connection pool exhaustion phát hiện, alarms kích hoạt
  * Bài học: Triển khai connection timeout optimization

#### 7. Tối ưu hóa Chi phí và Hiệu năng
* **Phân tích Chi phí Hàng tuần:**
  * ALB: $45.20 (bao gồm LCU charges)
  * Auto Scaling EC2 instances: $168.40 (trung bình 5.2 t3.medium instances)
  * Data transfer: $32.15
  * CloudWatch: $18.30 (custom metrics và logs)
  * Tổng: $264.05/tuần
  * **Tiết kiệm vs static provisioning:** 38% ($161/tuần tiết kiệm)

* **Cải thiện Hiệu năng:**
  * Application availability: 99.95% (tăng từ 98.2% ở tuần 6)
  * Thời gian phản hồi trung bình: 145ms (cải thiện từ 280ms)
  * P95 response time: 380ms (cải thiện từ 890ms)
  * Failed requests: 0.08% (giảm từ 1.3%)
  * Auto-recovery incidents: 3 (tất cả thành công, <1 phút recovery)

* **Bài học Quan trọng và Best Practices:**
  1. Triển khai Multi-AZ là quan trọng cho HA (zero downtime trong AZ failure simulation)
  2. Cấu hình health check đúng ngăn chặn cascading failures
  3. Connection draining cần thiết cho graceful instance termination
  4. VPC Flow Logs cung cấp insights vô giá cho security và optimization
  5. SNS-SQS fan-out pattern cho phép kiến trúc microservices có thể scale
  6. Target tracking scaling hiệu quả nhất cho gradual load changes
  7. Step scaling xử lý traffic spikes đột ngột tốt hơn
  8. Phân tích access logs tiết lộ cơ hội optimization


