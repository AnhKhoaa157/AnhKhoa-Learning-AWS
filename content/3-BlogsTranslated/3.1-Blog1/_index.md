---
title: "Blog 1"
date: "2025-12-04T07:05:17Z"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
# Enabling customers to deliver production-ready AI agents at scale

Quick facts:

- Author: Swami Sivasubramanian  
- Published: July 16, 2025  
- Topics: Amazon Bedrock, Amazon Connect, Amazon Nova, Amazon Q, Amazon S3, Announcements, AWS Inferentia, AWS Trainium, AWS Transform, Spotlight, Thought Leadership

---

## Table of contents

1. Introduction
2. Challenges
3. Guiding principles / AWS approach  
  3.1. Agility  
  3.2. Evolving the fundamentals for the agent era  
  3.3. Model & data choices  
  3.4. Deploying transformative solutions
4. New components introduced
5. Next steps / Call to action
6. Conclusion
7. Related resources

---

## 1. Introduction

AI agents will change how we work and live. AWS CEO Matt Garman described this shift as a technology change on the scale of the internet. This is an exciting vision, because we are already seeing intelligent agent systems begin to solve complex problems, automate workflows, and unlock new capabilities across industries.

![AI Agent Overview](https://d2908q01vomqb2.cloudfront.net/f1f836cb4ea6efb2a0b1b99f41ad8b103eff4b59/2025/07/16/2025-0716_middle-grey-studios_aws-amers_preview-001_websize.jpg)

---

## 2. Challenges

- To move beyond early successes and reach broad adoption, organizations need practical approaches to manage the inherent complexity of agent systems.  
- They must move quickly while establishing strong foundations for security, reliability, and operational excellence.  
- Systems must be flexible to adapt as models evolve, new capabilities emerge, and use cases scale across the enterprise.

---

## 3. Guiding principles / AWS approach

AWS focuses on capabilities that address the foundational aspects of building and deploying production-scale agents. Key principles and elements include:

### 3.1 Agility

Agent systems must embrace change, be open to new technologies, and integrate with proprietary data and existing tools.

### 3.2 Evolving the fundamentals for the agent era

- Security and trust: per-session isolation, memory separation to prevent data leaks.  
- Reliability and scalability: handle large numbers of concurrent sessions, checkpoint & recovery for failures/interruption.  
- Identity: for both agents and users, with fine-grained, short-lived access.  
- Observability: real-time dashboards and standard telemetry integrated with monitoring systems.

### 3.3 Model & data choices to deliver superior outcomes

- Allow customers to choose or customize the model best suited for each use case.  
- Support customization workflows such as fine-tuning, retraining, PEFT, SFT, PPO, etc.  
- Customer-owned data is the key to giving models the context they need.

### 3.4 Deploying transformative solutions

- Provide pre-built agents and tooling via the **AWS Marketplace** to simplify access.  
- Pre-packaged agent solutions reduce time from prototype to production and integrate quickly with existing systems.

![Guiding Principles Diagram](https://d2908q01vomqb2.cloudfront.net/f1f836cb4ea6efb2a0b1b99f41ad8b103eff4b59/2025/07/16/image6.png)

---

## 4. New components introduced

- **Amazon Bedrock AgentCore**: a secure, serverless runtime with per-session isolation, rights/context controls, suited for enterprise environments.  
- **Nova customization in Amazon SageMaker AI**: multi-stage model customization support including fine-tuning, alignment, PEFT, SFT, PPO, CPT, and Knowledge Distillation.  
- **Nova Act SDK**: models trained to perform browser-based actions for automating web tasks and building action-capable agents.  
- **Amazon S3 Vectors**: object storage with native vector support to store embeddings at ~90% lower cost while maintaining sub-second query performance.

---

## 5. Next steps / Call to action

- Start small: pick a specific business problem and build quickly.  
- Collect real-world feedback and iterate.  
- AWS is investing **$100M** in the **AWS Generative AI Innovation Center** to help customers across industries realize value from AI agents.

---

## 6. Conclusion

With clear guiding principles, strong platform primitives, and new supporting components, AWS is building a comprehensive foundation to help customers deploy production-grade AI agents — secure, reliable, and efficient.

Don’t wait for perfection; start, learn from production feedback, and scale.

---

## 7. Related resources

- [Read the original AWS post](https://aws.amazon.com/blogs/machine-learning/enabling-customers-to-deliver-production-ready-ai-agents-at-scale/)

This blog post is part of a larger series on getting started with setting up a healthcare data lake. In my final post of the series, *“Getting Started with Healthcare Data Lakes: Diving into Amazon Cognito”*, I focused on the specifics of using Amazon Cognito and Attribute Based Access Control (ABAC) to authenticate and authorize users in the healthcare data lake solution. In this blog, I detail how the solution evolved at a foundational level, including the design decisions I made and the additional features used. You can access the code samples for the solution in this Git repo for reference.

---

## Architecture Guidance

The main change since the last presentation of the overall architecture is the decomposition of a single service into a set of smaller services to improve maintainability and flexibility. Integrating a large volume of diverse healthcare data often requires specialized connectors for each format; by keeping them encapsulated separately as microservices, we can add, remove, and modify each connector without affecting the others. The microservices are loosely coupled via publish/subscribe messaging centered in what I call the “pub/sub hub.”

This solution represents what I would consider another reasonable sprint iteration from my last post. The scope is still limited to the ingestion and basic parsing of **HL7v2 messages** formatted in **Encoding Rules 7 (ER7)** through a REST interface.

**The solution architecture is now as follows:**

> *Figure 1. Overall architecture; colored boxes represent distinct services.*

---

While the term *microservices* has some inherent ambiguity, certain traits are common:  
- Small, autonomous, loosely coupled  
- Reusable, communicating through well-defined interfaces  
- Specialized to do one thing well  
- Often implemented in an **event-driven architecture**

When determining where to draw boundaries between microservices, consider:  
- **Intrinsic**: technology used, performance, reliability, scalability  
- **Extrinsic**: dependent functionality, rate of change, reusability  
- **Human**: team ownership, managing *cognitive load*

---

## Technology Choices and Communication Scope

| Communication scope                       | Technologies / patterns to consider                                                        |
| ----------------------------------------- | ------------------------------------------------------------------------------------------ |
| Within a single microservice              | Amazon Simple Queue Service (Amazon SQS), AWS Step Functions                               |
| Between microservices in a single service | AWS CloudFormation cross-stack references, Amazon Simple Notification Service (Amazon SNS) |
| Between services                          | Amazon EventBridge, AWS Cloud Map, Amazon API Gateway                                      |

---

## The Pub/Sub Hub

Using a **hub-and-spoke** architecture (or message broker) works well with a small number of tightly related microservices.  
- Each microservice depends only on the *hub*  
- Inter-microservice connections are limited to the contents of the published message  
- Reduces the number of synchronous calls since pub/sub is a one-way asynchronous *push*

Drawback: **coordination and monitoring** are needed to avoid microservices processing the wrong message.

---

## Core Microservice

Provides foundational data and communication layer, including:  
- **Amazon S3** bucket for data  
- **Amazon DynamoDB** for data catalog  
- **AWS Lambda** to write messages into the data lake and catalog  
- **Amazon SNS** topic as the *hub*  
- **Amazon S3** bucket for artifacts such as Lambda code

> Only allow indirect write access to the data lake through a Lambda function → ensures consistency.

---

## Front Door Microservice

- Provides an API Gateway for external REST interaction  
- Authentication & authorization based on **OIDC** via **Amazon Cognito**  
- Self-managed *deduplication* mechanism using DynamoDB instead of SNS FIFO because:  
  1. SNS deduplication TTL is only 5 minutes  
  2. SNS FIFO requires SQS FIFO  
  3. Ability to proactively notify the sender that the message is a duplicate  

---

## Staging ER7 Microservice

- Lambda “trigger” subscribed to the pub/sub hub, filtering messages by attribute  
- Step Functions Express Workflow to convert ER7 → JSON  
- Two Lambdas:  
  1. Fix ER7 formatting (newline, carriage return)  
  2. Parsing logic  
- Result or error is pushed back into the pub/sub hub  

---

## New Features in the Solution

### 1. AWS CloudFormation Cross-Stack References
Example *outputs* in the core microservice (exported so other stacks can import them with Fn::ImportValue / !ImportValue):

```yaml
Outputs:
  Bucket:
    Value: !Ref Bucket
    Export:
      Name: !Sub ${AWS::StackName}-Bucket
  ArtifactBucket:
    Value: !Ref ArtifactBucket
    Export:
      Name: !Sub ${AWS::StackName}-ArtifactBucket
  Topic:
    Value: !Ref Topic
    Export:
      Name: !Sub ${AWS::StackName}-Topic
  Catalog:
    Value: !Ref Catalog
    Export:
      Name: !Sub ${AWS::StackName}-Catalog
  CatalogArn:
    Value: !GetAtt Catalog.Arn
    Export:
      Name: !Sub ${AWS::StackName}-CatalogArn
```

A consumer stack can import these values using `Fn::ImportValue` (or `!ImportValue` in YAML). Example: import the Catalog ARN into a Lambda environment variable.

```yaml
Resources:
  MyFunction:
    Type: AWS::Lambda::Function
    Properties:
      Handler: index.handler
      Role: !GetAtt LambdaExecutionRole.Arn
      Runtime: nodejs18.x
      Environment:
        Variables:
          CATALOG_ARN: !ImportValue MyCoreStack-CatalogArn
```

If the export name is parameterized you can construct the import name with Fn::Sub:

```yaml
Parameters:
  CoreStackName:
    Type: String
Resources:
  MyFunction2:
    Type: AWS::Lambda::Function
    Properties:
      Handler: index.handler
      Role: !GetAtt LambdaExecutionRole.Arn
      Runtime: nodejs18.x
      Environment:
        Variables:
          CATALOG_ARN: !ImportValue
            'Fn::Sub': '${CoreStackName}-CatalogArn'
```














