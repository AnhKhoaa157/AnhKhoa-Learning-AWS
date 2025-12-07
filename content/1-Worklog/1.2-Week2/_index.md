---
title: "Week 2 Worklog"
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}


### Week 2 Objectives:

* Learn about AWS storage service (S3) and basic concepts.
* Understand Virtual Private Cloud (VPC) for networking.
* Study Identity and Access Management (IAM) for security.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Create S3 bucket to host static website <br> - Upload HTML/CSS demo files to S3 | 15/09/2025 | 15/09/2025      | AWS Journey |
| 3   | - Enable Static Website Hosting feature in S3 <br> - Configure bucket policy for public read access <br> - Test website access via S3 link | 16/09/2025 | 16/09/2025      | AWS Journey |
| 4   | - Create RDS MySQL instance (Free Tier) <br> - Configure VPC security group to allow DB connection <br> - Note endpoint & credentials | 17/09/2025 | 17/09/2025      | AWS Journey |
| 5   | - Create EC2 instance, install MySQL client <br> - Connect from EC2 to RDS via command line <br> - Test creating database & simple table | 18/09/2025 | 18/09/2025      | AWS Journey |
| 6   | - Learn about Route53, create hosted zone <br> - Add A/CNAME records to point domain to S3 static site <br> - Test website access via domain | 19/09/2025 | 19/09/2025      | AWS Journey |


### Week 2 Achievements:

* **Learned about Amazon S3 storage:**
  * Understood S3 as object storage service for files and data
  * Learned about different storage classes:
    * S3 Standard - for frequently accessed data
    * S3 Glacier - for archival and backup
  * Studied bucket policies and access control
  * Learned about versioning for data protection

* **Practiced with S3:**
  * Created S3 buckets with unique names
  * Uploaded and downloaded files using Console and CLI:
    * `aws s3 cp file.txt s3://my-bucket/`
    * `aws s3 ls s3://my-bucket/`
  * Configured bucket policies for access control
  * Enabled versioning on buckets
  * Set up simple static website hosting
  * Tested file management operations

* **Understood VPC networking basics:**
  * Learned VPC as virtual network in AWS
  * Studied CIDR blocks and IP addressing
  * Understood difference between public and private subnets:
    * Public subnet - has internet access
    * Private subnet - no direct internet access
  * Learned about VPC components:
    * Internet Gateway - for internet connectivity
    * Route Tables - for traffic routing
    * Security Groups - instance-level firewall
    * NACLs - subnet-level firewall

* **Practiced with VPC:**
  * Created custom VPC with CIDR block
  * Created public and private subnets
  * Configured route tables for subnets
  * Attached Internet Gateway to VPC
  * Launched EC2 instance in custom VPC
  * Configured Security Groups for access control
  * Tested network connectivity

* **Learned IAM security basics:**
  * Understood IAM for access management
  * Learned about IAM components:
    * Users - individual accounts
    * Groups - collections of users
    * Roles - for AWS services
    * Policies - permission definitions
  * Studied IAM best practices:
    * Use MFA for security
    * Follow least privilege principle
    * Create separate users instead of using root

* **Practiced with IAM:**
  * Created IAM users and groups
  * Attached policies to users and groups
  * Created custom policies for specific permissions
  * Set up IAM roles for EC2
  * Enabled MFA for users
  * Tested user permissions and access

* **Gained practical AWS skills:**
  * Can manage storage with S3
  * Understand basic networking with VPC
  * Know how to control access with IAM
  * Ready for more advanced topics in Week 3