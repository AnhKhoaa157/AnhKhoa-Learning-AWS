---
title: "Blog 1"
date: "2025-11-11T03:24:36Z"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "  
---
# Cho phép khách hàng triển khai AI agent sẵn sàng sản xuất ở quy mô lớn

> Ghi chú: Bố cục đã được chuẩn hóa theo template (metadata, mục lục, đánh số mục) để dễ đọc và đồng nhất với Blog 2. Nội dung giữ nguyên ý chính của bài gốc.

Thông tin nhanh:

- Tác giả: Swami Sivasubramanian  
- Ngày đăng: 16 Tháng 7, 2025  
- Chủ đề: Amazon Bedrock, Amazon Connect, Amazon Nova, Amazon Q, Amazon S3, Thông báo, AWS Inferentia, AWS Trainium, AWS Transform, Nổi bật, Lãnh đạo tư tưởng

---

## Mục lục

1. Giới thiệu
2. Vấn đề đặt ra / Thách thức
3. Nguyên tắc hướng dẫn / Giải pháp của AWS  
  3.1. Linh hoạt (Agility)  
  3.2. Cơ bản cho kỷ nguyên agent (Evolve fundamentals)  
  3.3. Lựa chọn mô hình & dữ liệu  
  3.4. Triển khai các giải pháp thay đổi trải nghiệm
4. Các thành phần mới được giới thiệu
5. Lộ trình tiếp theo / Kêu gọi hành động
6. Kết luận
7. Tài nguyên liên quan

---

## 1. Giới thiệu

AI agents (các tác tử AI) sẽ thay đổi cách chúng ta làm việc và sinh sống. CEO của AWS, Matt Garman, đã chia sẻ tầm nhìn về một sự thay đổi công nghệ có mức độ biến đổi tương đương với sự xuất hiện của Internet. Tôi rất hào hứng với tầm nhìn này vì đã chứng kiến trực tiếp cách các hệ thống agent thông minh đang bắt đầu giải quyết các vấn đề phức tạp, tự động hóa quy trình công việc và mở ra những khả năng mới trong nhiều ngành nghề.

![AI Agent Overview](https://d2908q01vomqb2.cloudfront.net/f1f836cb4ea6efb2a0b1b99f41ad8b103eff4b59/2025/07/16/2025-0716_middle-grey-studios_aws-amers_preview-001_websize.jpg)

---

## 2. Vấn đề đặt ra / Thách thức

- Để mở rộng những thành công ban đầu sang việc áp dụng rộng rãi, các tổ chức cần một hướng tiếp cận thực tiễn để xử lý tính phức tạp vốn có của các hệ thống agent.  
- Cần vừa đổi mới nhanh, vừa thiết lập nền tảng mạnh về bảo mật, độ tin cậy, và vận hành xuất sắc.  
- Hệ thống phải linh hoạt để thích nghi với sự phát triển của mô hình, các khả năng mới, và mở rộng dùng cho nhiều trường hợp sử dụng trong doanh nghiệp.

---

## 3. Nguyên tắc hướng dẫn / Giải pháp của AWS

AWS tập trung vào những khả năng mới để giải quyết các khía cạnh cơ bản khi xây dựng và triển khai agent ở quy mô sản xuất. Dưới đây là các nguyên tắc và thành phần chính:

### 3.1 Linh hoạt (Agility)

Hệ thống agent phải chấp nhận sự thay đổi, mở với công nghệ mới, tích hợp với nguồn dữ liệu riêng và các công cụ hiện có.

### 3.2 Cơ bản được phát triển cho kỷ nguyên agent (Evolve fundamentals)

- **Bảo mật và niềm tin**: môi trường chạy mỗi session riêng, ngăn cách bộ nhớ để tránh rò rỉ dữ liệu.  
- **Độ tin cậy và khả năng mở rộng**: xử lý số phiên song song lớn, checkpoint & recovery cho các lỗi/gián đoạn.  
- **Danh tính (Identity)**: cho cả agent lẫn người dùng, quyền truy cập tạm thời, chi tiết.  
- **Quan sát và giám sát (Observability)**: dashboard thời gian thực, telemetry chuẩn tích hợp với hệ thống giám sát.

### 3.3 Lựa chọn mô hình & dữ liệu để đưa ra kết quả vượt trội (Model choice & data)

- Cho phép khách hàng chọn hoặc tùy chỉnh mô hình phù hợp với từng trường hợp sử dụng.  
- Mở rộng khả năng tùy chỉnh như tinh chỉnh (fine-tuning), huấn luyện lại, PEFT, SFT, PPO, v.v.  
- Dữ liệu riêng của tổ chức là chìa khóa để mô hình hiểu môi trường đặc thù.

### 3.4 Triển khai các giải pháp thay đổi trải nghiệm (Deploy transformative solutions)

- Cung cấp sẵn các agent & công cụ trong **AWS Marketplace** để khách hàng dễ tiếp cận.  
- Các giải pháp agent xây dựng sẵn giúp giảm thời gian từ ý tưởng đến sản phẩm, tích hợp nhanh với hệ thống hiện có.

![Guiding Principles Diagram](https://d2908q01vomqb2.cloudfront.net/f1f836cb4ea6efb2a0b1b99f41ad8b103eff4b59/2025/07/16/image6.png)

---

## 4. Các thành phần mới được giới thiệu

- **Amazon Bedrock AgentCore**: cung cấp runtime serverless an toàn, isolation giữa các session, kiểm soát quyền & context, phù hợp cho môi trường doanh nghiệp.  
- **Nova customization trong Amazon SageMaker AI**: tùy chỉnh mô hình trong nhiều giai đoạn; hỗ trợ fine-tuning, alignment, PEFT, SFT, PPO, CPT, Knowledge Distillation.  
- **Nova Act SDK**: mô hình chuyên được đào tạo để thực hiện các hành động bên trong trình duyệt, hỗ trợ tạo các agent tự động hóa tác vụ web.  
- **Amazon S3 Vectors**: kho lưu trữ đối tượng đám mây có hỗ trợ vector native, giúp lưu embeddings vector với chi phí thấp hơn ~90%, vẫn đảm bảo tốc độ truy vấn dưới giây.

---

## 5. Lộ trình tiếp theo / Kêu gọi hành động

- **Bắt đầu nhanh**: chọn một vấn đề kinh doanh cụ thể quan trọng và bắt tay xây dựng.  
- **Tích lũy phản hồi thực tế** để điều chỉnh liên tục.  
- AWS tăng cường đầu tư: **100 triệu USD** vào **AWS Generative AI Innovation Center** để hỗ trợ khách hàng trong nhiều ngành nghề nhằm tạo ra giá trị thực từ AI agent.

---

## 6. Kết luận

Với các nguyên tắc hướng dẫn rõ ràng, các nền tảng kỹ thuật mạnh, và các thành phần mới hỗ trợ, AWS đang xây dựng một cơ sở toàn diện để giúp khách hàng triển khai agent AI ở quy mô sản xuất — tin cậy, an toàn và có hiệu quả.

Không cần chờ đợi mọi thứ hoàn hảo mới bắt đầu, mà hãy bắt tay vào thực hiện từng bước, học hỏi từ thực tế, và dần dần mở rộng.

---

## 7. Tài nguyên liên quan

- [Xem bài gốc tại AWS](https://aws.amazon.com/vi/blogs/machine-learning/enabling-customers-to-deliver-production-ready-ai-agents-at-scale/)

Các data lake có thể giúp các bệnh viện và cơ sở y tế chuyển dữ liệu thành những thông tin chi tiết về doanh nghiệp và duy trì hoạt động kinh doanh liên tục, đồng thời bảo vệ quyền riêng tư của bệnh nhân. **Data lake** là một kho lưu trữ tập trung, được quản lý và bảo mật để lưu trữ tất cả dữ liệu của bạn, cả ở dạng ban đầu và đã xử lý để phân tích. data lake cho phép bạn chia nhỏ các kho chứa dữ liệu và kết hợp các loại phân tích khác nhau để có được thông tin chi tiết và đưa ra các quyết định kinh doanh tốt hơn.

Bài đăng trên blog này là một phần của loạt bài lớn hơn về việc bắt đầu cài đặt data lake dành cho lĩnh vực y tế. Trong bài đăng blog cuối cùng của tôi trong loạt bài, *“Bắt đầu với data lake dành cho lĩnh vực y tế: Đào sâu vào Amazon Cognito”*, tôi tập trung vào các chi tiết cụ thể của việc sử dụng Amazon Cognito và Attribute Based Access Control (ABAC) để xác thực và ủy quyền người dùng trong giải pháp data lake y tế. Trong blog này, tôi trình bày chi tiết cách giải pháp đã phát triển ở cấp độ cơ bản, bao gồm các quyết định thiết kế mà tôi đã đưa ra và các tính năng bổ sung được sử dụng. Bạn có thể truy cập các code samples cho giải pháp tại Git repo này để tham khảo.

---

## Hướng dẫn kiến trúc

Thay đổi chính kể từ lần trình bày cuối cùng của kiến trúc tổng thể là việc tách dịch vụ đơn lẻ thành một tập hợp các dịch vụ nhỏ để cải thiện khả năng bảo trì và tính linh hoạt. Việc tích hợp một lượng lớn dữ liệu y tế khác nhau thường yêu cầu các trình kết nối chuyên biệt cho từng định dạng; bằng cách giữ chúng được đóng gói riêng biệt với microservices, chúng ta có thể thêm, xóa và sửa đổi từng trình kết nối mà không ảnh hưởng đến những kết nối khác. Các microservices được kết nối rời thông qua tin nhắn publish/subscribe tập trung trong cái mà tôi gọi là “pub/sub hub”.

Giải pháp này đại diện cho những gì tôi sẽ coi là một lần lặp nước rút hợp lý khác từ last post của tôi. Phạm vi vẫn được giới hạn trong việc nhập và phân tích cú pháp đơn giản của các **HL7v2 messages** được định dạng theo **Quy tắc mã hóa 7 (ER7)** thông qua giao diện REST.

**Kiến trúc giải pháp bây giờ như sau:**

> *Hình 1. Kiến trúc tổng thể; những ô màu thể hiện những dịch vụ riêng biệt.*

---

Mặc dù thuật ngữ *microservices* có một số sự mơ hồ cố hữu, một số đặc điểm là chung:  
- Chúng nhỏ, tự chủ, kết hợp rời rạc  
- Có thể tái sử dụng, giao tiếp thông qua giao diện được xác định rõ  
- Chuyên biệt để giải quyết một việc  
- Thường được triển khai trong **event-driven architecture**

Khi xác định vị trí tạo ranh giới giữa các microservices, cần cân nhắc:  
- **Nội tại**: công nghệ được sử dụng, hiệu suất, độ tin cậy, khả năng mở rộng  
- **Bên ngoài**: chức năng phụ thuộc, tần suất thay đổi, khả năng tái sử dụng  
- **Con người**: quyền sở hữu nhóm, quản lý *cognitive load*

---

## Lựa chọn công nghệ và phạm vi giao tiếp

| Phạm vi giao tiếp                        | Các công nghệ / mô hình cần xem xét                                                        |
| ---------------------------------------- | ------------------------------------------------------------------------------------------ |
| Trong một microservice                   | Amazon Simple Queue Service (Amazon SQS), AWS Step Functions                               |
| Giữa các microservices trong một dịch vụ | AWS CloudFormation cross-stack references, Amazon Simple Notification Service (Amazon SNS) |
| Giữa các dịch vụ                         | Amazon EventBridge, AWS Cloud Map, Amazon API Gateway                                      |

---

## The pub/sub hub

Việc sử dụng kiến trúc **hub-and-spoke** (hay message broker) hoạt động tốt với một số lượng nhỏ các microservices liên quan chặt chẽ.  
- Mỗi microservice chỉ phụ thuộc vào *hub*  
- Kết nối giữa các microservice chỉ giới hạn ở nội dung của message được xuất  
- Giảm số lượng synchronous calls vì pub/sub là *push* không đồng bộ một chiều

Nhược điểm: cần **phối hợp và giám sát** để tránh microservice xử lý nhầm message.

---

## Core microservice

Cung cấp dữ liệu nền tảng và lớp truyền thông, gồm:  
- **Amazon S3** bucket cho dữ liệu  
- **Amazon DynamoDB** cho danh mục dữ liệu  
- **AWS Lambda** để ghi message vào data lake và danh mục  
- **Amazon SNS** topic làm *hub*  
- **Amazon S3** bucket cho artifacts như mã Lambda

> Chỉ cho phép truy cập ghi gián tiếp vào data lake qua hàm Lambda → đảm bảo nhất quán.

---

## Front door microservice

- Cung cấp API Gateway để tương tác REST bên ngoài  
- Xác thực & ủy quyền dựa trên **OIDC** thông qua **Amazon Cognito**  
- Cơ chế *deduplication* tự quản lý bằng DynamoDB thay vì SNS FIFO vì:
  1. SNS deduplication TTL chỉ 5 phút
  2. SNS FIFO yêu cầu SQS FIFO
  3. Chủ động báo cho sender biết message là bản sao

---

## Staging ER7 microservice

- Lambda “trigger” đăng ký với pub/sub hub, lọc message theo attribute  
- Step Functions Express Workflow để chuyển ER7 → JSON  
- Hai Lambda:
  1. Sửa format ER7 (newline, carriage return)
  2. Parsing logic  
- Kết quả hoặc lỗi được đẩy lại vào pub/sub hub

---

## Tính năng mới trong giải pháp

### 1. AWS CloudFormation cross-stack references
Ví dụ *outputs* trong core microservice (các giá trị này được export để các stack khác có thể import bằng `Fn::ImportValue` / `!ImportValue`):

```yaml
Outputs:
  Bucket:
    Value: !Ref Bucket
    Export:
      Name: !Sub ${AWS::StackName}-Bucket
  ArtifactBucket:
    Value: !Ref ArtifactBucket
    Export:
      Name: !Sub ${AWS::StackName}-ArtifactBucket
  Topic:
    Value: !Ref Topic
    Export:
      Name: !Sub ${AWS::StackName}-Topic
  Catalog:
    Value: !Ref Catalog
    Export:
      Name: !Sub ${AWS::StackName}-Catalog
  CatalogArn:
    Value: !GetAtt Catalog.Arn
    Export:
      Name: !Sub ${AWS::StackName}-CatalogArn
```

Một stack consumer có thể import các giá trị này bằng `Fn::ImportValue` (hoặc `!ImportValue` trong YAML). Ví dụ: import Catalog ARN vào biến môi trường của một Lambda.

```yaml
Resources:
  MyFunction:
    Type: AWS::Lambda::Function
    Properties:
      Handler: index.handler
      Role: !GetAtt LambdaExecutionRole.Arn
      Runtime: nodejs18.x
      Environment:
        Variables:
          CATALOG_ARN: !ImportValue MyCoreStack-CatalogArn
```

Nếu tên export được parameterize, bạn có thể xây dựng tên import bằng `Fn::Sub`:

```yaml
Parameters:
  CoreStackName:
    Type: String
Resources:
  MyFunction2:
    Type: AWS::Lambda::Function
    Properties:
      Handler: index.handler
      Role: !GetAtt LambdaExecutionRole.Arn
      Runtime: nodejs18.x
      Environment:
        Variables:
          CATALOG_ARN: !ImportValue
            'Fn::Sub': '${CoreStackName}-CatalogArn'
```

Lưu ý: Cross-stack exports giúp tách module trong deployment nhưng tạo phụ thuộc về thứ tự triển khai — stack export phải tồn tại trước khi stack import có thể resolve các giá trị.













