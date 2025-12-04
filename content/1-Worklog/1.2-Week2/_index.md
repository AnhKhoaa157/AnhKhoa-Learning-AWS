---
title: "Week 2 Worklog"
date: "2025-12-04T07:05:17Z"
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---
{{% notice warning %}} 
 **Note:** For reference only. Do not copy verbatim.
{{% /notice %}}

##  Phase 1: AWS Foundation - Week 2

###  Objectives: Compute & Networking Basics

* Master EC2, Elastic IP, Auto Scaling
* Understand VPC architecture: Subnet, Route Table, Security Group, NACL
* Deploy first web server on AWS

###  Key Achievements:

**EC2 Compute:**
*  Instance families: t2/t3 (general), c5 (compute), r5 (memory), i3 (storage)
*  Instance lifecycle: pending  running  stopping  stopped  terminated
*  EBS (persistent, network) vs Instance Store (ephemeral, high-perf)

**VPC Networking:**
*  Created VPC: fcj-bootcamp-vpc (10.0.0.0/16)
*  Subnet design: Public (10.0.1.0/24), Private (10.0.2.0/24)
*  Attached Internet Gateway
*  Route Table: Public RT  0.0.0.0/0  IGW

**Security:**
*  Security Group web-server-sg: SSH (22) + HTTP (80)
*  NACL: stateless, subnet-level firewall

**Hands-on: First Web Server**
*  Launched EC2: t2.micro, Amazon Linux 2023
*  SSH connection successful
*  Installed Apache, accessed from internet
*  Attached Elastic IP for fixed public IP

###  Next Week: S3 advanced, RDS, EBS snapshots

