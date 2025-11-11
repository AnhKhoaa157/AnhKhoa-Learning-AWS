------

title: "Proposal"title: "Proposal"

date: "2025-11-11T02:45:23Z"date: "2025-11-11T02:45:23Z"

weight: 2weight: 2

chapter: falsechapter: false

pre: " <b> 2. </b> "pre: " <b> 2. </b> "

------

{{% notice warning %}}{{% notice warning %}}

⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.

{{% /notice %}}{{% /notice %}}



# Devteria Game Store PlatformIn this section, you need to summarize the contents of the workshop that you **plan** to conduct.

## AWS Cloud-Based E-commerce Solution for Digital Game Distribution

# Devteria Game Store Platform

### 1. Executive Summary## A Scalable AWS Cloud Solution for E-commerce Gaming Platform

Devteria Game Store is a scalable e-commerce platform for purchasing and managing digital game licenses. Built on AWS cloud infrastructure, it provides secure authentication, real-time inventory management, automated order processing, and global content delivery. The platform supports thousands of concurrent users while maintaining high availability (99.9% uptime) and cost efficiency through serverless architecture and auto-scaling capabilities.

### 1. Executive Summary

### 2. Problem StatementDevteria Game Store is a modern e-commerce platform designed to provide gamers with a seamless experience for purchasing, downloading, and managing digital game licenses. The platform leverages AWS cloud services to deliver high availability, scalability, and security. Built with a microservices architecture, it supports features such as user authentication, game catalog management, payment processing, order tracking, and automated notifications. The solution is designed to handle thousands of concurrent users while maintaining optimal performance and cost efficiency.



#### Current Challenges### 2. Problem Statement

- Traditional game stores struggle with traffic spikes during launches### What’s the Problem?

- Complex authentication and payment flows reduce conversion rates  Current weather stations require manual data collection, becoming unmanageable with multiple units. There is no centralized system for real-time data or analytics, and third-party platforms are costly and overly complex.

- Manual inventory management leads to overselling

- Lack of real-time analytics for business decisions### The Solution

- High infrastructure costs for peak capacityThe platform uses AWS IoT Core to ingest MQTT data, AWS Lambda and API Gateway for processing, Amazon S3 for storage (including a data lake), and AWS Glue Crawlers and ETL jobs to extract, transform, and load data from the S3 data lake to another S3 bucket for analysis. AWS Amplify with Next.js provides the web interface, and Amazon Cognito ensures secure access. Similar to Thingsboard and CoreIoT, users can register new devices and manage connections, though this platform operates on a smaller scale and is designed for private use. Key features include real-time dashboards, trend analysis, and low operational costs.



#### Our Solution### Benefits and Return on Investment

Devteria leverages AWS managed services to create a modern, scalable platform:The solution establishes a foundational resource for lab members to develop a larger IoT platform, serving as a study resource, and provides a data foundation for AI enthusiasts for model training or analysis. It reduces manual reporting for each station via a centralized platform, simplifying management and maintenance, and improves data reliability. Monthly costs are $0.66 USD per the AWS Pricing Calculator, with a 12-month total of $7.92 USD. All IoT equipment costs are covered by the existing weather station setup, eliminating additional development expenses. The break-even period of 6-12 months is achieved through significant time savings from reduced manual work.

- **CloudFront + S3**: Fast global content delivery

- **Cognito**: Secure user authentication with social login### 3. Solution Architecture

- **API Gateway + Lambda**: Serverless auto-scaling backendThe platform employs a serverless AWS architecture to manage data from 5 Raspberry Pi-based stations, scalable to 15. Data is ingested via AWS IoT Core, stored in an S3 data lake, and processed by AWS Glue Crawlers and ETL jobs to transform and load it into another S3 bucket for analysis. Lambda and API Gateway handle additional processing, while Amplify with Next.js hosts the dashboard, secured by Cognito. The architecture is detailed below:

- **RDS + S3**: Reliable data storage for transactions and assets

- **SQS + SNS**: Asynchronous order processing and notifications![IoT Weather Station Architecture](/images/2-Proposal/edge_architecture.jpeg)

- **CodePipeline**: Automated CI/CD for rapid deployments

![IoT Weather Platform Architecture](/images/2-Proposal/platform_architecture.jpeg)

### 3. Solution Architecture

### AWS Services Used

![Devteria Architecture](../images/2-Proposal/devteria-arch.png)- **AWS IoT Core**: Ingests MQTT data from 5 stations, scalable to 15.

- **AWS Lambda**: Processes data and triggers Glue jobs (two functions).

#### Key Components- **Amazon API Gateway**: Facilitates web app communication.

- **Amazon S3**: Stores raw data in a data lake and processed outputs (two buckets).

**Frontend Layer**- **AWS Glue**: Crawlers catalog data, and ETL jobs transform and load it.

- CloudFront CDN delivers static React app from S3- **AWS Amplify**: Hosts the Next.js web interface.

- Global edge caching for sub-2s page loads- **Amazon Cognito**: Secures access for lab users.

- HTTPS encryption and DDoS protection

### Component Design

**Backend Layer**- **Edge Devices**: Raspberry Pi collects and filters sensor data, sending it to IoT Core.

- API Gateway manages RESTful endpoints- **Data Ingestion**: AWS IoT Core receives MQTT messages from the edge devices.

- Lambda functions handle business logic (user, games, orders)- **Data Storage**: Raw data is stored in an S3 data lake; processed data is stored in another S3 bucket.

- ALB distributes traffic to EC2 microservices- **Data Processing**: AWS Glue Crawlers catalog the data, and ETL jobs transform it for analysis.

- Auto-scaling responds to demand- **Web Interface**: AWS Amplify hosts a Next.js app for real-time dashboards and analytics.

- **User Management**: Amazon Cognito manages user access, allowing up to 5 active accounts.

**Data Layer**

- RDS (PostgreSQL): User accounts, game catalog, orders### 4. Technical Implementation

- S3: Game files, images, user content**Implementation Phases**

- SQS: Async order processing queueThis project has two parts—setting up weather edge stations and building the weather platform—each following 4 phases:

- SNS: Email/SMS notifications- Build Theory and Draw Architecture: Research Raspberry Pi setup with ESP32 sensors and design the AWS serverless architecture (1 month pre-internship)

- Calculate Price and Check Practicality: Use AWS Pricing Calculator to estimate costs and adjust if needed (Month 1).

**Security & Monitoring**- Fix Architecture for Cost or Solution Fit: Tweak the design (e.g., optimize Lambda with Next.js) to stay cost-effective and usable (Month 2).

- Cognito: User pools with MFA- Develop, Test, and Deploy: Code the Raspberry Pi setup, AWS services with CDK/SDK, and Next.js app, then test and release to production (Months 2-3).

- IAM: Role-based access control  

- CloudWatch: Metrics, logs, alarms**Technical Requirements**

- Weather Edge Station: Sensors (temperature, humidity, rainfall, wind speed), a microcontroller (ESP32), and a Raspberry Pi as the edge device. Raspberry Pi runs Raspbian, handles Docker for filtering, and sends 1 MB/day per station via MQTT over Wi-Fi.

**CI/CD Pipeline**- Weather Platform: Practical knowledge of AWS Amplify (hosting Next.js), Lambda (minimal use due to Next.js), AWS Glue (ETL), S3 (two buckets), IoT Core (gateway and rules), and Cognito (5 users). Use AWS CDK/SDK to code interactions (e.g., IoT Core rules to S3). Next.js reduces Lambda workload for the fullstack web app.

- GitLab → CodePipeline → CodeBuild → Deployment

### 5. Timeline & Milestones

#### User Flow**Project Timeline**

1. User visits site via CloudFront- Pre-Internship (Month 0): 1 month for planning and old station review.

2. Cognito handles login/registration- Internship (Months 1-3): 3 months.

3. API Gateway + Lambda fetch game catalog from RDS    - Month 1: Study AWS and upgrade hardware.

4. User adds games to cart (DynamoDB session)    - Month 2: Design and adjust architecture.

5. Checkout triggers Lambda payment processing    - Month 3: Implement, test, and launch.

6. SQS queues license generation job- Post-Launch: Up to 1 year for research.

7. SNS sends confirmation email

8. S3 pre-signed URL provides secure download### 6. Budget Estimation

You can find the budget estimation on the [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=621f38b12a1ef026842ba2ddfe46ff936ed4ab01).  

### 4. AWS ServicesOr you can download the [Budget Estimation File](../attachments/budget_estimation.pdf).



| Service | Purpose | Configuration |### Infrastructure Costs

|---------|---------|---------------|- AWS Services:

| CloudFront | CDN | 10M requests, 50GB transfer |    - AWS Lambda: $0.00/month (1,000 requests, 512 MB storage).

| S3 | Storage | 100GB (frontend + assets) |    - S3 Standard: $0.15/month (6 GB, 2,100 requests, 1 GB scanned).

| API Gateway | API Management | 1M requests/month |    - Data Transfer: $0.02/month (1 GB inbound, 1 GB outbound).

| Lambda | Serverless Compute | 5M invocations, 512MB |    - AWS Amplify: $0.35/month (256 MB, 500 ms requests).

| EC2 | Microservices | 2x t3.medium |    - Amazon API Gateway: $0.01/month (2,000 requests).

| RDS | Database | db.t3.small Multi-AZ |    - AWS Glue ETL Jobs: $0.02/month (2 DPUs).

| ALB | Load Balancer | 1 ALB |    - AWS Glue Crawlers: $0.07/month (1 crawler).

| Cognito | Auth | 10K active users |    - MQTT (IoT Core): $0.08/month (5 devices, 45,000 messages).

| SQS | Message Queue | 5M requests |

| SNS | Notifications | 100K messages |Total: $0.7/month, $8.40/12 months

| CloudWatch | Monitoring | Standard metrics + logs |

| CodePipeline | CI/CD | 1 pipeline |- Hardware: $265 one-time (Raspberry Pi 5 and sensors).



### 5. Implementation Timeline### 7. Risk Assessment

#### Risk Matrix

**Phase 1 (Month 1): Foundation**- Network Outages: Medium impact, medium probability.

- AWS account setup, VPC configuration- Sensor Failures: High impact, low probability.

- RDS database deployment- Cost Overruns: Medium impact, low probability.

- S3 buckets + CloudFront distribution

- Cognito user pools#### Mitigation Strategies

- Network: Local storage on Raspberry Pi with Docker.

**Phase 2 (Months 2-3): Backend Development**- Sensors: Regular checks and spares.

- Lambda functions (auth, catalog, orders)- Cost: AWS budget alerts and optimization.

- API Gateway integration

- SQS/SNS setup#### Contingency Plans

- ALB deployment- Revert to manual methods if AWS fails.

- Use CloudFormation for cost-related rollbacks.

**Phase 3 (Month 3): Frontend**

- React/Next.js application### 8. Expected Outcomes

- Cognito integration#### Technical Improvements: 

- API connectionsReal-time data and analytics replace manual processes.  

- Deploy to S3 + CloudFrontScalable to 10-15 stations.

#### Long-term Value

**Phase 4 (Month 4): Advanced Features**1-year data foundation for AI research.  

- Payment gateway integrationReusable for future projects.

- Admin panel

- CloudWatch dashboards

- CI/CD pipeline



**Phase 5 (Month 5): Testing**

- Load testing

- Security audit

- Performance optimization

- UAT



**Phase 6 (Month 6): Launch**

- Soft launch
- Monitoring and feedback
- Public launch

### 6. Budget Estimation

#### Monthly Costs (10K users, 1K orders/month)

| Service | Cost |
|---------|------|
| CloudFront | $5 |
| S3 | $3 |
| API Gateway | $3.50 |
| Lambda | $20 |
| EC2 (2x t3.medium) | $60 |
| RDS (t3.small Multi-AZ) | $50 |
| ALB | $25 |
| Cognito | $25 |
| SQS + SNS | $4 |
| CloudWatch | $10 |
| Data Transfer | $15 |
| Route 53 | $2 |
| CodePipeline | $1 |
| Backups | $5 |

**Total: ~$228/month (~$2,742/year)**

**Scaling:**
- 50K users: ~$650/month
- 100K users: ~$1,200/month

#### One-Time Costs
- Development: $5,000-8,000
- Domain: $15/year
- SSL: Free (ACM)

### 7. Risk Assessment

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| DDoS Attack | High | Medium | AWS Shield, CloudFront, rate limiting |
| Data Breach | Critical | Low | Encryption, IAM, audits |
| Payment Fraud | High | Medium | 3D Secure, fraud detection |
| Lambda Cold Starts | Medium | High | Provisioned concurrency |
| Database Overload | High | Low | Read replicas, caching |
| Cost Overruns | Medium | Medium | Budget alerts, auto-scaling limits |

#### Contingency Plans
- RDS automated snapshots for quick recovery
- Multi-AZ deployment for high availability
- CodePipeline rollback for bad deployments
- Static maintenance page for AWS outages

### 8. Expected Outcomes

#### Technical
- **Performance**: <2s page loads globally
- **Scalability**: Handle 10x traffic spikes
- **Reliability**: 99.9% uptime
- **Security**: Zero breaches, PCI-ready

#### Business
- 40% reduction in checkout abandonment
- 60% less infrastructure management time
- 25-35% revenue increase from better UX
- Global market reach via CDN

#### Long-Term Value
- Scalable to 100K+ users
- Team gains AWS expertise
- Reusable microservices
- Rapid feature development

---

### Next Steps
1. Approve proposal
2. Set up AWS account
3. Assemble team
4. Begin Phase 1
5. Weekly progress reviews
