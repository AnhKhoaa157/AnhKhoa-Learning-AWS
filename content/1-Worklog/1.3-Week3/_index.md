---
title: "Week 3 Worklog"
date: "2025-12-04T07:05:17Z"
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---
{{% notice warning %}} 
 **Note:** For reference only.
{{% /notice %}}

##  Phase 1: AWS Foundation - Week 3

###  Objectives: Storage & Database

* Master S3, EBS, EFS storage
* Understand RDS managed database
* Distinguish block / object / file storage

###  Key Achievements:

**S3 Storage:**
*  Created bucket with versioning
*  Lifecycle: 30 days  IA, 90 days  Glacier
*  Cross-Region Replication setup
*  Encryption: SSE-S3, SSE-KMS, SSE-C

**EBS Volumes:**
*  Attached gp3 volume (100GB) to EC2
*  Created EBS snapshot
*  Compared: gp3 (general), io2 (high IOPS), st1 (throughput), sc1 (cold)

**RDS Database:**
*  Launched RDS MySQL 8.0: db.t3.micro
*  Connected from EC2
*  Created database schema and test queries

**Storage Types:**
* Object (S3): Static assets, backups
* Block (EBS): OS volumes, databases
* File (EFS): Shared file systems

###  Next Week: IAM, MFA, Security best practices

