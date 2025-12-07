---
title: "Worklog Tuần 6"
date: 2025-10-13
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Làm chủ các khái niệm bảo mật nâng cao của AWS IAM
* Triển khai mã hóa dữ liệu với AWS KMS và quản lý khóa
* Tự động hóa việc quản lý thông tin nhạy cảm với AWS Secrets Manager
* Áp dụng các thực hành tốt nhất về FinOps và tối ưu hóa chi phí AWS
* Thiết lập giám sát chi phí chủ động và cơ chế cảnh báo
* Phân tích và tối ưu hóa chi phí thực tế của tài nguyên AWS

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Ôn tập kiến thức IAM cơ bản <br> - Tìm hiểu IAM Policy nâng cao (cấu trúc JSON, Effect, Action, Resource, Condition) <br> - Tạo custom policies và gắn vào users/groups | 13/10/2025   | 13/10/2025      | AWS Journey |
| 3   | - Giới thiệu AWS Key Management Service (KMS) <br> - Tạo Customer Managed Key (CMK) và test mã hóa/giải mã file <br> - Áp dụng KMS để mã hóa S3 bucket hoặc EBS volume | 14/10/2025   | 14/10/2025      | AWS Journey |
| 4   | - Làm quen với AWS Secrets Manager <br> - Tạo secret để lưu thông tin kết nối Database <br> - Viết Lambda script nhỏ để đọc secret từ Secrets Manager | 15/10/2025   | 15/10/2025      | AWS Journey |
| 5   | - Khám phá AWS Billing Dashboard và Cost Explorer <br> - Xem chi phí theo service, region và thời gian <br> - Thiết lập Cost Anomaly Detection | 16/10/2025   | 16/10/2025      | AWS Journey |
| 6   | - Tạo AWS Budget và cấu hình cảnh báo chi phí qua email <br> - Viết báo cáo tóm tắt chi phí tuần với đề xuất tối ưu (dừng EC2, dọn dẹp EBS, giảm log retention, v.v.) <br> - Tổng kết kiến thức Tuần 6 | 17/10/2025   | 17/10/2025      | AWS Journey |


### Kết quả đạt được tuần 6:

#### 1. Bảo mật nâng cao với IAM
* **Permission Boundaries:** Đã triển khai permission boundaries thành công để giới hạn quyền tối đa mà IAM role có thể có:
  ```json
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [
          "s3:*",
          "dynamodb:*",
          "lambda:*"
        ],
        "Resource": "*",
        "Condition": {
          "StringEquals": {
            "aws:RequestedRegion": "ap-southeast-1"
          }
        }
      }
    ]
  }
  ```

* **Attribute-Based Access Control (ABAC):** Triển khai ABAC sử dụng resource tags để kiểm soát truy cập động:
  * Tạo IAM policy với điều kiện dựa trên tags
  * Tag resources theo Department, Environment, Project
  * Cho phép developers chỉ truy cập resources có tag phù hợp
  * Ví dụ policy:
  ```json
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": "ec2:*",
        "Resource": "*",
        "Condition": {
          "StringEquals": {
            "ec2:ResourceTag/Department": "${aws:PrincipalTag/Department}",
            "ec2:ResourceTag/Environment": "${aws:PrincipalTag/Environment}"
          }
        }
      }
    ]
  }
  ```

* **IAM Access Analyzer:** Cấu hình IAM Access Analyzer để phát hiện và khắc phục các vấn đề bảo mật:
  * Quét tất cả resources trong account để tìm external access
  * Phát hiện 3 S3 buckets có public access không mong muốn
  * Tạo findings report và remediation plan
  * Thiết lập automated notifications qua EventBridge + SNS

#### 2. Mã hóa dữ liệu với AWS KMS
* **Customer Managed Keys (CMK):** Tạo và quản lý CMK cho các use cases khác nhau:
  * CMK cho S3 bucket encryption (key alias: `alias/s3-encryption-key`)
  * CMK cho RDS encryption (key alias: `alias/rds-encryption-key`)
  * CMK cho EBS volume encryption (key alias: `alias/ebs-encryption-key`)

* **Automatic Key Rotation:** Kích hoạt automatic rotation cho tất cả CMKs:
  * Rotation period: 365 ngày
  * AWS tự động quản lý old key material cho decrypt operations
  * Không cần update applications hay re-encrypt data

* **Envelope Encryption:** Triển khai envelope encryption pattern cho large files:
  * Sử dụng Data Encryption Key (DEK) để mã hóa data
  * Sử dụng CMK để mã hóa DEK
  * Giảm thiểu số lần gọi KMS API (cost optimization)
  * Performance: Giảm 75% KMS API calls so với direct encryption

* **S3 Bucket Encryption với SSE-KMS:** Cấu hình default encryption cho S3 buckets:
  * Bucket policy yêu cầu bắt buộc encryption cho mọi uploads
  * CloudTrail logging cho tất cả KMS key usage
  * Cross-region replication với encrypted objects

#### 3. Quản lý Secrets với AWS Secrets Manager
* **Database Credentials Management:** Tạo và quản lý secrets cho database credentials:
  * RDS PostgreSQL credentials với automatic rotation
  * Rotation period: 30 ngày
  * Zero-downtime rotation với multi-user rotation strategy

* **Automatic Rotation:** Thiết lập Lambda-based automatic rotation:
  * Tạo rotation Lambda function (Python 3.11)
  * IAM role với permissions cần thiết
  * CloudWatch Logs để monitor rotation status
  * Ví dụ Lambda code:
  ```python
  import boto3
  import json

  def lambda_handler(event, context):
      service_client = boto3.client('secretsmanager')
      arn = event['SecretId']
      token = event['ClientRequestToken']
      step = event['Step']
      
      if step == "createSecret":
          # Tạo new credentials
          create_secret(service_client, arn, token)
      elif step == "setSecret":
          # Set new credentials in database
          set_secret(service_client, arn, token)
      elif step == "testSecret":
          # Test new credentials
          test_secret(service_client, arn, token)
      elif step == "finishSecret":
          # Mark rotation complete
          finish_secret(service_client, arn, token)
  ```

* **Lambda Integration:** Tích hợp Secrets Manager vào Lambda functions:
  * Sử dụng AWS SDK để retrieve secrets at runtime
  * Caching secrets với environment variables
  * Monitoring secret access với CloudTrail
  * Reduced cold start time: Cache secrets for 5 minutes

* **Security Best Practices:**
  * Enabled encryption at rest với KMS CMK
  * Resource-based policies để restrict access
  * VPC endpoints cho private connectivity
  * Audit logging với AWS CloudTrail

#### 4. FinOps và Cost Optimization
* **Cost Allocation Tags:** Triển khai comprehensive tagging strategy:
  * Mandatory tags: Environment, Department, Project, Owner, CostCenter
  * Activated cost allocation tags trong Billing console
  * 95% resources đã được tag đúng chuẩn
  * Monthly cost reports grouped by tags

* **Cost Anomaly Detection:** Cấu hình anomaly detection để phát hiện chi phí bất thường:
  * Created 3 monitors:
    - Service-level monitor (phát hiện anomalies theo từng AWS service)
    - Linked account monitor (cho multi-account setup)
    - Cost category monitor (theo business units)
  * Alert threshold: 10% deviation từ expected spending
  * Notification via SNS + Email
  * Đã phát hiện 2 anomalies trong tuần:
    - EC2 instance type upgrade không mong muốn
    - S3 data transfer spike do misconfigured CloudFront

* **Custom Cost Reports:** Tạo các custom reports chi tiết:
  * Daily cost and usage report
  * Monthly cost breakdown by service
  * Quarter-over-quarter growth analysis
  * Reserved Instance utilization report (current: 87%)

#### 5. AWS Budgets và Savings Plans
* **Multi-Threshold Budgets:** Tạo budgets với multiple alert thresholds:
  * Monthly budget: $1,500
  * Alert thresholds:
    - 80% ($1,200): Email notification to team
    - 90% ($1,350): Email + Slack notification
    - 100% ($1,500): Email + Slack + Manager escalation
  * Forecasted budget alerts enabled
  * Budget actions: Auto-stop development instances khi vượt 100%

* **Savings Plans Analysis:** Phân tích và recommend Savings Plans:
  * Analyzed last 30 days usage patterns
  * Compute Savings Plan recommendation:
    - Commit: $8/hour (1-year, no upfront)
    - Estimated savings: 25% ($320/month)
  * EC2 Instance Savings Plan recommendation:
    - Commit: $5/hour for ap-southeast-1 region
    - Estimated savings: 18% ($180/month)

* **Budget Actions với SNS:** Thiết lập automated actions:
  * SNS topic: `billing-alerts-topic`
  * Subscribers: Team email, Slack webhook, PagerDuty
  * Custom Lambda function để parse budget alerts và format messages

#### 6. Cost Optimization Report
* **Right-Sizing Recommendations:** Analyzed và implemented right-sizing:
  * **EC2 Instances:**
    - 3x t3.large → t3.medium (development environments)
    - Savings: $125/month (40% reduction)
  * **RDS Instances:**
    - 1x db.t3.large → db.t3.medium (staging database)
    - Savings: $78/month (35% reduction)

* **S3 Storage Optimization:**
  * Implemented S3 Intelligent-Tiering cho data buckets
  * Created lifecycle policies:
    - Archive to Glacier after 90 days
    - Delete after 365 days (for log files)
  * Estimated savings: $95/month (42% reduction in storage costs)

* **Reserved Instance Purchases:** Phân tích và mua Reserved Instances:
  * Purchased 2x t3.medium RIs (1-year, partial upfront)
  * Current RI coverage: 68% → Target: 80%
  * Estimated annual savings: $1,050

* **Cost Optimization Hub:** Sử dụng Cost Optimization Hub để identify savings:
  * Total recommendations: 18
  * Estimated monthly savings: $385 (34% reduction)
  * Priority actions:
    1. Delete unused EBS volumes (7 volumes, $87/month)
    2. Release idle Elastic IPs (4 IPs, $14.6/month)
    3. Right-size underutilized EC2 instances ($125/month)
    4. Enable S3 Intelligent-Tiering ($95/month)
    5. Purchase Reserved Instances for predictable workloads ($63/month)

* **Monitoring và Reporting:**
  * Daily cost dashboard trong CloudWatch
  * Weekly cost review meetings
  * Monthly FinOps report to management
  * Automated cost optimization recommendations qua email

#### 7. Key Metrics và Achievements
* **Security Metrics:**
  * IAM Access Analyzer findings: 3 resolved
  * Secrets rotation success rate: 100%
  * KMS key rotation enabled: 100%
  * Resources encrypted: 98%

* **Cost Metrics:**
  * Total monthly cost: $1,135 (giảm từ $1,520)
  * Cost reduction: 25.3% ($385/month)
  * Tagged resources: 95%
  * Budget alerts configured: 3 thresholds
  * RI coverage: 68%
  * Savings Plans commitment: $8/hour

* **Operational Excellence:**
  * Zero security incidents
  * Automated secret rotation: 100% success
  * Cost anomaly detection: 2 caught và resolved
  * FinOps maturity level: Intermediate (target: Advanced trong Q2)


