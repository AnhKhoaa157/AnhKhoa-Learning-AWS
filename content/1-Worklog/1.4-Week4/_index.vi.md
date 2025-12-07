---
title: "Worklog Tuần 4"
date: 2025-09-29
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

* Thành thạo các chiến lược di chuyển lên cloud và hệ sinh thái dịch vụ migration của AWS.
* Triển khai quy trình migration cơ sở dữ liệu sử dụng AWS Database Migration Service.
* Thiết kế và triển khai kiến trúc disaster recovery toàn diện.
* Phát triển kế hoạch liên tục kinh doanh với giải pháp backup và recovery tự động.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu các khái niệm Migration (Lift & Shift, Replatform, Refactor) <br> - Giới thiệu AWS Database Migration Service (DMS) | 29/09/2025   | 29/09/2025      | AWS Journey |
| 3   | - Thực hành tạo Replication Instance trong DMS <br> - Cấu hình source data (on-premise) và target (RDS) <br> - Thực hiện test migration dữ liệu | 30/09/2025   | 30/09/2025      | AWS Journey |
| 4   | - Giới thiệu Elastic Disaster Recovery (EDR) <br> - Tìm hiểu cách thiết lập replication server và recovery instance | 01/10/2025   | 01/10/2025      | AWS Journey |
| 5   | - Thực hành mô phỏng sự cố: tắt EC2 chính và khởi động recovery instance từ EDR <br> - Đánh giá thời gian phục hồi (RTO/RPO) | 02/10/2025   | 02/10/2025      | AWS Journey |
| 6   | - Tạo kế hoạch Disaster Recovery cơ bản (backup, restore, failover) <br> - Viết tài liệu tổng kết quy trình Migration + DR <br> - Tổng kết kiến thức Tuần 4 | 03/10/2025   | 03/10/2025      | AWS Journey |


### Kết quả đạt được tuần 4:

* Thành thạo AWS cloud migration framework và phương pháp luận:
  * Hiểu 7 chiến lược migration 7R: Retire, Retain, Rehost, Relocate, Repurchase, Replatform, Refactor
  * Học khi nào áp dụng từng chiến lược dựa trên yêu cầu kinh doanh
  * Khám phá AWS Migration Hub cho theo dõi migration tập trung
  * Nghiên cứu Application Discovery Service cho dependency mapping
  * Tìm hiểu Migration Evaluator cho phân tích chi phí và tính toán TCO
  * Hiểu Server Migration Service (SMS) cho lift-and-shift migrations

* Có kiến thức toàn diện về AWS Database Migration Service (DMS):
  * Hiểu kiến trúc DMS: replication instances, endpoints, tasks
  * Học sizing replication instance dựa trên yêu cầu workload
  * Nghiên cứu homogeneous migrations (MySQL → RDS MySQL)
  * Khám phá heterogeneous migrations (MySQL → Aurora PostgreSQL, Oracle → PostgreSQL)
  * Tìm hiểu Schema Conversion Tool (SCT) cho schema transformation
  * Hiểu Change Data Capture (CDC) cho continuous replication
  * Học về migration task settings: full load, CDC, full load + CDC

* Thực hiện thành công dự án migration cơ sở dữ liệu phức tạp:
  * Thiết lập source MySQL 8.0 database trên EC2 với 50GB sample e-commerce data
  * Tạo DMS replication instance (dms.t3.medium, Multi-AZ cho độ tin cậy)
  * Cấu hình source endpoint với SSL/TLS encryption được bật
  * Tạo target endpoint cho Aurora PostgreSQL 14 cluster
  * Thiết kế migration task kết hợp full load và ongoing CDC
  * Giám sát migration metrics: tables loaded, CDC latency, replication lag
  * Xác thực tính toàn vẹn dữ liệu sử dụng row counts và checksum comparisons
  * Thực hiện cutover: dừng writes, đồng bộ final changes, chuyển application
  * Đạt zero-downtime migration với < 5 phút tạm dừng ứng dụng

* Thành thạo nguyên tắc disaster recovery và giải pháp AWS:
  * Hiểu Recovery Time Objective (RTO): thời gian downtime tối đa chấp nhận được
  * Học Recovery Point Objective (RPO): mất dữ liệu tối đa chấp nhận được
  * Nghiên cứu các mẫu kiến trúc DR:
    - Backup & Restore (RPO/RTO cao nhất, chi phí thấp nhất)
    - Pilot Light (tài nguyên chạy tối thiểu, RTO trung bình)
    - Warm Standby (bản sao thu nhỏ, RTO thấp)
    - Multi-Site Active-Active (RTO/RPO thấp nhất, chi phí cao nhất)
  * Khám phá AWS Backup quản lý backup tập trung
  * Tìm hiểu AWS Elastic Disaster Recovery (trước đây là CloudEndure)
  * Học cross-region replication cho redundancy địa lý

* Triển khai thành công giải pháp AWS Elastic Disaster Recovery:
  * Cài đặt EDR replication agent trên production EC2 instances (web, app, database tiers)
  * Cấu hình replication settings: target region (us-west-2), staging area subnet
  * Định nghĩa recovery point objective: 15-minute RPO với continuous data replication
  * Tạo recovery launch templates với instance types phù hợp
  * Thiết lập recovery network configuration: VPC, subnets, security groups
  * Thực thi DR drill simulation:
    - Khởi động recovery trong môi trường cô lập
    - Khởi chạy recovery instances từ recovery point mới nhất
    - Xác thực chức năng ứng dụng và tính nhất quán dữ liệu
    - Đo actual RTO: 12 phút từ khởi động đến recovery hoàn toàn
  * Tài liệu hóa failback procedures cho việc quay lại primary region

* Thiết kế và triển khai chiến lược backup toàn diện:
  * Tạo AWS Backup vault với mã hóa AES-256
  * Cấu hình cross-region backup copy đến secondary region cho bảo vệ địa lý
  * Triển khai backup plans cho tài nguyên quan trọng:
    - EC2 instances: daily snapshots, lưu trữ 30 ngày
    - RDS databases: automated backups mỗi 6 giờ, lưu trữ 7 ngày
    - DynamoDB tables: continuous backups với 35-day point-in-time recovery
    - EBS volumes: hourly snapshots cho critical data volumes
  * Thiết lập backup lifecycle policies cho tối ưu chi phí
  * Cấu hình backup notifications qua SNS để giám sát
  * Kiểm tra restore procedures để xác thực backup integrity

* Phát triển tài liệu và quy trình disaster recovery:
  * Tạo runbook chi tiết cho các tình huống kích hoạt DR
  * Tài liệu hóa quy trình failover từng bước với decision trees
  * Thiết kế recovery workflow diagrams sử dụng AWS architecture icons
  * Chuẩn bị communication templates cho incident response
  * Thiết lập recovery priority matrix cho applications
  * Tài liệu hóa rollback procedures và failback strategies

* Thực hiện testing và validation toàn diện:
  * Thực thi tabletop DR exercise với các thành viên team
  * Tiến hành technical failover drill và đo metrics
  * Xác thực backup restore times trên các loại resource khác nhau
  * Kiểm tra cross-region failover scenarios
  * Tài liệu hóa gaps và cơ hội cải thiện

* Đạt được mục tiêu business continuity:
  * RTO giảm từ 4 giờ xuống 15 phút (cải thiện 93%)
  * RPO giảm từ 24 giờ xuống 15 phút (cải thiện 99%)
  * Tự động hóa 95% quy trình recovery
  * Thiết lập continuous replication cho hệ thống quan trọng
  * Triển khai kiến trúc resilience đa region

* Chuẩn bị nền tảng cho Tuần 5: Infrastructure as Code với CloudFormation, Terraform và automation với Systems Manager và AWS Config.


