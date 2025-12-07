---
title: "Thiết lập Lưu trữ (S3)"
date: "2025-12-06T00:00:00Z"
weight: 3
chapter: false
pre: " <b> 5.7.3. </b> "
---

#### Tổng quan

Trong phần này, chúng ta sẽ cấu hình Amazon S3 (Simple Storage Service) để lưu trữ hình ảnh game, avatars và các file media khác. S3 cung cấp object storage có khả năng mở rộng, bền vững và tính khả dụng cao.

**Những gì chúng ta sẽ cấu hình:**
- Tạo S3 bucket với naming phù hợp
- Cấu hình CORS cho frontend access
- Bucket policy cho public read access
- IAM user credentials cho backend
- Tích hợp với Spring Boot S3Service

#### Kiến trúc

```
┌──────────────┐         ┌──────────────┐         ┌──────────────┐
│   React      │────────►│  Spring Boot │────────►│   Amazon S3  │
│   Frontend   │  POST   │   S3Service  │  SDK v2 │    Bucket    │
└──────────────┘  image  └──────────────┘         └──────────────┘
       │                                                   │
       └───────────────────GET image──────────────────────┘
                      (public URL)
```

#### Bước 1: Tạo S3 Bucket

1. **Điều hướng đến S3 Console**
   - Truy cập: https://s3.console.aws.amazon.com/
   - Region: **ap-southeast-1** (Singapore)

2. **Tạo Bucket**
   - Click **Create bucket**
   - Bucket name: `game-store-images-2025` (phải unique toàn cầu!)
   - AWS Region: **Asia Pacific (Singapore) ap-southeast-1**

{{% notice tip %}}
**Bucket Naming:** Tên S3 bucket phải unique toàn cầu trong tất cả AWS accounts. Nếu `game-store-images-2025` đã được dùng, thử: `game-store-images-tenban-2025` hoặc `game-store-images-randomnumber`
{{% /notice %}}

3. **Object Ownership**
   - ACLs disabled (khuyến nghị)
   - Bucket owner enforced

4. **Block Public Access**
   - ☐ **Bỏ chọn** "Block all public access"
   - ☑️ Xác nhận cảnh báo: "I acknowledge that the current settings might result in this bucket and the objects within becoming public"

{{% notice warning %}}
**Public Access:** Chúng ta cho phép public read access chỉ cho các file hình ảnh cần hiển thị trên website. Sử dụng bucket policies để kiểm soát access chính xác.
{{% /notice %}}

5. **Bucket Versioning**
   - Disable (để tiết kiệm chi phí)

6. **Default encryption**
   - Encryption type: **Server-side encryption with Amazon S3 managed keys (SSE-S3)**
   - ☑️ Enable

7. **Advanced settings**
   - Object Lock: **Disable**

8. Click **Create bucket**

#### Bước 2: Cấu hình CORS

CORS (Cross-Origin Resource Sharing) cho phép React frontend upload files trực tiếp đến S3.

1. **Mở Bucket Settings**
   - S3 Console → Buckets → Click **game-store-images-2025**
   - Tab: **Permissions**

2. **Edit CORS Configuration**
   - Scroll đến **Cross-origin resource sharing (CORS)**
   - Click **Edit**

3. **Thêm CORS Rules**

```json
[
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "GET",
            "PUT",
            "POST",
            "DELETE",
            "HEAD"
        ],
        "AllowedOrigins": [
            "http://localhost:5173",
            "http://localhost:3000",
            "https://yourdomain.com"
        ],
        "ExposeHeaders": [
            "ETag",
            "x-amz-server-side-encryption",
            "x-amz-request-id",
            "x-amz-id-2"
        ],
        "MaxAgeSeconds": 3000
    }
]
```

4. Click **Save changes**

{{% notice info %}}
**CORS Origins:** Cập nhật `AllowedOrigins` để bao gồm production domain của bạn khi deploy. Cho development, `http://localhost:5173` (Vite) và `http://localhost:3000` (create-react-app) là đủ.
{{% /notice %}}

#### Bước 3: Thiết lập Bucket Policy cho Public Read

Bucket policies kiểm soát quyền truy cập ở mức bucket.

1. **Edit Bucket Policy**
   - S3 Console → **game-store-images-2025** → Tab **Permissions**
   - Scroll đến **Bucket policy**
   - Click **Edit**

2. **Thêm Policy**

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::game-store-images-2025/*"
        }
    ]
}
```

{{% notice warning %}}
**Thay thế bucket name:** Đổi `game-store-images-2025` thành tên bucket thực tế của bạn trong ARN.
{{% /notice %}}

3. Click **Save changes**

**Policy này làm gì:**
- Cho phép bất kỳ ai (`Principal: "*"`) đọc (`s3:GetObject`) tất cả objects trong bucket
- Upload/delete operations vẫn yêu cầu authentication (xử lý bởi backend)
- Chỉ GET requests là public - hoàn hảo để hiển thị hình ảnh

#### Bước 4: Tạo Cấu trúc Folder

Tổ chức files với cấu trúc folder logic:

1. **Tạo Folders**
   - S3 Console → **game-store-images-2025** → **Create folder**
   
   Tạo các folders sau:
   - `games/` - Hình ảnh cover game
   - `avatars/` - Hình ảnh avatar người dùng
   - `categories/` - Icons danh mục
   - `temp/` - Uploads tạm thời

2. **Cấu trúc Folder**
```
game-store-images-2025/
├── games/
│   ├── game-123-cover.jpg
│   └── game-456-cover.png
├── avatars/
│   ├── user-789-avatar.jpg
│   └── user-101-avatar.png
├── categories/
│   └── action-icon.png
└── temp/
```

#### Bước 5: Tạo IAM User cho Backend

Spring Boot backend của bạn cần AWS credentials để upload files lên S3.

1. **Điều hướng đến IAM Console**
   - Truy cập: https://console.aws.amazon.com/iam/
   - Menu bên trái → **Users** → **Add users**

2. **Tạo User**
   - User name: `game-store-s3-user`
   - Access type: ☑️ **Programmatic access** (access key)
   - Click **Next: Permissions**

3. **Attach Policies**
   - **Attach existing policies directly**
   - Tìm và chọn: `AmazonS3FullAccess`
   - Click **Next: Tags** → **Next: Review**

{{% notice tip %}}
**Least Privilege:** Cho production, tạo custom policy chỉ cho phép truy cập bucket cụ thể của bạn thay vì `AmazonS3FullAccess`.
{{% /notice %}}

**Ví dụ Custom Policy (Production):**
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:DeleteObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::game-store-images-2025",
                "arn:aws:s3:::game-store-images-2025/*"
            ]
        }
    ]
}
```

4. **Tạo User và Lưu Credentials**
   - Click **Create user**
   - ⚠️ **Download CSV** hoặc copy credentials:
     - Access key ID: `AKIAIOSFODNN7EXAMPLE`
     - Secret access key: `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`

{{% notice warning %}}
**Bảo mật:** Lưu trữ credentials an toàn! Không bao giờ commit chúng vào Git. Đây là cơ hội duy nhất để xem secret access key.
{{% /notice %}}

#### Bước 6: Cấu hình Backend (Spring Boot)

1. **Cập nhật `application.yaml`**

```yaml
aws:
  region: ap-southeast-1
  access-key: ${AWS_ACCESS_KEY}
  secret-key: ${AWS_SECRET_KEY}
  s3:
    bucket-name: game-store-images-2025
```

2. **Set Environment Variables**

```bash
# Windows PowerShell
$env:AWS_ACCESS_KEY="AKIAIOSFODNN7EXAMPLE"
$env:AWS_SECRET_KEY="wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
$env:AWS_REGION="ap-southeast-1"
$env:AWS_S3_BUCKET_NAME="game-store-images-2025"
```

3. **Xác minh S3Service Configuration**

Backend đã có `S3Service.java` với các methods chính:
- `uploadImage(MultipartFile file, String folder)` - Upload image vào folder chỉ định
- `deleteImage(String imageUrl)` - Xóa image theo URL
- `generateFileName(String originalName)` - Tạo filenames unique

**Ví dụ Sử dụng:**
```java
@PostMapping("/games")
public ResponseEntity<GameResponse> createGame(
    @RequestParam("image") MultipartFile imageFile,
    @RequestParam("name") String name
) {
    // Upload image lên S3
    String imageUrl = s3Service.uploadImage(imageFile, "games");
    
    // Lưu game với S3 URL
    Game game = new Game();
    game.setName(name);
    game.setImageUrl(imageUrl);
    gameRepository.save(game);
    
    return ResponseEntity.ok(new GameResponse(game));
}
```

#### Bước 7: Kiểm tra S3 Upload

1. **Khởi động Backend**
```bash
cd Back-End
mvn spring-boot:run
```

2. **Kiểm tra Upload qua Postman**

**Request:**
```http
POST http://localhost:8080/identity/games/image
Content-Type: multipart/form-data

Body:
- Key: "image" | Type: File | Value: (chọn file .jpg hoặc .png)
```

**Response Mong đợi:**
```json
{
    "imageUrl": "https://game-store-images-2025.s3.ap-southeast-1.amazonaws.com/games/20250106-123456-unique-id.jpg"
}
```

3. **Xác minh trong S3 Console**
   - Vào S3 → **game-store-images-2025** → Folder **games/**
   - Bạn sẽ thấy file đã upload

4. **Kiểm tra Public Access**
   - Copy imageUrl từ response
   - Paste vào browser → Hình ảnh sẽ hiển thị
   - Ví dụ: `https://game-store-images-2025.s3.ap-southeast-1.amazonaws.com/games/20250106-123456-unique-id.jpg`

#### Bước 8: Tích hợp Frontend

Frontend sử dụng hàm `uploadImageToS3()` trong `src/api/client.ts`:

```typescript
export const uploadImageToS3 = async (file: File): Promise<string> => {
  const formData = new FormData();
  formData.append('image', file);

  const response = await apiClient.post<ApiResponse<{ imageUrl: string }>>(
    '/games/image',
    formData
    // Để Axios tự động set Content-Type với boundary
  );

  return response.data.result.imageUrl;
};
```

**Sử dụng trong Component:**
```typescript
const handleImageUpload = async (file: File) => {
  try {
    const imageUrl = await uploadImageToS3(file);
    console.log('Image uploaded:', imageUrl);
    // Sử dụng imageUrl trong form của bạn
  } catch (error) {
    console.error('Upload failed:', error);
  }
};
```

#### Giới hạn Kích thước File S3

Backend có validation trong `S3Service.java`:

```java
private static final long MAX_FILE_SIZE = 10 * 1024 * 1024; // 10MB

public String uploadImage(MultipartFile file, String folder) {
    // Validate file size
    if (file.getSize() > MAX_FILE_SIZE) {
        throw new RuntimeException("File size exceeds 10MB limit");
    }
    
    // Validate file type
    String contentType = file.getContentType();
    if (!contentType.startsWith("image/")) {
        throw new RuntimeException("Only image files are allowed");
    }
    
    // ... upload logic
}
```

#### Best Practices cho S3

**Performance:**
- ✅ Sử dụng CloudFront CDN cho image delivery nhanh hơn (tùy chọn)
- ✅ Tối ưu hóa images trước khi upload (resize, compress)
- ✅ Sử dụng image formats phù hợp (WebP cho browsers hiện đại)

**Security:**
- ✅ Không bao giờ expose AWS credentials trong frontend code
- ✅ Sử dụng backend làm proxy cho uploads (đã implement)
- ✅ Validate file types và sizes trên backend
- ✅ Sử dụng signed URLs cho private content (nếu cần)

**Tối ưu Chi phí:**
- ✅ Xóa các files không dùng thường xuyên
- ✅ Sử dụng S3 Lifecycle policies để chuyển old files sang Glacier
- ✅ Enable S3 Intelligent-Tiering cho automatic cost savings

**Ví dụ Lifecycle Policy:**
```json
{
    "Rules": [
        {
            "Id": "Delete temp files after 7 days",
            "Status": "Enabled",
            "Filter": {
                "Prefix": "temp/"
            },
            "Expiration": {
                "Days": 7
            }
        }
    ]
}
```

#### Giám sát S3 Usage

1. **S3 Storage Metrics**
   - S3 Console → **game-store-images-2025** → Tab **Metrics**
   - Xem: Bucket size, số lượng objects, requests

2. **CloudWatch Alarms** (Tùy chọn)
```bash
aws cloudwatch put-metric-alarm \
    --alarm-name s3-bucket-size-alarm \
    --alarm-description "Alert when bucket exceeds 10GB" \
    --metric-name BucketSizeBytes \
    --namespace AWS/S3 \
    --statistic Average \
    --period 86400 \
    --threshold 10737418240 \
    --comparison-operator GreaterThanThreshold
```

3. **Cost Tracking**
   - AWS Billing Dashboard → **Cost Explorer**
   - Filter by Service: **S3**
   - Giám sát storage + request costs

#### Xử lý Sự cố

**Vấn đề: 403 Forbidden khi truy cập images**

✅ Kiểm tra bucket policy cho phép `s3:GetObject`
✅ Xác minh "Block all public access" đã TẮT
✅ Xác nhận định dạng URL: `https://BUCKET.s3.REGION.amazonaws.com/path/file.jpg`

**Vấn đề: CORS error trong frontend**

✅ Xác minh CORS configuration trong S3
✅ Kiểm tra `AllowedOrigins` bao gồm frontend URL của bạn
✅ Xóa browser cache
✅ Kiểm tra browser console cho CORS error cụ thể

**Vấn đề: "Access Denied" khi uploading**

✅ Xác minh IAM user có quyền `s3:PutObject`
✅ Kiểm tra AWS credentials trong environment variables
✅ Xác nhận bucket name khớp trong application.yaml

**Vấn đề: Upload times lâu**

✅ Sử dụng multipart upload cho files >100MB (đã cấu hình trong S3Service)
✅ Nén images trước khi upload
✅ Kiểm tra kết nối mạng

#### Ước tính Chi phí

**S3 Pricing (ap-southeast-1):**

| Component | Usage | Chi phí |
|-----------|-------|---------|
| Storage | 5GB | $0.025/GB = $0.13/tháng |
| PUT requests | 1000 uploads | $0.0047/1000 = $0.0047 |
| GET requests | 10,000 views | $0.0004/1000 = $0.004 |
| Data transfer OUT | 5GB | $0.12/GB = $0.60 |
| **Tổng** | | **~$0.74/tháng** |

{{% notice info %}}
**Free Tier:** 12 tháng đầu bao gồm 5GB storage, 20,000 GET requests, 2,000 PUT requests miễn phí!
{{% /notice %}}

#### Tổng kết

Bạn đã thành công:
- ✅ Tạo S3 bucket với tên unique
- ✅ Cấu hình CORS cho frontend uploads
- ✅ Thiết lập bucket policy cho public read access
- ✅ Tạo IAM user với S3 credentials
- ✅ Cấu hình Spring Boot S3Service
- ✅ Kiểm tra image upload và public access
- ✅ Tích hợp với React frontend

**Bước tiếp theo:** Cấu hình SES (Simple Email Service) cho transactional emails →

{{% notice info %}}
**Thời gian Ước tính:** 15-20 phút cho thiết lập và kiểm tra S3
{{% /notice %}}
