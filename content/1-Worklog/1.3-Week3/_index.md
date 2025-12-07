---
title: "Week 3 Worklog"
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}


### Week 3 Objectives:

* Learn about Amazon RDS for managed databases.
* Understand AWS Lambda and serverless basics.
* Study CloudWatch for monitoring and logging.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Introduction to CDN concepts and CloudFront benefits <br> - Create CloudFront Distribution to distribute S3 website content | 22/09/2025 | 22/09/2025      | AWS Journey |
| 3   | - Configure CloudFront behavior and cache policy <br> - Test website access via CloudFront URL <br> - Perform invalidation to update new content | 23/09/2025 | 23/09/2025      | AWS Journey |
| 4   | - Introduction to DynamoDB (NoSQL Database) <br> - Create DynamoDB tables (Users, Products, etc.) <br> - Practice CRUD operations on Console | 24/09/2025 | 24/09/2025      | AWS Journey |
| 5   | - Connect and query DynamoDB using AWS CLI <br> - Write small scripts to add and read data from tables | 25/09/2025 | 25/09/2025      | AWS Journey |
| 6   | - Learn about ElastiCache (Redis & Memcached) <br> - Create basic Redis cluster <br> - Test connection from EC2 to store and read cache data | 26/09/2025 | 26/09/2025      | AWS Journey |


### Week 3 Achievements:

* **Learned about Amazon RDS:**
  * Understood RDS as managed database service
  * Learned benefits of managed databases:
    * Automated backups
    * Automatic updates
    * Easy scaling
  * Studied database engines:
    * MySQL - popular for web applications
    * PostgreSQL - advanced features
  * Understood Multi-AZ for high availability
  * Learned about Read Replicas for scaling

* **Practiced with RDS:**
  * Created RDS MySQL instance (db.t3.micro)
  * Configured security groups for database access
  * Connected to RDS from EC2 instance:
    ```bash
    mysql -h mydb.ap-southeast-1.rds.amazonaws.com -u admin -p
    ```
  * Created sample database and tables
  * Configured automated backups
  * Tested snapshot creation and restore
  * Monitored database metrics in CloudWatch

* **Understood AWS Lambda basics:**
  * Learned serverless computing concept:
    * No server management
    * Auto scaling
    * Pay per use
  * Understood Lambda function structure:
    * Handler function
    * Event input
    * Execution role
  * Studied Lambda triggers:
    * S3 events
    * API Gateway
    * CloudWatch Events
  * Learned Lambda limitations:
    * 15-minute timeout
    * Memory limits

* **Practiced with Lambda:**
  * Created Lambda function in Python:
    ```python
    def lambda_handler(event, context):
        return {
            'statusCode': 200,
            'body': 'Hello from Lambda!'
        }
    ```
  * Configured IAM execution role
  * Set up S3 trigger for image processing
  * Tested function execution
  * Monitored logs in CloudWatch
  * Analyzed function performance

* **Learned CloudWatch monitoring:**
  * Understood CloudWatch components:
    * Metrics - performance data
    * Logs - application logs
    * Alarms - automated alerts
  * Learned about metric types:
    * EC2 metrics (CPU, disk, network)
    * RDS metrics (connections, storage)
    * Lambda metrics (invocations, errors)

* **Practiced with CloudWatch:**
  * Created alarms for EC2 instances:
    * High CPU usage
    * Low disk space
  * Set up alarms for RDS:
    * High connections
    * Low storage
  * Viewed and analyzed CloudWatch Logs
  * Built simple dashboard for monitoring
  * Configured log retention policies

* **Gained practical experience:**
  * Can set up and manage RDS databases
  * Understand serverless with Lambda
  * Know how to monitor with CloudWatch
  * Ready for Week 4 advanced topics