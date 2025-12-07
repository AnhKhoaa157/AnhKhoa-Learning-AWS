---
title: "Week 1 Worklog"
date: "2025-12-04T07:05:17Z"
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---
{{% notice warning %}} 
 **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}

##  Phase 1: AWS Foundation - Week 1

###  Week 1 Objectives:

**Introduction & Environment Setup**

* Understand Cloud computing model, Regions, Availability Zones (AZ)
* Create and configure AWS account
* Get familiar with AWS Management Console and AWS CLI
* Manage costs with Free Tier and Budget Alerts

###  Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 1 | - Connect with FCJ members<br>- Understand bootcamp structure & expectations<br>- Read internship guidelines | 09/08/2025 | 09/08/2025 | [FCJ Community](https://cloudjourney.awsstudygroup.com/) |
| 2 | - Learn Cloud Computing fundamentals:<br>&emsp;+ Cloud deployment models (Public, Private, Hybrid)<br>&emsp;+ Service models (IaaS, PaaS, SaaS)<br>&emsp;+ AWS Global Infrastructure (Regions, AZs, Edge Locations)<br>- Understand AWS Free Tier benefits & limitations | 09/09/2025 | 09/09/2025 | [AWS Cloud Essentials](https://aws.amazon.com/getting-started/cloud-essentials/)<br>[AWS Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/) |
| 3 | - Create AWS Free Tier account<br>- Enable MFA for root account<br>- Set up Billing Alerts and AWS Budgets<br>- Navigate AWS Management Console | 09/10/2025 | 09/10/2025 | [AWS Free Tier](https://aws.amazon.com/free/)<br>[AWS Budgets](https://aws.amazon.com/aws-cost-management/aws-budgets/) |
| 4 | - Install & configure AWS CLI v2:<br>&emsp;+ Create IAM user with programmatic access<br>&emsp;+ Configure AWS credentials<br>&emsp;+ Test basic CLI commands<br>- Learn AWS CLI syntax and help system | 09/11/2025 | 09/11/2025 | [AWS CLI Guide](https://docs.aws.amazon.com/cli/latest/userguide/)<br>[IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) |
| 5 | **Practice: Hands-on with S3**<br>- Create S3 bucket via Console<br>- Upload/download files<br>- Configure bucket permissions<br>- Enable versioning<br>- Test S3 operations via CLI | 09/12/2025 | 09/12/2025 | [S3 Getting Started](https://docs.aws.amazon.com/AmazonS3/latest/userguide/GetStartedWithS3.html) |

###  Week 1 Achievements:

**Cloud Computing Foundation:**
*  Understood core cloud concepts: elasticity, scalability, pay-as-you-go pricing
*  Learned AWS Global Infrastructure:
  * 33+ Regions worldwide
  * 105+ Availability Zones
  * 600+ Edge Locations (CloudFront CDN)
*  Compared IaaS vs PaaS vs SaaS with real-world examples

**AWS Account Setup:**
*  Successfully created AWS Free Tier account
*  Enabled Multi-Factor Authentication (MFA) for root user security
*  Set up billing alerts for $10 threshold to avoid unexpected charges
*  Created Budget with $20 monthly limit + email notifications

**AWS Console Navigation:**
*  Mastered AWS Management Console interface
*  Understood service categories: Compute, Storage, Database, Networking, Security
*  Used search and favorites features for quick access
*  Explored AWS Free Tier dashboard

**AWS CLI Configuration:**
*  Installed AWS CLI v2 on local machine
*  Created IAM user `fcj-cli-user` with programmatic access
*  Configured credentials using `aws configure`:
  * Access Key ID
  * Secret Access Key
  * Default region: `ap-southeast-1` (Singapore)
  * Default output format: `json`
*  Tested CLI with basic commands:
  ```bash
  aws sts get-caller-identity
  aws ec2 describe-regions
  aws s3 ls
  ```

**S3 Hands-on Practice:**
*  Created first S3 bucket: `fcj-bootcamp-week1-[random-id]`
*  Uploaded test files via Console and CLI
*  Configured bucket versioning for file history
*  Set bucket policies for public/private access
*  Successfully performed S3 operations:
  ```bash
  aws s3 cp test.txt s3://my-bucket/
  aws s3 ls s3://my-bucket/
  aws s3 sync ./local-folder s3://my-bucket/backup/
  ```

**Key Learnings:**
*  Always enable MFA on root account - security first!
*  Free Tier covers 750 hours/month of EC2 t2.micro and 5GB S3 storage
*  Budget alerts are essential to avoid surprise bills
*  IAM users for daily work, never use root account
*  S3 bucket names must be globally unique across ALL AWS accounts

###  Next Week Preview (Week 2):
* EC2 instances and compute fundamentals
* VPC, Subnets, Security Groups
* Deploy first web server on AWS

