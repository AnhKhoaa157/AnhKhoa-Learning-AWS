---
title: "Workshop"
date: "2025-12-04T07:05:17Z"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---
{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

# Building Devteria Game Store Platform on AWS

#### Overview

This workshop guides you through building a complete **serverless e-commerce platform** for digital game distribution using AWS cloud services. You will learn how to create a scalable, secure, and cost-effective game store with real-time inventory management, automated order processing, and global content delivery.

**What you will build:**
- Serverless backend API with Lambda + API Gateway
- Secure authentication with AWS Cognito (MFA enabled)
- RDS PostgreSQL database for games catalog and orders
- S3 + CloudFront for fast global content delivery
- SQS/SNS for asynchronous order processing
- CloudWatch monitoring and alerts
- CI/CD pipeline with CodePipeline

#### Architecture

![Devteria Architecture](/images/5-Workshop/devteria-architecture.png)

**Request Flow:**
```
User → CloudFront → S3 (React Frontend)
       ↓
User Login → Cognito (MFA) → JWT Token
       ↓
Browse Games → API Gateway → Lambda → RDS (Catalog)
       ↓
Purchase → Lambda → RDS (Order) → SQS (License Queue)
       ↓
SQS Trigger → Lambda (Generate License) → SNS (Email)
       ↓
Download → CloudFront → S3 (Game Files)
```

#### Project Structure

```
devteria-gamestore/
├── frontend/                # React App
│   ├── src/
│   │   ├── components/      # UI components
│   │   ├── pages/           # Pages (Home, Catalog, Cart)
│   │   ├── services/        # API calls
│   │   └── App.js
│   └── package.json
├── backend/                 # Lambda Functions
│   ├── auth/                # Cognito integration
│   ├── catalog/             # Game CRUD
│   ├── orders/              # Order processing
│   ├── licenses/            # License generation
│   └── shared/              # Common utils
├── infrastructure/          # CloudFormation/Terraform
│   ├── network.yml          # VPC, Subnets
│   ├── database.yml         # RDS PostgreSQL
│   ├── storage.yml          # S3, CloudFront
│   ├── compute.yml          # Lambda, API Gateway
│   └── cicd.yml             # CodePipeline
└── buildspec.yml            # CodeBuild config
```

#### Content

1. [Workshop Overview](5.1-Workshop-overview/)
2. [Prerequisites](5.2-Prerequiste/)
3. [Setup Backend Infrastructure](5.3-S3-vpc/)
4. [Build Frontend & Deploy](5.4-S3-onprem/)
5. [Configure CI/CD Pipeline](5.5-Policy/)
6. [Clean up Resources](5.6-Cleanup/)

#### AWS Services Used

| Category | Service | Purpose |
|----------|---------|---------|
| **Compute** | AWS Lambda | Serverless backend functions |
| **API** | API Gateway | RESTful API management |
| **Auth** | Amazon Cognito | User authentication & MFA |
| **Database** | Amazon RDS PostgreSQL | Relational data (games, orders, users) |
| **Storage** | Amazon S3 | Game files & frontend assets |
| **CDN** | Amazon CloudFront | Global content delivery |
| **Queue** | Amazon SQS | Async order processing |
| **Notifications** | Amazon SNS | Email alerts |
| **Monitoring** | Amazon CloudWatch | Logs, metrics, alarms |
| **CI/CD** | CodePipeline, CodeBuild | Automated deployment |
| **Security** | IAM, Secrets Manager | Access control & secrets |

#### Estimated Time & Cost

| Metric | Value |
|--------|-------|
| **Duration** | 4-5 hours |
| **Level** | Intermediate |
| **Cost** | ~$10-15 (during workshop) |
| **Monthly** | ~$50-100 (if kept running) |

{{% notice tip %}}
Use **AWS Free Tier** for Lambda (1M requests), API Gateway (1M calls), S3 (5GB), CloudFront (1TB), and Cognito (50K users) to minimize costs!
{{% /notice %}}

#### Learning Outcomes

After completing this workshop, you will:

✅ Build a production-ready serverless e-commerce platform  
✅ Implement secure JWT authentication with Cognito + MFA  
✅ Design scalable API architecture with Lambda + API Gateway  
✅ Set up global CDN with CloudFront for fast delivery  
✅ Create async workflows with SQS/SNS  
✅ Deploy infrastructure as code with CloudFormation  
✅ Configure CI/CD pipelines for automated deployment  
✅ Monitor applications with CloudWatch  
✅ Optimize costs with serverless architecture  

#### Prerequisites Check

Before starting, ensure you have:
- ✅ AWS Account with admin access
- ✅ AWS CLI installed & configured
- ✅ Node.js 18+ (for React frontend)
- ✅ Git installed
- ✅ Basic knowledge of JavaScript/React
- ✅ Understanding of REST APIs
- ✅ Familiarity with SQL (PostgreSQL)

Ready? Let's build! 🚀












