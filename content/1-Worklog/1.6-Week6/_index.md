---
title: "Week 6 Worklog"
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}


### Week 6 Objectives:

* Learn about monitoring and logging with CloudWatch.
* Understand Auto Scaling for automatic scaling resources.
* Study cost optimization and billing management.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Review basic IAM knowledge <br> - Learn advanced IAM Policy (JSON structure, Effect, Action, Resource, Condition) <br> - Create custom policies and attach to users/groups                                                                                             | 13/10/2025 | 13/10/2025      | AWS Journey                              |
| 3   | - Introduction to AWS Key Management Service (KMS) <br> - Create a Customer Managed Key (CMK) and test file encryption/decryption <br> - Apply KMS to encrypt an S3 bucket or an EBS volume                                                           | 14/10/2025 | 14/10/2025      | AWS Journey |
| 4   | - Get familiar with AWS Secrets Manager <br> - Create a secret to store Database connection information <br> - Write a small Lambda script to read the secret from Secrets Manager                                                          | 15/10/2025 | 15/10/2025      | AWS Journey                  |
| 5   | - Explore AWS Billing Dashboard and Cost Explorer <br> - View costs by service, region, and time period <br> - Set up Cost Anomaly Detection                                                           | 16/10/2025 | 16/10/2025      | AWS Journey                                   |
| 6   | - Create an AWS Budget and configure cost alerts via email <br> - Write a weekly cost summary report with optimization proposals (stop EC2, cleanup EBS, reduce log retention, etc.) <br> - Wrap up Week 6 learnings                                            | 17/10/2025 | 17/10/2025      | AWS Journey                               |


### Week 6 Achievements:

* Learned about CloudWatch monitoring:
  * Understood CloudWatch metrics for AWS services
  * Learned how to view and analyze metrics
  * Studied CloudWatch dashboards for visualization

* Created CloudWatch alarms:
  * Set up alarm for EC2 CPU utilization > 80%
  * Configured SNS topic for email notifications
  * Tested alarm by increasing CPU load
  * Viewed CloudWatch Logs for Lambda executions

* Understood Auto Scaling concepts:
  * Learned benefits of automatic scaling
  * Studied Auto Scaling Groups components
  * Understood scaling policies:
    * Target tracking (maintain metric at target)
    * Step scaling (scale based on thresholds)

* Practiced with Auto Scaling:
  * Created Launch Template for EC2 instances
  * Set up Auto Scaling Group (min: 2, max: 5)
  * Configured scaling policy based on CPU
  * Generated load to test automatic scaling
  * Observed instances launching and terminating

* Learned about AWS billing:
  * Explored AWS Billing Dashboard
  * Set up billing alerts for cost threshold
  * Reviewed Cost Explorer for spending patterns
  * Learned about cost allocation tags