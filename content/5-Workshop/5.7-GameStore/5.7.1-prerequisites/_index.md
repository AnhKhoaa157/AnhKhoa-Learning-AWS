---
title: "Prerequisites & Setup"
date: "2025-12-06T00:00:00Z"
weight: 1
chapter: false
pre: " <b> 5.7.1. </b> "
---

#### Prerequisites

Before starting this workshop, ensure you have:

**AWS Account Requirements:**
- An active AWS account with administrator access
- Familiarity with AWS Console
- Understanding of basic cloud concepts

**Development Tools:**
- **Java Development Kit (JDK) 17+**
  ```bash
  java -version
  # Should show: java version "17" or higher
  ```

- **Node.js 18+ and npm**
  ```bash
  node --version  # v18.0.0 or higher
  npm --version   # v9.0.0 or higher
  ```

- **Maven 3.8+**
  ```bash
  mvn --version
  ```

- **Git**
  ```bash
  git --version
  ```

- **VS Code or IntelliJ IDEA** (recommended)

**AWS CLI (Optional but recommended):**
```bash
# Install AWS CLI
pip install awscli --upgrade --user

# Configure AWS CLI
aws configure
# AWS Access Key ID: YOUR_ACCESS_KEY
# AWS Secret Access Key: YOUR_SECRET_KEY
# Default region: ap-southeast-1
# Default output format: json
```

#### IAM Permissions

Create an IAM user with the following permissions:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "rds:*",
                "s3:*",
                "ses:*",
                "ec2:*",
                "iam:PassRole",
                "cloudformation:*",
                "cloudwatch:*",
                "logs:*"
            ],
            "Resource": "*"
        }
    ]
}
```

**Steps to create IAM user:**

1. Go to **IAM Console** → **Users** → **Add users**
2. User name: `game-store-workshop`
3. Access type: ✅ **Programmatic access** + ✅ **AWS Management Console access**
4. Permissions: **Attach existing policies directly**
   - `AmazonRDSFullAccess`
   - `AmazonS3FullAccess`
   - `AmazonSESFullAccess`
   - `AmazonEC2FullAccess`
5. **Download credentials CSV** - Save securely!

#### Cost Estimation

**AWS Resources Costs (ap-southeast-1 region):**

| Service | Configuration | Estimated Cost |
|---------|--------------|----------------|
| RDS MySQL | db.t3.micro, 20GB | ~$15/month |
| S3 Storage | 5GB storage + requests | ~$0.50/month |
| EC2 Instance | t2.micro (Free Tier) | Free/$8/month |
| SES | 1000 emails | Free (within limits) |
| Data Transfer | 10GB outbound | ~$1/month |
| **Total** | | **~$16-25/month** |

{{% notice tip %}}
**Cost Savings:**
- Use **Free Tier** eligible resources (RDS db.t3.micro for 12 months)
- Stop EC2 instances when not in use
- Delete resources after workshop completion
- Use **RDS Single-AZ** for development (not production)
{{% /notice %}}

#### Clone Repository

```bash
# Clone the game store repository
git clone https://github.com/AnhKhoaa157/Workshop-AWS.git
cd Workshop-AWS

# Check structure
ls -la
# You should see:
# - Back-End/    (Spring Boot application)
# - Front-End/   (React application)
# - README.md
```

#### Verify Installation

Run these commands to verify your setup:

```bash
# Check Java
java -version

# Check Maven
mvn --version

# Check Node.js
node --version

# Check npm
npm --version

# Check Git
git --version

# Check AWS CLI (optional)
aws --version
```

#### AWS Region Selection

**Recommended Region:** `ap-southeast-1` (Singapore)

**Reasons:**
- ✅ Low latency for Southeast Asia users
- ✅ Full service availability (RDS, S3, SES, EC2)
- ✅ Competitive pricing
- ✅ Good documentation and support

**To set region:**
```bash
# Via AWS CLI
export AWS_DEFAULT_REGION=ap-southeast-1

# Or in AWS Console
# Top-right corner → Select "Asia Pacific (Singapore) ap-southeast-1"
```

#### Project Structure Overview

```
Workshop-AWS/
├── Back-End/                    # Spring Boot Backend
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/
│   │   │   │   └── com/se182393/baidautien/
│   │   │   │       ├── controller/      # REST Controllers
│   │   │   │       ├── service/         # Business Logic
│   │   │   │       ├── repository/      # Data Access
│   │   │   │       ├── entity/          # JPA Entities
│   │   │   │       ├── dto/             # Data Transfer Objects
│   │   │   │       ├── configuration/   # Spring Config
│   │   │   │       └── enums/           # Enums (Role, etc)
│   │   │   └── resources/
│   │   │       ├── application.yaml     # Local config
│   │   │       └── application-ec2.yaml # Production config
│   │   └── test/                        # Unit tests
│   ├── pom.xml                          # Maven dependencies
│   └── README.md
│
├── Front-End/                   # React Frontend
│   ├── src/
│   │   ├── api/                 # API client
│   │   ├── components/          # React components
│   │   ├── pages/               # Page components
│   │   ├── context/             # React Context
│   │   ├── utils/               # Utility functions
│   │   └── styles/              # CSS files
│   ├── package.json
│   ├── vite.config.ts
│   └── README.md
│
└── README.md                    # Project documentation
```

#### Architecture Overview

```
┌─────────────┐
│   User      │
└──────┬──────┘
       │
       ├──────────────────┐
       │                  │
       ▼                  ▼
┌─────────────┐    ┌─────────────┐
│   React     │    │  Spring     │
│   Frontend  │◄───┤  Boot API   │
└─────────────┘    └──────┬──────┘
                          │
                ┌─────────┼─────────┐
                │         │         │
                ▼         ▼         ▼
         ┌──────────┐ ┌─────┐ ┌─────┐
         │ RDS MySQL│ │ S3  │ │ SES │
         └──────────┘ └─────┘ └─────┘
```

**Components:**
1. **Frontend (React):** User interface hosted on S3/EC2
2. **Backend (Spring Boot):** REST API for business logic
3. **Database (RDS):** MySQL for persistent data storage
4. **Storage (S3):** Images, avatars, game files
5. **Email (SES):** Transactional emails (orders, verification)

#### Environment Setup

Create environment files for configuration:

**Backend `.env` file:**
```bash
# Navigate to Back-End directory
cd Back-End

# Create .env file
cat > .env << 'EOF'
# Database
RDS_ENDPOINT=your-rds-endpoint.rds.amazonaws.com
RDS_PORT=3306
RDS_DATABASE=identity_service
RDS_USERNAME=admin
RDS_PASSWORD=your-password

# AWS S3
AWS_REGION=ap-southeast-1
AWS_ACCESS_KEY=YOUR_ACCESS_KEY
AWS_SECRET_KEY=YOUR_SECRET_KEY
AWS_S3_BUCKET_NAME=game-store-images-2025

# AWS SES
AWS_SES_FROM_EMAIL=your-verified-email@example.com

# JWT
JWT_SIGNER_KEY=your-secret-key-min-32-chars

# Email
MAIL_USERNAME=your-email@gmail.com
MAIL_PASSWORD=your-app-password

# MoMo Payment
MOMO_PARTNER_CODE=MOMOLRJZ20181206
MOMO_ACCESS_KEY=mTCKt9W3eU1m39TW
MOMO_SECRET_KEY=SetA5RDnLHvt51AULf51DyauxUo3kDU6
EOF
```

**Frontend `.env` file:**
```bash
# Navigate to Front-End directory
cd Front-End

# Create .env file
cat > .env << 'EOF'
VITE_API_URL=http://localhost:8080/identity
VITE_GOOGLE_CLIENT_ID=your-google-oauth-client-id
EOF
```

{{% notice warning %}}
**Security:** Never commit `.env` files to Git! Add them to `.gitignore`.
{{% /notice %}}

#### Ready to Start!

Now that you have:
- ✅ AWS account setup
- ✅ Development tools installed
- ✅ Repository cloned
- ✅ Environment configured

You're ready to proceed to the next section: **Database Setup (RDS)** →

{{% notice info %}}
**Estimated Time:** 30-45 minutes to complete all prerequisites
{{% /notice %}}
