---
title: "Worklog Tuần 11"
date: 2025-11-17
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}


### Mục tiêu tuần 11:

* Tìm hiểu về các khái niệm Modernization và Serverless.
* Hiểu về kiến trúc monolithic và microservices.
* Thực hành xây dựng ứng dụng serverless với Lambda, API Gateway và các dịch vụ AWS khác.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Giới thiệu về các khái niệm Modernization và Serverless <br> - So sánh kiến trúc monolithic và microservices <br> - Phân tích lợi ích của việc chuyển đổi sang serverless                                                                                             | 17/11/2025   | 17/11/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Thực hành AWS Lambda: tạo function, cấu hình trigger, xem log trong CloudWatch <br> - Triển khai logic xử lý API cơ bản sử dụng Lambda                                                           | 18/11/2025   | 18/11/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tích hợp API Gateway với Lambda để tạo REST API <br> - Kết nối dữ liệu với DynamoDB (các thao tác CRUD) <br> - Kiểm tra API bằng Postman                                                           | 19/11/2025   | 19/11/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Cấu hình Cognito để xác thực người dùng (user pool, token) <br> - Tích hợp xác thực Cognito vào API Gateway <br> - Quản lý quyền truy cập thông qua IAM Role                                                           | 20/11/2025   | 20/11/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - Thực hành triển khai ứng dụng Serverless hoàn chỉnh bằng AWS SAM (Serverless Application Model) <br> - Testing, logging và tối ưu hóa hiệu suất <br> - Tổng hợp kiến thức và báo cáo tuần                                                           | 21/11/2025   | 21/11/2025      | <https://cloudjourney.awsstudygroup.com/> |



### Kết quả đạt được tuần 11:

* Hiểu về Kiến trúc Serverless:
  * Tìm hiểu các khái niệm và lợi ích của serverless
  * So sánh kiến trúc monolithic và microservices
  * Nghiên cứu ưu điểm của serverless:
    * Không cần quản lý server
    * Tự động mở rộng quy mô
    * Tính phí theo mức sử dụng
    * Thời gian đưa sản phẩm ra thị trường nhanh hơn

* Thực hành AWS Lambda:
  * Tạo Lambda function sử dụng Python
  * Cấu hình trigger cho function (API Gateway, S3, CloudWatch Events)
  * Xem execution log trong CloudWatch
  * Thiết lập biến môi trường và timeout
  * Triển khai logic xử lý API cơ bản

* Xây dựng REST API với API Gateway và Lambda:
  * Tạo REST API sử dụng API Gateway
  * Tích hợp Lambda function làm backend
  * Thực hiện các thao tác CRUD với DynamoDB
  * Kiểm tra API endpoint bằng Postman
  * Cấu hình request/response mapping

* Triển khai Xác thực với Cognito:
  * Tạo Cognito User Pool
  * Cấu hình luồng xác thực người dùng
  * Tích hợp Cognito với API Gateway
  * Sử dụng JWT token để ủy quyền
  * Quản lý IAM role cho kiểm soát truy cập

* Triển khai Ứng dụng Serverless:
  * Tìm hiểu cơ bản về AWS SAM framework
  * Tạo SAM template (template.yaml)
  * Triển khai ứng dụng serverless hoàn chỉnh bằng SAM CLI
  * Kiểm tra chức năng ứng dụng
  * Xem lại CloudWatch log để giám sát


