---
title: "Building Full-Stack Game Store"
date: "2025-12-06T00:00:00Z"
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

#### Overview

In this workshop, you will build a complete **full-stack e-commerce platform** for digital game distribution using AWS cloud services. This is a real-world production-ready application that demonstrates integration of multiple AWS services with Spring Boot backend and React frontend.

**What you will build:**
- Spring Boot REST API backend with JWT authentication
- React SPA frontend with modern UI/UX
- AWS RDS MySQL database for persistent storage
- AWS S3 for game images and user avatars
- AWS SES for email notifications
- Payment integration (MoMo, VNPay)
- OAuth2 Google authentication
- Multi-role authorization (ADMIN, USER, MOD)

#### Architecture

![Game Store Architecture](/images/5-Workshop/5.7-GameStore/architecture.png)

**Technology Stack:**
```
Frontend: React 18 + TypeScript + Vite + Axios
Backend: Spring Boot 3.4 + Spring Security + JWT
Database: AWS RDS MySQL 8.0
Storage: AWS S3 (ap-southeast-1)
Email: AWS SES
Hosting: AWS EC2 (Ubuntu)
Payment: MoMo & VNPay Gateway
```

**Request Flow:**
```
User → React App (CloudFront/S3)
       ↓
Login → Spring Security → JWT Token
       ↓
Browse Games → REST API → RDS (Games Catalog)
       ↓
Purchase → Payment Gateway (MoMo/VNPay)
       ↓
Order Confirmed → Lambda (Generate Key) → SES (Email)
       ↓
Download → S3 (CloudFront Distribution)
```

#### Features

**User Features:**
- Browse game catalog with filters and search
- View detailed game information with screenshots
- Add games to cart and wishlist
- Secure checkout with multiple payment methods
- User profile management with avatar upload
- Wallet top-up via MoMo
- Order history and transaction tracking
- Email notifications for orders

**Admin Features:**
- Game management (CRUD operations)
- User management with role assignment
- Order tracking and analytics
- Category management
- Dashboard with statistics
- Bulk operations

**Security Features:**
- JWT-based authentication
- Role-based access control (RBAC)
- Password encryption (BCrypt)
- CORS configuration
- SQL injection prevention
- XSS protection
- CSRF protection

#### Workshop Content

This workshop is divided into the following sections:

1. [**Prerequisites & Setup**](5.7.1-prerequisites/) - AWS account, tools installation
2. [**Database Setup (RDS)**](5.7.2-database/) - Create RDS MySQL instance
3. [**Storage Setup (S3)**](5.7.3-storage/) - Configure S3 bucket for images
4. [**Email Service (SES)**](5.7.4-email/) - Setup AWS SES for notifications
5. [**Backend Development**](5.7.5-backend/) - Deploy Spring Boot API
6. [**Frontend Development**](5.7.6-frontend/) - Deploy React application
7. [**Payment Integration**](5.7.7-payment/) - Setup MoMo & VNPay
8. [**Security Configuration**](5.7.8-security/) - Implement authentication
9. [**Testing & Validation**](5.7.9-testing/) - End-to-end testing
10. [**Cleanup**](5.7.10-cleanup/) - Remove AWS resources

#### Estimated Time
- **Total Duration:** 4-6 hours
- **Cost:** $5-10 for the workshop (assuming you clean up resources afterwards)

#### Learning Objectives

By the end of this workshop, you will:
- ✅ Understand AWS RDS for relational databases
- ✅ Learn S3 for object storage and CloudFront for CDN
- ✅ Implement AWS SES for transactional emails
- ✅ Deploy Spring Boot applications on EC2
- ✅ Configure security with Spring Security and JWT
- ✅ Integrate payment gateways (MoMo, VNPay)
- ✅ Build production-ready REST APIs
- ✅ Create responsive React frontends
- ✅ Implement role-based access control
- ✅ Handle file uploads to S3
- ✅ Manage environment variables and secrets

{{% notice info %}}
This workshop uses real AWS resources which will incur costs. Make sure to follow the cleanup instructions at the end to avoid unexpected charges.
{{% /notice %}}

{{% notice warning %}}
**Production Considerations:** This workshop focuses on core functionality. For production deployment, consider adding:
- Amazon CloudWatch for monitoring
- AWS Secrets Manager for credentials
- Amazon CloudFront for global CDN
- AWS Lambda for serverless functions
- Amazon SQS for message queuing
- AWS WAF for web application firewall
- Auto Scaling for high availability
{{% /notice %}}

#### Let's get started! →
