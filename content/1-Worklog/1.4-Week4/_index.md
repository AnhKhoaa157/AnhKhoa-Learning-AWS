---
title: "Week 4 Worklog"
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}


### Week 4 Objectives:

* Learn about AWS Lambda and serverless computing.
* Understand API Gateway for building REST APIs.
* Study SNS and SQS for messaging services.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Learn Migration concepts (Lift & Shift, Replatform, Refactor) <br> - Introduction to AWS Database Migration Service (DMS)                                                                                                   | 29/09/2025 | 29/09/2025      | AWS Journey |
| 3   | - Practice creating Replication Instance in DMS <br> - Configure source data (on-premise) and target (RDS) <br> - Perform test data migration                                              | 30/09/2025 | 30/09/2025      | AWS Journey |
| 4   | - Introduction to Elastic Disaster Recovery (EDR) <br> - Learn how to set up replication server and recovery instance | 01/10/2025 | 01/10/2025      | AWS Journey |
| 5   | - Practice simulating failures: shut down main EC2 and start recovery instance from EDR <br> - Evaluate recovery time (RTO/RPO)   <br>                            | 02/10/2025 | 02/10/2025      | AWS Journey |
| 6   | - Create basic Disaster Recovery plan (backup, restore, failover) <br> - Write documentation summarizing Migration + DR processes <br> - Summarize Week 4 knowledge                                                                                     | 03/10/2025 | 03/10/2025      | AWS Journey |


### Week 4 Achievements:

* Learned about serverless computing and benefits:
  * No server management required
  * Pay only for execution time
  * Automatic scaling

* Created Lambda functions using Python:
  * Basic hello world function
  * Function with environment variables
  * Tested with different event sources

* Configured Lambda settings:
  * Timeout (default 3 seconds)
  * Memory allocation
  * Execution role with basic permissions

* Built REST API with API Gateway:
  * Created simple GET/POST endpoints
  * Connected API to Lambda backend
  * Tested API calls using browser and Postman

* Practiced with SNS:
  * Created SNS topic
  * Added email subscription
  * Sent test notifications

* Worked with SQS:
  * Created standard queue
  * Sent messages to queue
  * Retrieved and deleted messages from queue
  * Understood message retention