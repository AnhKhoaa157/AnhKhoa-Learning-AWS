---
title: "Worklog Tuần 5"
date: 2025-10-06
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

* Thành thạo các nguyên tắc Infrastructure as Code (IaC) sử dụng AWS CloudFormation và AWS CDK.
* Triển khai provisioning hạ tầng theo khai báo với YAML/JSON templates.
* Khám phá triển khai hạ tầng lập trình sử dụng AWS CDK với TypeScript.
* Tập trung quản lý hệ thống và tự động hóa sử dụng AWS Systems Manager.
* Xây dựng pipeline triển khai hạ tầng có thể lặp lại, được kiểm soát phiên bản.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Giới thiệu khái niệm Infrastructure as Code (IaC) và lợi ích so với triển khai thủ công <br> - Làm quen với AWS CloudFormation: template, stack, parameter | 06/10/2025   | 06/10/2025      | AWS Journey |
| 3   | - Viết CloudFormation template để triển khai S3 bucket và EC2 instance <br> - Tạo, cập nhật và xóa stack qua AWS Console | 07/10/2025   | 07/10/2025      | AWS Journey |
| 4   | - Giới thiệu AWS CDK (Cloud Development Kit) <br> - Cài đặt AWS CDK, tạo CDK project sử dụng Python hoặc TypeScript <br> - Viết CDK code để triển khai EC2 instance | 08/10/2025   | 08/10/2025      | AWS Journey |
| 5   | - Giới thiệu AWS Systems Manager (SSM) và các tính năng chính: Parameter Store, Run Command, Automation, Session Manager <br> - Tạo Parameter Store để lưu trữ biến cấu hình | 09/10/2025   | 09/10/2025      | AWS Journey |
| 6   | - Thực hành tạo Automation Document trong SSM để tự động start/stop EC2 <br> - Test Session Manager (truy cập EC2 không cần SSH key) <br> - Tổng kết tuần: demo IaC + SSM | 10/10/2025   | 10/10/2025      | AWS Journey |


### Kết quả đạt được tuần 5:

* Thành thạo các nguyên tắc và lợi ích Infrastructure as Code:
  * Hiểu version control cho cấu hình hạ tầng
  * Học khả năng lặp lại và tính nhất quán qua các môi trường
  * Khám phá tự động hóa giảm lỗi con người và thời gian triển khai
  * Nghiên cứu hạ tầng tự tài liệu hóa thông qua code
  * Tìm hiểu drift detection và remediation strategies

* Đạt được chuyên môn trong AWS CloudFormation:
  * Hiểu vòng đời stack: create, update, delete operations
  * Học cấu trúc template: AWSTemplateFormatVersion, Description, Metadata, Parameters, Mappings, Conditions, Resources, Outputs
  * Thành thạo intrinsic functions:
    - `!Ref` - tham chiếu parameters và logical IDs
    - `!GetAtt` - lấy giá trị attribute
    - `!Sub` - thay thế chuỗi với biến
    - `!Join` - nối chuỗi với delimiter
    - `!ImportValue` - tham chiếu cross-stack
    - `!FindInMap` - tra cứu giá trị trong mapping
  * Khám phá pseudo parameters: AWS::Region, AWS::AccountId, AWS::StackName
  * Nghiên cứu change sets cho cập nhật hạ tầng an toàn
  * Học nested stacks cho tính modular của template
  * Tìm hiểu StackSets cho triển khai multi-account/region

* Xây dựng thành công CloudFormation templates cấp production:
  * **Network Stack:** Tạo VPC với CIDR 10.0.0.0/16
    - 3 public subnets trải rộng availability zones
    - 3 private subnets cho application tier
    - 3 database subnets với subnet groups
    - Internet Gateway cho public internet access
    - NAT Gateways trong mỗi AZ cho high availability
    - Route tables với associations phù hợp
    - Network ACLs cho lớp bảo mật bổ sung
  * **Security Stack:** Thiết kế security groups
    - ALB security group: HTTP/HTTPS từ 0.0.0.0/0
    - Web tier security group: traffic chỉ từ ALB
    - App tier security group: traffic từ web tier
    - Database security group: port 3306 từ app tier
    - Bastion host security group: SSH từ corporate IP
  * **Application Stack:** Triển khai auto-scaling web tier
    - Application Load Balancer với health checks
    - Launch Template với user data bootstrap scripts
    - Auto Scaling Group: min 2, desired 3, max 6 instances
    - Target Tracking scaling policy dựa trên CPU 60%
    - CloudWatch alarms để giám sát
  * **Database Stack:** Provisioned RDS MySQL
    - db.r6g.large instance class, Multi-AZ deployment
    - Automated backups với 7-day retention
    - Read replica cho reporting workloads
    - Enhanced monitoring enabled
  * Triển khai cross-stack references sử dụng Outputs và ImportValue
  * Kiểm tra rollback on failure scenarios
  * Xác thực change sets trước khi áp dụng updates

* Thành thạo AWS Cloud Development Kit (CDK):
  * Hiểu lợi ích CDK: type safety, IDE autocomplete, ngôn ngữ lập trình quen thuộc
  * Học ba cấp độ constructs:
    - L1 (CFN Resources): Low-level CloudFormation resources
    - L2 (Curated): Higher-level với sensible defaults
    - L3 (Patterns): Complete architecture patterns
  * Khám phá CDK CLI commands:
    - `cdk init` - khởi tạo project mới
    - `cdk synth` - synthesize CloudFormation template
    - `cdk diff` - hiển thị thay đổi trước deployment
    - `cdk deploy` - deploy stacks lên AWS
    - `cdk destroy` - tear down resources
  * Nghiên cứu cấu trúc CDK project: bin/, lib/, cdk.json, package.json

* Phát triển thành công hạ tầng sử dụng CDK với TypeScript:
  * Khởi tạo CDK project với TypeScript template
  * Cài đặt AWS construct libraries qua npm
  * Tạo VPC sử dụng L2 construct với automatic subnet allocation:
    ```typescript
    const vpc = new ec2.Vpc(this, 'MyVpc', {
      maxAzs: 3,
      natGateways: 1,
      subnetConfiguration: [
        { name: 'Public', subnetType: ec2.SubnetType.PUBLIC },
        { name: 'Private', subnetType: ec2.SubnetType.PRIVATE_WITH_EGRESS },
        { name: 'Database', subnetType: ec2.SubnetType.PRIVATE_ISOLATED }
      ]
    });
    ```
  * Triển khai S3 bucket với cấu hình nâng cao:
    - Versioning enabled cho data protection
    - AES-256 server-side encryption
    - Lifecycle rules: chuyển sang Glacier sau 90 ngày
    - Block public access settings
    - Bucket policies cho least privilege
  * Tạo Aurora Serverless v2 cluster:
    - PostgreSQL engine version 14.6
    - Auto-scaling từ 0.5 đến 4 ACU
    - Multi-AZ cho high availability
    - Automated backups và snapshots
    - Secrets Manager integration cho credentials
  * Synthesized CloudFormation templates thành công
  * Triển khai stacks với confirmation prompts cho security
  * Quan sát khả năng CDK giảm boilerplate code 70%

* Thành thạo các khả năng AWS Systems Manager:
  * **Parameter Store:** Quản lý cấu hình tập trung
    - Tổ chức parameter phân cấp: /app/dev/, /app/prod/
    - Các loại String, StringList, SecureString
    - Mã hóa KMS cho dữ liệu nhạy cảm
    - Version tracking cho parameter changes
    - Cross-region parameter replication
  * **Session Manager:** Truy cập instance an toàn
    - Browser-based shell không cần SSH keys
    - Session logging vào S3 và CloudWatch
    - Kiểm soát truy cập dựa trên IAM
    - Không cần inbound ports
    - Audit trail cho compliance
  * **Run Command:** Thực thi script từ xa
    - Thực thi AWS-managed hoặc custom documents
    - Target instances theo tags, instance IDs, resource groups
    - Rate control và concurrency limits
    - Output captured trong S3 và CloudWatch
    - Event-driven automation với EventBridge
  * **Automation Documents:** Workflow orchestration
    - Multi-step automation runbooks
    - Conditional branching và error handling
    - Tích hợp với Lambda, Step Functions
    - Scheduled execution qua Maintenance Windows
  * **State Manager:** Configuration compliance
    - Apply và enforce desired state
    - Association targeting theo tags
    - Scheduled association execution
    - Compliance reporting và dashboards
  * **Patch Manager:** Automated patching
    - Patch baselines cho các loại OS khác nhau
    - Maintenance windows cho controlled updates
    - Compliance reporting cho security audits
  * **Inventory:** Resource data collection
    - Track installed applications, network configs
    - Custom inventory cho business metadata
    - Tích hợp với Config và Security Hub

* Triển khai thành công các giải pháp Systems Manager:
  * Cấu hình Parameter Store với cấu trúc phân cấp:
    - /myapp/database/connection-string (SecureString)
    - /myapp/api/keys/third-party (SecureString với KMS)
    - /myapp/config/feature-flags (String)
    - Application đọc parameters tại runtime
  * Tạo Automation Document cho tối ưu chi phí:
    - Scheduled Lambda-backed automation
    - Stop non-production EC2 instances lúc 6 PM
    - Start instances lúc 8 AM vào ngày trong tuần
    - Tag-based filtering: Environment=Dev
    - CloudWatch Events trigger automation
    - Ước tính tiết kiệm 60% chi phí trên dev environments
  * Triển khai Session Manager cho truy cập an toàn:
    - Cấu hình SSM Agent trên tất cả EC2 instances
    - Tạo IAM policies cho session permissions
    - Bật session logging vào S3 bucket
    - Kiểm tra browser-based shell access
    - Xóa SSH key pairs khỏi instances
    - Tăng cường bảo mật loại bỏ port 22 exposure
  * Sử dụng Run Command cho fleet management:
    - Thực thi AWS-UpdateSSMAgent trên tất cả instances
    - Chạy custom shell scripts cho log rotation
    - Cài đặt CloudWatch agent fleet-wide
    - Target instances sử dụng tag: Application=WebServer
    - Parallel execution với concurrency limit của 10
  * Cấu hình State Manager associations:
    - Đảm bảo CloudWatch agent running và configured
    - Áp dụng security hardening scripts
    - Duy trì desired package versions
    - Compliance rate tracked: 98% trên fleet
  * Xây dựng Inventory collection:
    - Thu thập installed applications trên instances
    - Collected network configuration metadata
    - Tạo custom inventory cho licensing compliance
    - Queried inventory data với Resource Data Sync

* Tích hợp IaC với Systems Manager:
  * CloudFormation templates triển khai SSM-enabled instances
  * CDK stacks cấu hình Parameter Store values
  * Automation Documents triggered bởi CloudFormation custom resources
  * Infrastructure state tracked thông qua SSM Inventory
  * Đạt được vòng đời hạ tầng tự động, có thể audit hoàn toàn

* Phát triển operational dashboards và monitoring:
  * CloudWatch dashboard hiển thị stack deployment metrics
  * SSM compliance dashboard showing configuration drift
  * CloudTrail integration cho IaC change auditing
  * SNS notifications cho automation failures
  * Centralized logging cho troubleshooting

* Đạt được mức độ trưởng thành infrastructure automation:
  * Giảm thời gian deployment từ hours xuống minutes
  * Loại bỏ lỗi cấu hình thủ công
  * Cho phép self-service infrastructure provisioning
  * Thiết lập version-controlled infrastructure repository
  * Triển khai approval workflows cho production changes
  * Đạt 100% infrastructure reproducibility

* Chuẩn bị nền tảng cho Tuần 6: Security governance với AWS Security Hub, IAM Access Analyzer, tối ưu chi phí với Cost Explorer, Budgets và Trusted Advisor.


