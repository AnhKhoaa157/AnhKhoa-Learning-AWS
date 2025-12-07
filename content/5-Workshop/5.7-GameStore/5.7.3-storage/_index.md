---
title: "Storage Setup (S3)"
date: "2025-12-06T00:00:00Z"
weight: 3
chapter: false
pre: " <b> 5.7.3. </b> "
---

#### Overview

In this section, we'll configure Amazon S3 (Simple Storage Service) to store game images, avatars, and other media files. S3 provides scalable, durable, and highly available object storage.

**What we'll configure:**
- S3 bucket creation with appropriate naming
- CORS configuration for frontend access
- Bucket policy for public read access
- IAM user credentials for backend
- Integration with Spring Boot S3Service

#### Architecture

```
┌──────────────┐         ┌──────────────┐         ┌──────────────┐
│   React      │────────►│  Spring Boot │────────►│   Amazon S3  │
│   Frontend   │  POST   │   S3Service  │  SDK v2 │    Bucket    │
└──────────────┘  image  └──────────────┘         └──────────────┘
       │                                                   │
       └───────────────────GET image──────────────────────┘
                      (public URL)
```

#### Step 1: Create S3 Bucket

1. **Navigate to S3 Console**
   - Go to: https://s3.console.aws.amazon.com/
   - Region: **ap-southeast-1** (Singapore)

2. **Create Bucket**
   - Click **Create bucket**
   - Bucket name: `game-store-images-2025` (must be globally unique!)
   - AWS Region: **Asia Pacific (Singapore) ap-southeast-1**

{{% notice tip %}}
**Bucket Naming:** S3 bucket names must be globally unique across all AWS accounts. If `game-store-images-2025` is taken, try: `game-store-images-yourname-2025` or `game-store-images-randomnumber`
{{% /notice %}}

3. **Object Ownership**
   - ACLs disabled (recommended)
   - Bucket owner enforced

4. **Block Public Access**
   - ☐ **Uncheck** "Block all public access"
   - ☑️ Acknowledge warning: "I acknowledge that the current settings might result in this bucket and the objects within becoming public"

{{% notice warning %}}
**Public Access:** We're allowing public read access only for image files that need to be displayed on the website. Use bucket policies to control access precisely.
{{% /notice %}}

5. **Bucket Versioning**
   - Disable (for cost savings)

6. **Default encryption**
   - Encryption type: **Server-side encryption with Amazon S3 managed keys (SSE-S3)**
   - ☑️ Enable

7. **Advanced settings**
   - Object Lock: **Disable**

8. Click **Create bucket**

#### Step 2: Configure CORS

CORS (Cross-Origin Resource Sharing) allows your React frontend to upload files directly to S3.

1. **Open Bucket Settings**
   - S3 Console → Buckets → Click **game-store-images-2025**
   - Tab: **Permissions**

2. **Edit CORS Configuration**
   - Scroll to **Cross-origin resource sharing (CORS)**
   - Click **Edit**

3. **Add CORS Rules**

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
**CORS Origins:** Update `AllowedOrigins` to include your production domain when deploying. For development, `http://localhost:5173` (Vite) and `http://localhost:3000` (create-react-app) are sufficient.
{{% /notice %}}

#### Step 3: Set Bucket Policy for Public Read

Bucket policies control access permissions at the bucket level.

1. **Edit Bucket Policy**
   - S3 Console → **game-store-images-2025** → **Permissions** tab
   - Scroll to **Bucket policy**
   - Click **Edit**

2. **Add Policy**

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
**Replace bucket name:** Change `game-store-images-2025` to your actual bucket name in the ARN.
{{% /notice %}}

3. Click **Save changes**

**What this policy does:**
- Allows anyone (`Principal: "*"`) to read (`s3:GetObject`) all objects in the bucket
- Upload/delete operations still require authentication (handled by backend)
- Only GET requests are public - perfect for displaying images

#### Step 4: Create Folder Structure

Organize files with a logical folder structure:

1. **Create Folders**
   - S3 Console → **game-store-images-2025** → **Create folder**
   
   Create the following folders:
   - `games/` - Game cover images
   - `avatars/` - User avatar images
   - `categories/` - Category icons
   - `temp/` - Temporary uploads

2. **Folder Structure**
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

#### Step 5: Create IAM User for Backend

Your Spring Boot backend needs AWS credentials to upload files to S3.

1. **Navigate to IAM Console**
   - Go to: https://console.aws.amazon.com/iam/
   - Left menu → **Users** → **Add users**

2. **Create User**
   - User name: `game-store-s3-user`
   - Access type: ☑️ **Programmatic access** (access key)
   - Click **Next: Permissions**

3. **Attach Policies**
   - **Attach existing policies directly**
   - Search and select: `AmazonS3FullAccess`
   - Click **Next: Tags** → **Next: Review**

{{% notice tip %}}
**Least Privilege:** For production, create a custom policy that only allows access to your specific bucket instead of `AmazonS3FullAccess`.
{{% /notice %}}

**Custom Policy Example (Production):**
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

4. **Create User and Save Credentials**
   - Click **Create user**
   - ⚠️ **Download CSV** or copy credentials:
     - Access key ID: `AKIAIOSFODNN7EXAMPLE`
     - Secret access key: `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`

{{% notice warning %}}
**Security:** Store credentials securely! Never commit them to Git. This is your only chance to view the secret access key.
{{% /notice %}}

#### Step 6: Configure Backend (Spring Boot)

1. **Update `application.yaml`**

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

3. **Verify S3Service Configuration**

Backend already has `S3Service.java` with these key methods:
- `uploadImage(MultipartFile file, String folder)` - Upload image to specified folder
- `deleteImage(String imageUrl)` - Delete image by URL
- `generateFileName(String originalName)` - Generate unique filenames

**Example Usage:**
```java
@PostMapping("/games")
public ResponseEntity<GameResponse> createGame(
    @RequestParam("image") MultipartFile imageFile,
    @RequestParam("name") String name
) {
    // Upload image to S3
    String imageUrl = s3Service.uploadImage(imageFile, "games");
    
    // Save game with S3 URL
    Game game = new Game();
    game.setName(name);
    game.setImageUrl(imageUrl);
    gameRepository.save(game);
    
    return ResponseEntity.ok(new GameResponse(game));
}
```

#### Step 7: Test S3 Upload

1. **Start Backend**
```bash
cd Back-End
mvn spring-boot:run
```

2. **Test Upload via Postman**

**Request:**
```http
POST http://localhost:8080/identity/games/image
Content-Type: multipart/form-data

Body:
- Key: "image" | Type: File | Value: (select a .jpg or .png file)
```

**Expected Response:**
```json
{
    "imageUrl": "https://game-store-images-2025.s3.ap-southeast-1.amazonaws.com/games/20250106-123456-unique-id.jpg"
}
```

3. **Verify in S3 Console**
   - Go to S3 → **game-store-images-2025** → **games/** folder
   - You should see the uploaded file

4. **Test Public Access**
   - Copy the imageUrl from response
   - Paste in browser → Image should display
   - Example: `https://game-store-images-2025.s3.ap-southeast-1.amazonaws.com/games/20250106-123456-unique-id.jpg`

#### Step 8: Frontend Integration

The frontend uses `uploadImageToS3()` function in `src/api/client.ts`:

```typescript
export const uploadImageToS3 = async (file: File): Promise<string> => {
  const formData = new FormData();
  formData.append('image', file);

  const response = await apiClient.post<ApiResponse<{ imageUrl: string }>>(
    '/games/image',
    formData
    // Let Axios auto-set Content-Type with boundary
  );

  return response.data.result.imageUrl;
};
```

**Usage in Component:**
```typescript
const handleImageUpload = async (file: File) => {
  try {
    const imageUrl = await uploadImageToS3(file);
    console.log('Image uploaded:', imageUrl);
    // Use imageUrl in your form
  } catch (error) {
    console.error('Upload failed:', error);
  }
};
```

#### S3 File Size Limits

Backend has validation in `S3Service.java`:

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

#### S3 Best Practices

**Performance:**
- ✅ Use CloudFront CDN for faster image delivery (optional)
- ✅ Optimize images before upload (resize, compress)
- ✅ Use appropriate image formats (WebP for modern browsers)

**Security:**
- ✅ Never expose AWS credentials in frontend code
- ✅ Use backend as proxy for uploads (already implemented)
- ✅ Validate file types and sizes on backend
- ✅ Use signed URLs for private content (if needed)

**Cost Optimization:**
- ✅ Delete unused files regularly
- ✅ Use S3 Lifecycle policies to move old files to Glacier
- ✅ Enable S3 Intelligent-Tiering for automatic cost savings

**Lifecycle Policy Example:**
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

#### Monitoring S3 Usage

1. **S3 Storage Metrics**
   - S3 Console → **game-store-images-2025** → **Metrics** tab
   - View: Bucket size, number of objects, requests

2. **CloudWatch Alarms** (Optional)
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
   - Monitor storage + request costs

#### Troubleshooting

**Issue: 403 Forbidden when accessing images**

✅ Check bucket policy allows `s3:GetObject`
✅ Verify "Block all public access" is OFF
✅ Confirm URL format: `https://BUCKET.s3.REGION.amazonaws.com/path/file.jpg`

**Issue: CORS error in frontend**

✅ Verify CORS configuration in S3
✅ Check `AllowedOrigins` includes your frontend URL
✅ Clear browser cache
✅ Check browser console for specific CORS error

**Issue: "Access Denied" when uploading**

✅ Verify IAM user has `s3:PutObject` permission
✅ Check AWS credentials in environment variables
✅ Confirm bucket name matches in application.yaml

**Issue: Large upload times**

✅ Use multipart upload for files >100MB (already configured in S3Service)
✅ Compress images before upload
✅ Check network connection

#### Cost Estimate

**S3 Pricing (ap-southeast-1):**

| Component | Usage | Cost |
|-----------|-------|------|
| Storage | 5GB | $0.025/GB = $0.13/month |
| PUT requests | 1000 uploads | $0.0047/1000 = $0.0047 |
| GET requests | 10,000 views | $0.0004/1000 = $0.004 |
| Data transfer OUT | 5GB | $0.12/GB = $0.60 |
| **Total** | | **~$0.74/month** |

{{% notice info %}}
**Free Tier:** First 12 months include 5GB storage, 20,000 GET requests, 2,000 PUT requests free!
{{% /notice %}}

#### Summary

You have successfully:
- ✅ Created S3 bucket with unique name
- ✅ Configured CORS for frontend uploads
- ✅ Set bucket policy for public read access
- ✅ Created IAM user with S3 credentials
- ✅ Configured Spring Boot S3Service
- ✅ Tested image upload and public access
- ✅ Integrated with React frontend

**Next Step:** Configure SES (Simple Email Service) for transactional emails →

{{% notice info %}}
**Estimated Time:** 15-20 minutes for S3 setup and testing
{{% /notice %}}
