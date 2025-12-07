---
title: "Week 5 Worklog"
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}


### Week 5 Objectives:

* Learn about container services on AWS.
* Understand ECS and basic Docker concepts.
* Study Load Balancer for distributing traffic.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Introduction to Infrastructure as Code (IaC) concepts and benefits compared to manual deployment <br> - Get familiar with AWS CloudFormation: template, stack, parameter                                                                                                   | 06/10/2025 | 06/10/2025      | AWS Journey |
| 3   | - Write CloudFormation template to deploy S3 bucket and EC2 instance <br> - Create, update, and delete stack via AWS Console                                              | 07/10/2025 | 07/10/2025      | AWS Journey |
| 4   | - Introduction to AWS CDK (Cloud Development Kit) <br> - Install AWS CDK, create CDK project using Python or TypeScript <br> - Write CDK code to deploy EC2 instance | 08/10/2025 | 08/10/2025      | AWS Journey |
| 5   | - Introduction to AWS Systems Manager (SSM) and key features: Parameter Store, Run Command, Automation, Session Manager <br> - Create Parameter Store to store configuration variables   <br>                            | 09/10/2025 | 09/10/2025      | AWS Journey |
| 6   | - Practice creating Automation Document in SSM to automatically start/stop EC2 <br> - Test Session Manager (access EC2 without SSH key) <br> - Week summary: IaC + SSM demo                                                                                     | 10/10/2025 | 10/10/2025      | AWS Journey |


### Week 5 Achievements:

* Learned about containerization and Docker:
  * Understood benefits of containers
  * Learned difference between containers and VMs
  * Studied Docker images and containers

* Practiced with Docker locally:
  * Installed Docker Desktop
  * Pulled images from Docker Hub
  * Ran basic containers:
    * `docker pull nginx`
    * `docker run -d -p 80:80 nginx`
  * Learned basic Docker commands

* Understood Amazon ECS:
  * Learned ECS as managed container service
  * Studied ECS components:
    * Clusters
    * Task definitions
    * Services
  * Compared Fargate (serverless) vs EC2 launch types

* Deployed application on ECS:
  * Created ECS cluster
  * Created task definition with simple web app
  * Deployed service on Fargate
  * Viewed container logs in CloudWatch

* Worked with Application Load Balancer:
  * Created ALB for distributing traffic
  * Configured target groups
  * Integrated ALB with ECS service
  * Tested load balancing with multiple tasks