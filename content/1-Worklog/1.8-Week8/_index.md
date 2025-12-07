---
title: "Week 8 Worklog"
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}


### Week 8 Objectives:

* Learn about containers and Docker basics.
* Understand Amazon ECS and container orchestration.
* Deploy containerized applications on AWS.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Overview of AWS Well-Architected Framework, 5 pillars: Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization <br> - Identify the role and importance of each pillar in system design                                                                                                                | 27/10/2025 | 27/10/2025      | AWS Journey                                    |
| 3   | - Review Secure Architecture Design <br> → IAM, MFA, SCP, Encryption (KMS, TLS/ACM), Security Groups, NACLs, GuardDuty, Shield, WAF, Secrets Manager                                                                                 | 28/10/2025 | 28/10/2025      | AWS Journey                                   |
| 4   | - Review Resilient Architecture Design <br> → Multi-AZ, Multi-Region, DR Strategies, Auto Scaling, Route 53, Load Balancing, Backup & Restore                                                                               | 29/10/2025 | 29/10/2025      | AWS Journey                                    |
| 5   | - Review Performance and Cost Optimization (High-Performing & Cost-Optimized Architectures) <br> → EC2 Auto Scaling, Lambda, Fargate, CloudFront, Global Accelerator, Cost Explorer, Budgets, Savings Plans, Storage Tiering                                                              | 30/10/2025 | 30/10/2025      | AWS Journey                        |
| 6   | - Comprehensive Practice: <br> + Build a sample architecture combining EC2, S3, RDS, IAM, VPC, CloudFront, Lambda, CloudWatch <br> + Evaluate according to 5 Well-Architected Framework criteria <br> + Write weekly summary report                                                               | 31/10/2025 | 31/10/2025      | AWS Journey                      |


### Week 8 Achievements:

* Learned about containerization:
  * Understood benefits of containers vs VMs
  * Learned Docker basics and terminology
  * Studied container use cases

* Practiced with Docker locally:
  * Installed Docker Desktop
  * Pulled images from Docker Hub (nginx, node, python)
  * Created simple Dockerfile:
    ```dockerfile
    FROM node:14
    WORKDIR /app
    COPY package*.json ./
    RUN npm install
    COPY . .
    EXPOSE 3000
    CMD ["npm", "start"]
    ```
  * Built and ran containers locally

* Understood Amazon ECS:
  * Learned ECS architecture and components
  * Compared Fargate (serverless) vs EC2 launch types
  * Studied task definitions and services

* Worked with ECR:
  * Created ECR repository
  * Tagged and pushed Docker image to ECR
  * Configured authentication with ECR

* Deployed on ECS:
  * Created ECS cluster
  * Created task definition with container settings
  * Deployed service on Fargate
  * Configured networking and security groups
  * Accessed application via public IP
  * Viewed container logs in CloudWatch