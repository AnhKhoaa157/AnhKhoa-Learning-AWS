------

title: "Bản đề xuất"title: "Bản đề xuất"

date: "2025-11-11T02:45:23Z"date: "2025-11-11T02:45:23Z"

weight: 2weight: 2

chapter: falsechapter: false

pre: " <b> 2. </b> "pre: " <b> 2. </b> "

------

{{% notice warning %}}{{% notice warning %}}

⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.

{{% /notice %}}{{% /notice %}}



# Nền tảng Devteria Game StoreTại phần này, bạn cần tóm tắt các nội dung trong workshop mà bạn **dự tính** sẽ làm.

## Giải pháp E-commerce trên AWS Cloud cho phân phối game số

# IoT Weather Platform for Lab Research  

### 1. Tóm tắt điều hành## Giải pháp AWS Serverless hợp nhất cho giám sát thời tiết thời gian thực  

Devteria Game Store là nền tảng thương mại điện tử có khả năng mở rộng dành cho việc mua và quản lý bản quyền game kỹ thuật số. Được xây dựng trên cơ sở hạ tầng AWS cloud, nền tảng cung cấp xác thực bảo mật, quản lý tồn kho thời gian thực, xử lý đơn hàng tự động và phân phối nội dung toàn cầu. Hệ thống hỗ trợ hàng nghìn người dùng đồng thời trong khi duy trì độ sẵn sàng cao (99.9% uptime) và hiệu quả chi phí thông qua kiến trúc serverless và khả năng tự động mở rộng.

### 1. Tóm tắt điều hành  

### 2. Tuyên bố vấn đềIoT Weather Platform được thiết kế dành cho nhóm *ITea Lab* tại TP. Hồ Chí Minh nhằm nâng cao khả năng thu thập và phân tích dữ liệu thời tiết. Nền tảng hỗ trợ tối đa 5 trạm thời tiết, có khả năng mở rộng lên 10–15 trạm, sử dụng thiết bị biên Raspberry Pi kết hợp cảm biến ESP32 để truyền dữ liệu qua MQTT. Nền tảng tận dụng các dịch vụ AWS Serverless để cung cấp giám sát thời gian thực, phân tích dự đoán và tiết kiệm chi phí, với quyền truy cập giới hạn cho 5 thành viên phòng lab thông qua Amazon Cognito.  



#### Thách thức hiện tại### 2. Tuyên bố vấn đề  

- Các cửa hàng game truyền thống gặp khó khăn với lượng truy cập tăng đột biến khi ra mắt game mới*Vấn đề hiện tại*  

- Quy trình xác thực và thanh toán phức tạp giảm tỷ lệ chuyển đổiCác trạm thời tiết hiện tại yêu cầu thu thập dữ liệu thủ công, khó quản lý khi có nhiều trạm. Không có hệ thống tập trung cho dữ liệu hoặc phân tích thời gian thực, và các nền tảng bên thứ ba thường tốn kém và quá phức tạp.  

- Quản lý tồn kho thủ công dẫn đến bán vượt số lượng

- Thiếu phân tích thời gian thực để đưa ra quyết định kinh doanh*Giải pháp*  

- Chi phí cơ sở hạ tầng cao cho công suất đỉnhNền tảng sử dụng AWS IoT Core để tiếp nhận dữ liệu MQTT, AWS Lambda và API Gateway để xử lý, Amazon S3 để lưu trữ (bao gồm data lake), và AWS Glue Crawlers cùng các tác vụ ETL để trích xuất, chuyển đổi, tải dữ liệu từ S3 data lake sang một S3 bucket khác để phân tích. AWS Amplify với Next.js cung cấp giao diện web, và Amazon Cognito đảm bảo quyền truy cập an toàn. Tương tự như Thingsboard và CoreIoT, người dùng có thể đăng ký thiết bị mới và quản lý kết nối, nhưng nền tảng này hoạt động ở quy mô nhỏ hơn và phục vụ mục đích sử dụng nội bộ. Các tính năng chính bao gồm bảng điều khiển thời gian thực, phân tích xu hướng và chi phí vận hành thấp.  



#### Giải pháp của chúng tôi*Lợi ích và hoàn vốn đầu tư (ROI)*  

Devteria tận dụng các dịch vụ quản lý của AWS để tạo nền tảng hiện đại, có khả năng mở rộng:Giải pháp tạo nền tảng cơ bản để các thành viên phòng lab phát triển một nền tảng IoT lớn hơn, đồng thời cung cấp nguồn dữ liệu cho những người nghiên cứu AI phục vụ huấn luyện mô hình hoặc phân tích. Nền tảng giảm bớt báo cáo thủ công cho từng trạm thông qua hệ thống tập trung, đơn giản hóa quản lý và bảo trì, đồng thời cải thiện độ tin cậy dữ liệu. Chi phí hàng tháng ước tính 0,66 USD (theo AWS Pricing Calculator), tổng cộng 7,92 USD cho 12 tháng. Tất cả thiết bị IoT đã được trang bị từ hệ thống trạm thời tiết hiện tại, không phát sinh chi phí phát triển thêm. Thời gian hoàn vốn 6–12 tháng nhờ tiết kiệm đáng kể thời gian thao tác thủ công.  

- **CloudFront + S3**: Phân phối nội dung nhanh toàn cầu

- **Cognito**: Xác thực người dùng an toàn với đăng nhập mạng xã hội### 3. Kiến trúc giải pháp  

- **API Gateway + Lambda**: Backend serverless tự động mở rộngNền tảng áp dụng kiến trúc AWS Serverless để quản lý dữ liệu từ 5 trạm dựa trên Raspberry Pi, có thể mở rộng lên 15 trạm. Dữ liệu được tiếp nhận qua AWS IoT Core, lưu trữ trong S3 data lake và xử lý bởi AWS Glue Crawlers và ETL jobs để chuyển đổi và tải vào một S3 bucket khác cho mục đích phân tích. Lambda và API Gateway xử lý bổ sung, trong khi Amplify với Next.js cung cấp bảng điều khiển được bảo mật bởi Cognito.  

- **RDS + S3**: Lưu trữ dữ liệu đáng tin cậy cho giao dịch và tài sản

- **SQS + SNS**: Xử lý đơn hàng bất đồng bộ và thông báo![IoT Weather Station Architecture](/images/2-Proposal/edge_architecture.jpeg)

- **CodePipeline**: CI/CD tự động cho triển khai nhanh

![IoT Weather Platform Architecture](/images/2-Proposal/platform_architecture.jpeg)

### 3. Kiến trúc giải pháp

*Dịch vụ AWS sử dụng*  

![Kiến trúc Devteria](../images/2-Proposal/devteria-arch.png)- *AWS IoT Core*: Tiếp nhận dữ liệu MQTT từ 5 trạm, mở rộng lên 15.  

- *AWS Lambda*: Xử lý dữ liệu và kích hoạt Glue jobs (2 hàm).  

#### Các thành phần chính- *Amazon API Gateway*: Giao tiếp với ứng dụng web.  

- *Amazon S3*: Lưu trữ dữ liệu thô (data lake) và dữ liệu đã xử lý (2 bucket).  

**Tầng Frontend**- *AWS Glue*: Crawlers lập chỉ mục dữ liệu, ETL jobs chuyển đổi và tải dữ liệu.  

- CloudFront CDN phân phối ứng dụng React tĩnh từ S3- *AWS Amplify*: Lưu trữ giao diện web Next.js.  

- Bộ nhớ đệm biên toàn cầu để tải trang dưới 2 giây- *Amazon Cognito*: Quản lý quyền truy cập cho người dùng phòng lab.  

- Mã hóa HTTPS và bảo vệ DDoS

*Thiết kế thành phần*  

**Tầng Backend**- *Thiết bị biên*: Raspberry Pi thu thập và lọc dữ liệu cảm biến, gửi tới IoT Core.  

- API Gateway quản lý điểm cuối RESTful- *Tiếp nhận dữ liệu*: AWS IoT Core nhận tin nhắn MQTT từ thiết bị biên.  

- Hàm Lambda xử lý logic nghiệp vụ (người dùng, game, đơn hàng)- *Lưu trữ dữ liệu*: Dữ liệu thô lưu trong S3 data lake; dữ liệu đã xử lý lưu ở một S3 bucket khác.  

- ALB phân phối lưu lượng đến các microservices EC2- *Xử lý dữ liệu*: AWS Glue Crawlers lập chỉ mục dữ liệu; ETL jobs chuyển đổi để phân tích.  

- Tự động mở rộng theo nhu cầu- *Giao diện web*: AWS Amplify lưu trữ ứng dụng Next.js cho bảng điều khiển và phân tích thời gian thực.  

- *Quản lý người dùng*: Amazon Cognito giới hạn 5 tài khoản hoạt động.  

**Tầng dữ liệu**

- RDS (PostgreSQL): Tài khoản người dùng, danh mục game, đơn hàng### 4. Triển khai kỹ thuật  

- S3: Tệp game, hình ảnh, nội dung người dùng*Các giai đoạn triển khai*  

- SQS: Hàng đợi xử lý đơn hàng bất đồng bộDự án gồm 2 phần — thiết lập trạm thời tiết biên và xây dựng nền tảng thời tiết — mỗi phần trải qua 4 giai đoạn:  

- SNS: Thông báo email/SMS1. *Nghiên cứu và vẽ kiến trúc*: Nghiên cứu Raspberry Pi với cảm biến ESP32 và thiết kế kiến trúc AWS Serverless (1 tháng trước kỳ thực tập).  

2. *Tính toán chi phí và kiểm tra tính khả thi*: Sử dụng AWS Pricing Calculator để ước tính và điều chỉnh (Tháng 1).  

**Bảo mật & Giám sát**3. *Điều chỉnh kiến trúc để tối ưu chi phí/giải pháp*: Tinh chỉnh (ví dụ tối ưu Lambda với Next.js) để đảm bảo hiệu quả (Tháng 2).  

- Cognito: Nhóm người dùng với MFA4. *Phát triển, kiểm thử, triển khai*: Lập trình Raspberry Pi, AWS services với CDK/SDK và ứng dụng Next.js, sau đó kiểm thử và đưa vào vận hành (Tháng 2–3).  

- IAM: Kiểm soát truy cập dựa trên vai trò

- CloudWatch: Số liệu, nhật ký, cảnh báo*Yêu cầu kỹ thuật*  

- *Trạm thời tiết biên*: Cảm biến (nhiệt độ, độ ẩm, lượng mưa, tốc độ gió), vi điều khiển ESP32, Raspberry Pi làm thiết bị biên. Raspberry Pi chạy Raspbian, sử dụng Docker để lọc dữ liệu và gửi 1 MB/ngày/trạm qua MQTT qua Wi-Fi.  

**Pipeline CI/CD**- *Nền tảng thời tiết*: Kiến thức thực tế về AWS Amplify (lưu trữ Next.js), Lambda (giảm thiểu do Next.js xử lý), AWS Glue (ETL), S3 (2 bucket), IoT Core (gateway và rules), và Cognito (5 người dùng). Sử dụng AWS CDK/SDK để lập trình (ví dụ IoT Core rules tới S3). Next.js giúp giảm tải Lambda cho ứng dụng web fullstack.  

- GitLab → CodePipeline → CodeBuild → Triển khai

### 5. Lộ trình & Mốc triển khai  

#### Luồng người dùng- *Trước thực tập (Tháng 0)*: 1 tháng lên kế hoạch và đánh giá trạm cũ.  

1. Người dùng truy cập trang web qua CloudFront- *Thực tập (Tháng 1–3)*:  

2. Cognito xử lý đăng nhập/đăng ký    - Tháng 1: Học AWS và nâng cấp phần cứng.  

3. API Gateway + Lambda lấy danh mục game từ RDS    - Tháng 2: Thiết kế và điều chỉnh kiến trúc.  

4. Người dùng thêm game vào giỏ (phiên DynamoDB)    - Tháng 3: Triển khai, kiểm thử, đưa vào sử dụng.  

5. Thanh toán kích hoạt Lambda xử lý thanh toán- *Sau triển khai*: Nghiên cứu thêm trong vòng 1 năm.  

6. SQS xếp hàng công việc tạo bản quyền

7. SNS gửi email xác nhận### 6. Ước tính ngân sách  

8. URL được ký trước S3 cung cấp tải xuống an toànCó thể xem chi phí trên [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=621f38b12a1ef026842ba2ddfe46ff936ed4ab01)  

Hoặc tải [tệp ước tính ngân sách](../attachments/budget_estimation.pdf).  

### 4. Dịch vụ AWS

*Chi phí hạ tầng*  

| Dịch vụ | Mục đích | Cấu hình |- AWS Lambda: 0,00 USD/tháng (1.000 request, 512 MB lưu trữ).  

|---------|---------|----------|- S3 Standard: 0,15 USD/tháng (6 GB, 2.100 request, 1 GB quét).  

| CloudFront | CDN | 10M request, 50GB transfer |- Truyền dữ liệu: 0,02 USD/tháng (1 GB vào, 1 GB ra).  

| S3 | Lưu trữ | 100GB (frontend + tài sản) |- AWS Amplify: 0,35 USD/tháng (256 MB, request 500 ms).  

| API Gateway | Quản lý API | 1M request/tháng |- Amazon API Gateway: 0,01 USD/tháng (2.000 request).  

| Lambda | Serverless Compute | 5M invocations, 512MB |- AWS Glue ETL Jobs: 0,02 USD/tháng (2 DPU).  

| EC2 | Microservices | 2x t3.medium |- AWS Glue Crawlers: 0,07 USD/tháng (1 crawler).  

| RDS | Database | db.t3.small Multi-AZ |- MQTT (IoT Core): 0,08 USD/tháng (5 thiết bị, 45.000 tin nhắn).  

| ALB | Load Balancer | 1 ALB |

| Cognito | Xác thực | 10K người dùng hoạt động |*Tổng*: 0,7 USD/tháng, 8,40 USD/12 tháng  

| SQS | Message Queue | 5M request |- *Phần cứng*: 265 USD một lần (Raspberry Pi 5 và cảm biến).  

| SNS | Thông báo | 100K tin nhắn |

| CloudWatch | Giám sát | Số liệu + nhật ký chuẩn |### 7. Đánh giá rủi ro  

| CodePipeline | CI/CD | 1 pipeline |*Ma trận rủi ro*  

- Mất mạng: Ảnh hưởng trung bình, xác suất trung bình.  

### 5. Lộ trình triển khai- Hỏng cảm biến: Ảnh hưởng cao, xác suất thấp.  

- Vượt ngân sách: Ảnh hưởng trung bình, xác suất thấp.  

**Giai đoạn 1 (Tháng 1): Nền tảng**

- Thiết lập tài khoản AWS, cấu hình VPC*Chiến lược giảm thiểu*  

- Triển khai cơ sở dữ liệu RDS- Mạng: Lưu trữ cục bộ trên Raspberry Pi với Docker.  

- S3 buckets + phân phối CloudFront- Cảm biến: Kiểm tra định kỳ, dự phòng linh kiện.  

- Nhóm người dùng Cognito- Chi phí: Cảnh báo ngân sách AWS, tối ưu dịch vụ.  



**Giai đoạn 2 (Tháng 2-3): Phát triển Backend***Kế hoạch dự phòng*  

- Hàm Lambda (xác thực, danh mục, đơn hàng)- Quay lại thu thập thủ công nếu AWS gặp sự cố.  

- Tích hợp API Gateway- Sử dụng CloudFormation để khôi phục cấu hình liên quan đến chi phí.  

- Thiết lập SQS/SNS

- Triển khai ALB### 8. Kết quả kỳ vọng  

*Cải tiến kỹ thuật*: Dữ liệu và phân tích thời gian thực thay thế quy trình thủ công. Có thể mở rộng tới 10–15 trạm.  

**Giai đoạn 3 (Tháng 3): Frontend***Giá trị dài hạn*: Nền tảng dữ liệu 1 năm cho nghiên cứu AI, có thể tái sử dụng cho các dự án tương lai.

- Ứng dụng React/Next.js

- Tích hợp Cognito

- Kết nối API

- Triển khai lên S3 + CloudFront



**Giai đoạn 4 (Tháng 4): Tính năng nâng cao**

- Tích hợp cổng thanh toán

- Bảng điều khiển quản trị

- Bảng điều khiển CloudWatch

- Pipeline CI/CD



**Giai đoạn 5 (Tháng 5): Kiểm thử**
- Kiểm thử tải
- Kiểm toán bảo mật
- Tối ưu hiệu suất
- Kiểm thử chấp nhận người dùng

**Giai đoạn 6 (Tháng 6): Ra mắt**
- Ra mắt thử nghiệm
- Giám sát và phản hồi
- Ra mắt công khai

### 6. Ước tính ngân sách

#### Chi phí hàng tháng (10K người dùng, 1K đơn hàng/tháng)

| Dịch vụ | Chi phí |
|---------|---------|
| CloudFront | $5 |
| S3 | $3 |
| API Gateway | $3.50 |
| Lambda | $20 |
| EC2 (2x t3.medium) | $60 |
| RDS (t3.small Multi-AZ) | $50 |
| ALB | $25 |
| Cognito | $25 |
| SQS + SNS | $4 |
| CloudWatch | $10 |
| Truyền dữ liệu | $15 |
| Route 53 | $2 |
| CodePipeline | $1 |
| Sao lưu | $5 |

**Tổng: ~$228/tháng (~$2,742/năm)**

**Mở rộng:**
- 50K người dùng: ~$650/tháng
- 100K người dùng: ~$1,200/tháng

#### Chi phí một lần
- Phát triển: $5,000-8,000
- Tên miền: $15/năm
- SSL: Miễn phí (ACM)

### 7. Đánh giá rủi ro

| Rủi ro | Ảnh hưởng | Xác suất | Giảm thiểu |
|--------|-----------|----------|------------|
| Tấn công DDoS | Cao | Trung bình | AWS Shield, CloudFront, giới hạn tốc độ |
| Rò rỉ dữ liệu | Nghiêm trọng | Thấp | Mã hóa, IAM, kiểm toán |
| Gian lận thanh toán | Cao | Trung bình | 3D Secure, phát hiện gian lận |
| Lambda Cold Starts | Trung bình | Cao | Provisioned concurrency |
| Quá tải cơ sở dữ liệu | Cao | Thấp | Read replicas, caching |
| Vượt chi phí | Trung bình | Trung bình | Cảnh báo ngân sách, giới hạn tự động mở rộng |

#### Kế hoạch dự phòng
- Sao lưu tự động RDS để khôi phục nhanh
- Triển khai Multi-AZ cho độ sẵn sàng cao
- CodePipeline rollback cho triển khai lỗi
- Trang bảo trì tĩnh cho sự cố AWS

### 8. Kết quả kỳ vọng

#### Kỹ thuật
- **Hiệu suất**: Tải trang <2s toàn cầu
- **Khả năng mở rộng**: Xử lý tăng lưu lượng 10x
- **Độ tin cậy**: 99.9% uptime
- **Bảo mật**: Không rò rỉ, sẵn sàng PCI

#### Kinh doanh
- Giảm 40% tỷ lệ bỏ giỏ hàng
- Giảm 60% thời gian quản lý cơ sở hạ tầng
- Tăng 25-35% doanh thu từ UX tốt hơn
- Tiếp cận thị trường toàn cầu qua CDN

#### Giá trị dài hạn
- Mở rộng đến 100K+ người dùng
- Nhóm tích lũy kinh nghiệm AWS
- Microservices tái sử dụng
- Phát triển tính năng nhanh

---

### Bước tiếp theo
1. Phê duyệt đề xuất
2. Thiết lập tài khoản AWS
3. Tập hợp nhóm
4. Bắt đầu Giai đoạn 1
5. Đánh giá tiến độ hàng tuần
