---
title: "Week 4 Worklog"
date: "2025-12-04T07:05:17Z"
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---
{{% notice warning %}} 
 **Note:** For reference only.
{{% /notice %}}

##  Phase 1: AWS Foundation - Week 4

###  Objectives: IAM & Security Basics

* Master IAM Users, Groups, Roles, Policies
* Implement MFA and security best practices
* Apply Principle of Least Privilege

###  Key Achievements:

**IAM Configuration:**
*  Created IAM groups: Admins, Developers, ReadOnly
*  Attached managed policies: AdministratorAccess, PowerUserAccess
*  Created custom inline policy for specific S3 bucket
*  Enabled MFA for all IAM users
*  Password policy: min 12 chars, rotate every 90 days

**IAM Roles:**
*  Created EC2 instance role for S3 access
*  Set up cross-account role
*  Configured AssumeRole policies

**Security Best Practices:**
*  Never use root account for daily tasks
*  Enable MFA on ALL accounts
*  Rotate access keys every 90 days
*  Use IAM roles instead of access keys for EC2
*  Regular audits with IAM Access Analyzer

###  Next Week: CloudWatch, CloudTrail, SNS, EventBridge

