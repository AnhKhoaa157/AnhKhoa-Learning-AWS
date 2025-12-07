---
title: "Week 7 Worklog"
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}


### Week 7 Objectives:

* Learn about CI/CD concepts and AWS CodePipeline.
* Understand CodeCommit, CodeBuild, and CodeDeploy.
* Build a simple deployment pipeline.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Learn about High Availability, Fault Tolerance and Elasticity concepts <br> - Introduction to Auto Scaling Group (ASG) and Elastic Load Balancer (ELB)                                                                                              | 20/10/2025 | 20/10/2025      | AWS Journey                               |
| 3   | - Practice creating Auto Scaling Group for EC2 instance <br> - Set up launch template, scaling policy and target tracking                                            | 21/10/2025 | 21/10/2025      | AWS Journey                             |
| 4   | - Create and configure Application Load Balancer (ALB) <br> - Connect ALB with ASG for load distribution <br> - Test website access through ALB DNS                                                                                | 22/10/2025 | 22/10/2025      | AWS Journey                              |
| 5   | - Get familiar with Amazon SQS and SNS services <br> - Create SQS queue, SNS topic and subscription <br> - Send and receive notifications between components                                                                                 | 23/10/2025 | 23/10/2025      | AWS Journey                                     |
| 6   | - Enable VPC Flow Logs to monitor network traffic <br> - Analyze logs in CloudWatch Logs <br> - Summarize knowledge about reliability & scaling                                                 | 24/10/2025 | 24/10/2025      | AWS Journey                     |


### Week 7 Achievements:

* Learned about CI/CD concepts:
  * Understood Continuous Integration
  * Understood Continuous Deployment
  * Learned benefits of automation

* Worked with AWS CodeCommit:
  * Created Git repository in CodeCommit
  * Configured Git credentials
  * Pushed sample web application code
  * Learned basic Git commands

* Practiced with AWS CodeBuild:
  * Created buildspec.yml configuration file
  * Set up CodeBuild project
  * Configured build environment (Ubuntu, Node.js)
  * Built application successfully
  * Viewed build logs

* Deployed with AWS CodeDeploy:
  * Created appspec.yml file
  * Set up deployment group with EC2 instances
  * Installed CodeDeploy agent on EC2
  * Deployed application to EC2
  * Verified deployment success

* Built complete pipeline with CodePipeline:
  * Created pipeline with 3 stages: Source, Build, Deploy
  * Connected CodeCommit as source
  * Added CodeBuild for building
  * Added CodeDeploy for deployment
  * Tested automatic deployment on code push