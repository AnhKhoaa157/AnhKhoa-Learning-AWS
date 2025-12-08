---
title: "Blog 1 - Riskthinking.AI Climate Earth Digital Twin on AWS"
date: 2025-04-02
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Giới thiệu Riskthinking.AI Climate Earth Digital Twin trên AWS

**Tác giả**: Ilan Gleiser, Andrew Wiebe, Dilshan Kathriarachchi, Marco Masciola, và Ron Dembo  
**Ngày đăng**: 02/04/2025  
**Danh mục**: Amazon Elastic Kubernetes Service, AWS Batch, High Performance Computing, Partner solutions

---

## Tổng quan

Biến đổi khí hậu đang gây ra những thách thức ngày càng tăng cho các hệ sinh thái, nền kinh tế và cơ sở hạ tầng trên toàn cầu. Nhiệt độ tăng cao, thay đổi các mô hình thời tiết, và tần suất các hiện tượng thời tiết cực đoan gia tăng đang tạo áp lực lên cả tài sản công và tư.

Đặc biệt trong **lĩnh vực điện lực**, các nhà máy phát điện, đường dây truyền tải và mạng lưới phân phối phải đối mặt với rủi ro đáng kể:

- Nắng nóng kéo dài có thể làm giảm hiệu suất của các nhà máy nhiệt điện và gây quá tải lưới điện
- Hạn hán làm giảm mực nước, ảnh hưởng đến khả năng phát điện của các nhà máy thủy điện
- Các trạm điện ven biển dễ bị tổn thương trước mực nước biển dâng và sóng thần
- Tuabin gió và tấm pin mặt trời có thể bị hư hại bởi các cơn bão mạnh hơn

Một ví dụ cụ thể là cơ sở hạ tầng điện tại **Ý**. Quốc gia này phụ thuộc vào hỗn hợp năng lượng đa dạng gồm điện mặt trời, gió, thủy điện và nhiên liệu hóa thạch, khiến nó dễ bị ảnh hưởng bởi nhiều loại rủi ro khí hậu vật lý.

---

## Riskthinking.AI's Climate Earth Digital Twin (CDT™)

**Riskthinking.AI** đã phát triển **Climate Earth Digital Twin (CDT™)** để đánh giá rủi ro tài chính và tác động của biến đổi khí hậu ở nhiều quy mô khác nhau, từ toàn bộ nền kinh tế đến từng tài sản riêng lẻ. 

### Cơ sở dữ liệu

CDT tích hợp dữ liệu về hàng triệu tài sản vật lý với các dự báo khí hậu chi tiết:

- **13.000+** công ty mẹ công khai
- **5 triệu+** tài sản vật lý
- Mỗi tài sản được căn chỉnh về mặt không gian địa lý
- Đánh giá mức độ phơi nhiễm với các loại rủi ro khí hậu khác nhau

### Phương pháp phân tích

CDT sử dụng **phân phối xác suất** thay vì dự báo xác định để mô phỏng một loạt các kịch bản tương lai. Kiểm tra áp lực đa yếu tố cho phép khách hàng đánh giá cách nhiều rủi ro, khi xảy ra đồng thời, có thể ảnh hưởng đến giá trị của một tài sản theo thời gian.

### Yêu cầu tính toán

Việc chạy phân tích rủi ro khí hậu toàn diện đòi hỏi:

- **Hàng petabyte** dữ liệu cần xử lý
- Căn chỉnh thông tin khí hậu và tài sản cả về không gian và thời gian
- Đánh giá mỗi tài sản dưới nhiều mối nguy khí hậu
- Phân tích qua nhiều giai đoạn: 5, 10, 15, 20 và 25 năm

Kiểm tra áp lực đa yếu tố này được hỗ trợ bởi **thuật toán Dembo được cấp bằng sáng chế**, đòi hỏi nguồn lực tính toán khổng lồ.

### Kết quả phân tích

Kết quả cuối cùng là một bài kiểm tra áp lực đa yếu tố tổng hợp:

- **Value-at-Risk (VaR)** - chỉ số rủi ro tổng hợp
- **Điểm Phơi nhiễm** - xác suất về tác động biến đổi khí hậu trong tương lai
- Phân tách theo từng yếu tố rủi ro: nóng cực đoan, lũ lụt, cháy rừng, bão nhiệt đới
- Độ phân giải không gian: ~10 km x 10 km trên toàn cầu

---

## Kiến trúc AWS

![CDT Architecture](/images/3.1-Blog1/image11.png)
*Hình 1. Climate Earth Digital Twin tận dụng khả năng High Performance Computing (HPC) của AWS, kết hợp Amazon EKS để điều phối các mô phỏng quy mô lớn với AWS Batch để phân bổ tài nguyên hiệu quả. Kiến trúc HPC mạnh mẽ này cho phép CDT xử lý hàng tỷ mô phỏng khí hậu, tự động mở rộng từ 100 đến 1.000.000 compute instances khi cần.*

### Dịch vụ cốt lõi

Climate Earth Digital Twin chạy trên AWS sử dụng:

#### 1. Amazon Elastic Kubernetes Service (EKS)
- Điều phối các mô phỏng khí hậu quy mô lớn
- Quản lý và mở rộng ứng dụng containerized
- Xử lý từ 300 tỷ đến hơn 1 nghìn tỷ mô phỏng
- Triển khai liên tục thông qua Amazon ECR

#### 2. AWS Batch
- Phân bổ tài nguyên tính toán hiệu quả
- Tự động mở rộng từ 100 đến 1.000.000 instances
- Phối hợp với EKS để quản lý workload

#### 3. AWS Lambda
- Pipeline dữ liệu serverless
- Tự động hóa thu thập, chuyển đổi và xử lý dữ liệu
- Tối ưu hóa hiệu suất và chi phí

#### 4. Amazon S3
- Lưu trữ chính cho dữ liệu khí hậu
- Hàng petabyte dữ liệu
- Độ bền dữ liệu cao
- Truy xuất nhanh khi cần thiết

#### 5. Amazon RDS (PostgreSQL)
- Lưu trữ dữ liệu có cấu trúc
- Mô hình khí hậu lịch sử và dự báo
- Giải pháp database được quản lý

#### 6. Amazon ElastiCache (Redis)
- Lớp caching giảm độ trễ
- Tăng tốc truy cập dữ liệu thường xuyên
- Đảm bảo hiệu suất mô phỏng

### CDT API

API đóng vai trò quan trọng:

- Cung cấp giao diện truy cập phân tích rủi ro khí hậu
- Tương tác liền mạch với Công cụ Phân tích
- Hiệu suất nhất quán dưới khối lượng công việc nặng
- Thông tin chi tiết rủi ro khí hậu thời gian thực

---

## Thiết lập đơn giản với CDT API

CDT API đóng vai trò quan trọng trong việc cung cấp kết quả của các mô phỏng này cho khách hàng, cung cấp giao diện trực tiếp để truy cập phân tích rủi ro khí hậu của CDT.

### Ví dụ cấu hình

```python
# Cấu hình API key và endpoint để truy cập Climate Digital Twin của RiskThinking.AI
API_URL = "https://api.riskthinking.ai"

# Tham số rủi ro khí hậu: tập trung vào các khoảng thời gian cụ thể và lộ trình kịch bản
HORIZONS = {2025, 2030, 2050, 2070, 2090}  # Các năm dự báo tương lai
PATHWAYS = {"SV"}  # "SV" đề cập đến kịch bản Stochastic View được phát triển bởi RiskThinking.AI
```

### Lấy dữ liệu tài sản vật lý từ CDT

Đoạn code dưới đây lấy dữ liệu tài sản vật lý liên quan đến lĩnh vực sản xuất, truyền tải và phân phối điện năng ở Ý từ API.

```python
all_assets = []
cursor = ""

# Lặp qua các phản hồi API được phân trang để thu thập dữ liệu tài sản
all_assets = []
page_size = 100
cursor = None

while True:
    # API call để lấy tài sản
    response = api.get_physical_assets(
        country='Italy',
        sector='electricity',
        page_size=page_size,
        cursor=cursor
    )
    
    # Trích xuất thông tin tài sản
    for asset in response['assets']:
        all_assets.append({
            'id': asset['id'],
            'state': asset['state'],
            'longitude': asset['longitude'],
            'latitude': asset['latitude'],
            'hex_id': asset['hex_id']
        })
    
    # Kiểm tra trang cuối
    if not response['has_more']:
        break
    cursor = response['next_cursor']
```

### Code mẫu: Làm giàu với phân tích rủi ro

```python
# Làm giàu tài sản với dữ liệu rủi ro khí hậu
all_climate_scores = []

PATHWAYS = ['ssp126', 'ssp245', 'ssp585']
HORIZONS = [2030, 2040, 2050, 2070, 2090]

for asset in all_assets:
    # Lấy dữ liệu rủi ro cho từng tài sản
    risk_data = api.get_climate_risk(asset['id'])
    
    for score in risk_data['scores']:
        if score['pathway'] in PATHWAYS and score['horizon'] in HORIZONS:
            all_climate_scores.append({
                'asset_id': asset['id'],
                'state': asset['state'],
                'hex_id': asset['hex_id'],
                'year': score['horizon'],
                'pathway': score['pathway'],
                'expected_impact': score['expected_impact'],
                'var_95': score['value_at_risk_95']
            })
```

---

## Phân tích Case Study: Cơ sở hạ tầng điện của Ý

### Phân bố không gian các cơ sở phát điện

![Italy Power Plants](/images/3.1-Blog1/image2.png)
*Hình 2. Vị trí của các nhà máy phát điện nhiên liệu hóa thạch, thủy điện, và năng lượng mặt trời trên khắp Ý*

Cơ sở dữ liệu của Riskthinking.AI chứa:

- Nhà máy phát điện nhiên liệu hóa thạch
- Nhà máy thủy điện
- Trang trại năng lượng mặt trời
- Trạm biến áp và hệ thống phân phối

**Quan sát quan trọng**: Các nhà máy thủy điện tập trung cao ở phía bắc Ý, gây ra rủi ro tập trung - một sự kiện khí hậu có thể ảnh hưởng đồng thời nhiều cơ sở.

### Rủi ro biến đổi khí hậu hiện tại và tương lai

![Climate Risks 2025 vs 2090](/images/3.1-Blog1/image6.png)
*Hình 3. Mô hình không gian của các tác động yếu tố cho 2025 và 2090*

#### Kết quả phân tích chính:

**Hiện tại (2025)**:
- Chu kỳ đóng băng-tan băng: mối đe dọa lớn nhất cho nhà máy nhiên liệu hóa thạch và mặt trời
- Lạnh cực đoan: ảnh hưởng chủ yếu đến nhà máy thủy điện
- Nóng cực đoan: bắt đầu ảnh hưởng tất cả các loại

**Tương lai (2090)**:
- Tác động từ đóng băng-tan băng tăng lên **5,27%** (từ 2,48%)
- Nóng cực đoan tăng đáng kể cho tất cả loại cơ sở
- Nhà máy mặt trời: giảm công suất 10-25% ở nhiệt độ cao
- Ở điều kiện cực đoan: tấm pin có thể đạt 65°C

![Impact Analysis](/images/3.1-Blog1/image7.png)
*Hình 4. Tác động về mặt giá trị tài sản bị mất do các yếu tố rủi ro khác nhau*

### Tác động cụ thể theo loại cơ sở

#### Nhà máy Thủy điện
- Dễ bị tổn thương trước chu kỳ đóng băng-tan băng
- Hạn hán làm giảm mực nước
- Tập trung địa lý tăng rủi ro hệ thống

#### Nhà máy Năng lượng Mặt trời
- Nóng cực đoan giảm hiệu suất
- Hoạt động tốt nhất ở 15-35°C
- Bão mạnh có thể gây hư hại vật lý

#### Nhà máy Nhiên liệu Hóa thạch
- Quá tải lưới điện trong đợt nóng
- Chu kỳ đóng băng-tan băng ảnh hưởng cấu trúc
- Thay đổi hiệu suất theo nhiệt độ

---

## Lợi ích của CDT trên AWS

### 1. Khả năng mở rộng
- Tự động scale từ 100 đến 1 triệu instances
- Xử lý hàng nghìn tỷ mô phỏng
- Đáp ứng nhu cầu tăng trưởng

### 2. Hiệu suất
- HPC với EKS và Batch
- Caching với ElastiCache
- Pipeline serverless với Lambda

### 3. Chi phí tối ưu
- Pay-as-you-go pricing
- Tự động quản lý tài nguyên
- Tối ưu hóa storage với S3

### 4. Độ tin cậy
- Lưu trữ dữ liệu bền vững với S3
- RDS managed database
- Continuous deployment với ECR

### 5. Tính linh hoạt
- Container-based architecture
- API-driven access
- Multi-scenario analysis

---

## Kết luận

Phân tích của CDT cho thấy cơ sở hạ tầng phát điện của Ý đối mặt với rủi ro ngày càng tăng từ biến đổi khí hậu:

**Phát hiện chính**:
- Sự kiện thời tiết cực đoan gia tăng về tần suất và cường độ
- Tập trung địa lý của thủy điện tạo rủi ro hệ thống
- Nóng cực đoan ảnh hưởng tất cả loại cơ sở phát điện
- Cần các chiến lược thích ứng và giảm thiểu rủi ro

**Giá trị của CDT**:
- Đánh giá rủi ro toàn diện với VaR và Exposure Score
- Phân tách theo yếu tố rủi ro cụ thể
- Mô hình xác suất qua nhiều kịch bản
- Hỗ trợ quyết định đầu tư cơ sở hạ tầng

**Vai trò của AWS**:
- Cung cấp nền tảng HPC mạnh mẽ
- Đảm bảo khả năng mở rộng và độ tin cậy
- Tối ưu chi phí với pay-as-you-go
- Hỗ trợ phân tích khí hậu toàn cầu

Các chính phủ và tổ chức được khuyến khích thực hiện các đánh giá rủi ro khí hậu tương tự để đảm bảo khả năng phục hồi và tính bền vững dài hạn.

---

## Liên hệ

**Riskthinking.AI**  
- Email: info@riskthinking.ai  
- Website: www.riskthinking.ai

**AWS HPC Team**  
- Email: aws-hpc-info@amazon.com

---

## Tài liệu tham khảo

[1] Ron Dembo, *Risk Thinking, Appendix 1*, Archway Press 2020  
[2] Ron Dembo, *Algorithmic Scenario Generation*, Risk.Net, July 2020

---

## Về tác giả

<div style="display: flex; align-items: flex-start; margin-bottom: 20px;">
  <img src="/images/3.1-Blog1/image3.png" alt="Ilan Gleiser" style="width: 120px; height: 120px; object-fit: cover; border-radius: 50%; margin-right: 20px;" />
  <div>
    <h3 style="margin-top: 0;">Ilan Gleiser</h3>
    <p>Ilan Gleiser là Principal GenAI Specialist tại AWS WWSO Frameworks team, tập trung vào phát triển kiến trúc Artificial General Intelligence có thể mở rộng và tối ưu hóa việc training và inference của foundation models. Với nền tảng vững chắc về AI và machine learning, Ilan đã xuất bản hơn 20 blog và phát triển 100+ prototype trên toàn cầu trong 5 năm qua. Ilan có bằng Thạc sĩ về kinh tế toán học.</p>
  </div>
</div>

<div style="display: flex; align-items: flex-start; margin-bottom: 20px;">
  <img src="/images/3.1-Blog1/image1.png" alt="Andrew Wiebe" style="width: 120px; height: 120px; object-fit: cover; border-radius: 50%; margin-right: 20px;" />
  <div>
    <h3 style="margin-top: 0;">Andrew Wiebe</h3>
    <p>Andrew Wiebe, Chief Science Officer tại Riskthinking.AI, là nhà khoa học khí quyển với hơn 20 năm kinh nghiệm trong khí tượng lớp biên, dự báo thời tiết số, dự báo vận hành, phân tích khí hậu, mô hình phân tán, kiểm kê phát thải công nghiệp, phân tích thống kê, phân tích khí hậu và nghiên cứu biến đổi khí hậu. Ông có chuyên môn về high-performance computing và phân tích dữ liệu, đã viết và đồng viết nhiều bài báo được đánh giá ngang hàng. Andrew hiện đang tham gia World Climate Research Program's Coupled Model Intercomparison Project Phase 7 (WCRP-CMIP7) Impact and Adaptation Author Team và Data Advisory Team.</p>
  </div>
</div>

<div style="display: flex; align-items: flex-start; margin-bottom: 20px;">
  <img src="/images/3.1-Blog1/image4.png" alt="Dilshan Kathriarachchi" style="width: 120px; height: 120px; object-fit: cover; border-radius: 50%; margin-right: 20px;" />
  <div>
    <h3 style="margin-top: 0;">Dilshan Kathriarachchi</h3>
    <p>Dilshan Kathriarachchi là lãnh đạo công nghệ dày dạn kinh nghiệm với thành tích đã được chứng minh trong việc mở rộng doanh nghiệp, xây dựng các đội ngũ hiệu suất cao và dẫn đầu đổi mới sản phẩm. Dilshan hiện đảm nhiệm vị trí CTO tại Riskthinking.AI. Ông đóng vai trò quan trọng trong việc chuyển đổi EQ Works (TSXV: EQ) từ nhà cung cấp AdTech thành doanh nghiệp phân tích địa lý không gian dựa trên dữ liệu. Trước đó, với vai trò President và CTO của Addictive Mobility, Dilshan tiên phong thành lập một trong những công ty AdTech đầu tiên của Canada. Với hơn 15 năm kinh nghiệm, ông tận dụng chuyên môn sâu rộng về machine learning, data infrastructure, AI và geospatial analytics để thúc đẩy sứ mệnh của công ty trong việc cung cấp thông tin rủi ro khí hậu có thể hành động.</p>
  </div>
</div>

<div style="display: flex; align-items: flex-start; margin-bottom: 20px;">
  <img src="/images/3.1-Blog1/image8.png" alt="Marco Masciola" style="width: 120px; height: 120px; object-fit: cover; border-radius: 50%; margin-right: 20px;" />
  <div>
    <h3 style="margin-top: 0;">Marco Masciola</h3>
    <p>Marco Masciola là Senior Partner Solutions Architect cho Global Sustainability Partner Segment tại Amazon Web Services. Marco giúp các đối tác AWS xây dựng và cung cấp các giải pháp bền vững thông qua AWS cloud.</p>
  </div>
</div>

<div style="display: flex; align-items: flex-start; margin-bottom: 20px;">
  <img src="/images/3.1-Blog1/image10.png" alt="Ron Dembo" style="width: 120px; height: 120px; object-fit: cover; border-radius: 50%; margin-right: 20px;" />
  <div>
    <h3 style="margin-top: 0;">Ron Dembo</h3>
    <p>Tiến sĩ Ron Dembo từfng là Phó Giáo sư tại Yale, giáo sư thỉnh giảng tại MIT, cố vấn cho Goldman Sachs và các tập đoàn và ngân hàng lớn khác, và là tác giả. Ông sáng lập Algorithmics, công ty mà ông phát triển trở thành nhà cung cấp hệ thống quản lý rủi ro tài chính doanh nghiệp lớn nhất cho các ngân hàng trên toàn thế giới trước khi bán cho Fitch và sau đó là IBM. Ông đã xuất bản nhiều bài báo trên các tạp chí khoa học và là tác giả của nhiều bằng sáng chế toàn cầu. Ông được phong thành viên trọn đời của Fields Institute of Mathematics vào năm 2007 vì đóng góp cho Toán học Canada và đã nhận được nhiều giải thưởng cho công trình về tối ưu hóa, quản lý rủi ro và biến đổi khí hậu. Với vai trò Founder và CEO của Riskthinking.AI, Ron đã áp dụng chuyên môn về mô hình hóa kịch bản đa yếu tố được cấp bằng sáng chế để tạo ra một nền tảng dữ liệu và công cụ phân tích để đo lường và quản lý rủi ro tài chính khí hậu.</p>
  </div>
</div>

---

**Tags**: Weather, Climate Risk, Digital Twin, High Performance Computing, Amazon EKS, AWS Batch, Amazon S3, Machine Learning

