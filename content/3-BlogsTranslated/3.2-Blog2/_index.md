---
title: "Blog 2 - ALB for SAP workloads on AWS"
date: 2025-01-07
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Elevate User Experience and Security of Application Load Balancer for SAP workloads on AWS

**Authors**: Ferry Mulyadi, Mohit Biyani, and Krishnakumar Ramadoss  
**Published**: January 7, 2025  
**Categories**: Elastic Load Balancing, How-To, SAP on AWS, Technical How-to, Thought Leadership  
**Source**: AWS for SAP Blog

---

## Overview

This article presents how to **enhance user experience and security** when using Application Load Balancer (ALB) for SAP workloads on AWS. Many customers run SAP on AWS and build cloud-native application extensions to maintain a clean ERP core and accelerate innovation.

### Main Focus

A key focus is **principal propagation**, the ability to carry authenticated identity from ALB to SAP systems without re-authentication, enabling a **Single Sign-On (SSO)** experience.

The article explains how **mTLS (Mutual Transport Layer Security)** and X.509 certificates support principal propagation, including two mTLS modes on ALB:

- **Passthrough mode**: forwards the entire certificate chain to backend
- **Verify mode**: ALB authenticates client first, offloading backend

**Recommendation**: Use **mTLS verify mode** for SAP on AWS as it ensures security and processing efficiency.

---

## What is Principal Propagation?

**Principal propagation** allows users to log in once to access multiple systems securely, eliminating the need for multiple logins.

### How it Works

When a user is authenticated (e.g., via client certificate at ALB), that identity is recognized and trusted consistently by downstream systems like SAP. This:

- Avoids redundant login prompts
- Maintains secure user context across the landscape
- Enables **Single Sign-On (SSO)**
- Ensures actions are performed under user identity, not system user
- Improves security by relying on token-based authentication

---

## Key Benefits of Principal Propagation

### 1. Better User Experience with SSO

Users only need to authenticate once to access multiple systems and applications, eliminating the frustration of repeated login prompts and maintaining productivity across platforms.

### 2. Enhanced Security

- Eliminates need to store multiple credentials
- Reduces authentication touchpoints
- Creates unified security context
- Maintains clear audit trails
- Enforces consistent security policies across integrated systems

### 3. Compliance and Governance

- Comprehensive user activity tracking
- Improved accountability
- User actions properly recorded and logged
- Simplifies audit processes and regulatory reporting

### 4. Administrative Efficiency

IT teams can manage:
- User access
- Permissions
- Credentials

From a single control point, simplifying user lifecycle management.

### 5. System Integration

Principal propagation serves as a bridge between different systems and platforms, maintaining consistent user context and authorization across system boundaries. Especially valuable in modern hybrid environments.

### 6. Cost Reduction

- Reduced help desk tickets
- Simplified security deployment
- More efficient resource use through centralized management

---

## How mTLS Authentication Supports Principal Propagation

**Mutual Transport Layer Security (mTLS) Authentication** establishes a secure, two-way encrypted connection between client and server.

### Difference from Standard TLS

Unlike standard TLS (where only server provides certificate), **mTLS requires both parties to present digital certificates**.

![mTLS Authentication Flow](/images/3.2-Blog2/image1.png)
*Figure 1. mTLS authentication flow between client and server*

### mTLS Authentication Process

1. **Client** requests connection to server
2. **Server** presents its certificate
3. **Client** verifies servers certificate
4. **Client** presents its certificate for server to verify and authenticate
5. Secure connection is established between client and server

### Requirements

You need to provide client and server certificates using your **Certificate Authority (CA)** to ensure both are trusted.

**Recommendation**: All SSL/TLS certificates across the entire infrastructure (ALB, SAP Web Dispatcher, S/4HANA) should originate from a single trusted root certificate authority.

---

## mTLS Client Authentication with Application Load Balancer

ALB supports mTLS authentication with two modes: **passthrough** and **verify mode**.

### mTLS Passthrough Mode

In this mode, ALB forwards the entire client certificate chain to backend targets via the `X-Amzn-Mtls-Clientcert` HTTP header.

#### Characteristics

- Chain includes leaf certificate
- PEM format URL-encoded with `+`, `=`, and `/` as safe characters
- ALB doesnt add header if client certificate is absent
- Backend must handle client authentication and error processing
- For HTTPS listeners, ALB terminates client-ALB TLS and initiates new ALB-backend TLS
- Allows use of any ALB routing algorithm

### mTLS Verify Mode  (Recommended)

#### How it Works

1. Create **trust store** containing CA certificate bundle
   - Use AWS Certificate Manager (ACM)
   - AWS Private CA
   - Import your own certificates

2. Manage revoked certificates
   - Use Certificate Revocation Lists (CRLs)
   - Store in Amazon S3
   - Link to trust store

#### Benefits

- **ALB handles verification** of client certificate against trust store
- Effectively blocks unauthorized requests
- **Offloads mTLS processing** from backend targets
- Improves overall system efficiency
- ALB imports CRLs from S3 and performs checks without repeated S3 fetches
- Minimizes latency

#### Certificate Metadata

ALB transmits client certificate metadata via HTTP Headers (e.g., `X-Amzn-Mtls-Clientcert-Leaf`) to SAP Web Dispatcher backend.

This enables:
- Implementing additional logic on backend based on certificate details
- Meeting requirements for SAP Servers to preserve original "Host Header" information
- Processing client certificate metadata consistently

#### SAP Web Dispatcher Configuration

When deploying end-to-end encryption through ALB  SAP Web Dispatcher  SAP Servers, need to configure profile parameter:

```
icm/HTTPS/client_certificate_header_name
```

---

## Solution Architecture

![mTLS Verify Architecture](/images/3.2-Blog2/image2.png)
*Figure 2. mTLS verify authentication data flow for SAP workloads on AWS*

Architecture describes how mTLS authentication is deployed in:
- **On-premise** and **AWS VPC** connected via AWS Direct Connect
- Remote users with internet connectivity

### Architecture Flow

#### Step 1: Client Certificate
Users have X.509 certificate installed on client device (laptop and/or mobile).

#### Step 2: Authentication at ALB
- Client device initiates connection to ALB
- Shares certificate for mutual verification
- ALB uses Amazon S3 as trust store

#### Step 3: Forward to SAP Web Dispatcher
Once verified, ALB forwards connection to SAP Web Dispatcher for verification and authentication.

#### Step 4: Connect to SAP Instances
SAP Web Dispatcher forwards connection to SAP Instances (S/4HANA, ABAP Stacks, etc.) for final verification and authentication.

---

## Implementation Steps

### 1. Configure Trust Store with Amazon S3

#### a) Create S3 Bucket
```bash
aws s3 mb s3://my-sap-trust-store-bucket
```

#### b) Upload CA and Intermediate Certificates
```bash
aws s3 cp root-ca.pem s3://my-sap-trust-store-bucket/
aws s3 cp intermediate-ca.pem s3://my-sap-trust-store-bucket/
```

#### c) Create Trust Store via EC2 Console
- Link to S3 bucket
- Configure Certificate Revocation Lists (CRLs)

### 2. Configure Application Load Balancer

#### a) Request SSL Public Certificate
```bash
aws acm request-certificate \
  --domain-name sap.example.com \
  --validation-method DNS
```

#### b) Create HTTPS Listener
```bash
aws elbv2 create-listener \
  --load-balancer-arn <ALB-ARN> \
  --protocol HTTPS \
  --port 443 \
  --certificates CertificateArn=<CERT-ARN> \
  --default-actions Type=forward,TargetGroupArn=<TG-ARN>
```

#### c) Enable mTLS Verify
```bash
aws elbv2 modify-listener \
  --listener-arn <LISTENER-ARN> \
  --mutual-authentication Mode=verify,TrustStoreArn=<TRUST-STORE-ARN>
```

#### d) Create Security Group for SAP Web Dispatcher
```bash
# Only allow traffic from ALB
aws ec2 create-security-group \
  --group-name SAP-WebDispatcher-SG \
  --description "Security group for SAP Web Dispatcher"
```

#### e) Create Target Group
```bash
aws elbv2 create-target-group \
  --name SAP-WebDispatcher-TG \
  --protocol HTTPS \
  --port 443 \
  --vpc-id <VPC-ID>
```

### 3. Configure SAP Web Dispatcher

#### a) Import Root and Intermediate Certificate
```bash
sapgenpse import_own_cert \
  -p SAPSSLS.pse \
  -c root-ca.pem \
  -r intermediate-ca.pem
```

#### b) Create SSL Public Certificate
```bash
sapgenpse get_pse \
  -p SAPSSLS.pse \
  -x <PASSWORD> \
  "CN=webdispatcher.example.com, O=MyOrg, C=US"
```

#### c) Configure SAP Parameters
In SAP Web Dispatcher profile file, add:

```ini
icm/HTTPS/client_certificate_header_name = x-amzn-mtls-clientcert
```

### 4. Configure SAP S/4HANA

#### a) Import Certificates into STRUST
1. Log into SAP GUI
2. Transaction code: **STRUST**
3. Import Root CA and Intermediate CA
4. Activate changes

#### b) Configure Trusted Reverse Proxy
```ini
icm/trusted_reverse_proxy_<N> = SUBJECT="CN=webdispatcher.example.com, O=MyOrg, C=US", ISSUER="CN=Root-CA, O=MyOrg, C=US"
```

**Reference**: [SAP Help Portal - Web Dispatcher Parameters](https://help.sap.com/docs)

---

## Pricing

### ALB Pricing Information

Detailed pricing for Application Load Balancer: [AWS Pricing](https://aws.amazon.com/elasticloadbalancing/pricing/)

### Additional Fees for mTLS

- **Per-hour per-trust store** associated with ALB
- Based on mTLS Verify usage scenario

### Cost Calculation Example

**Assumptions**:
- S/4 HANA application receives average 1 new connection/second
- Each connection lasts 2 minutes
- Client sends average 5 requests/second
- Total bytes processed: 300 KB/second
- 1 rule on load balancer
- 1 trust store for Mutual TLS
- Region: US East (Northern Virginia)

#### LCU Calculations

**1. New connections (per second)**
```
1 connection/sec  25 connections/sec = 0.04 LCUs
```

**2. Active connections (per minute)**
```
120 active connections/min  3,000 connections/min = 0.04 LCUs
```

**3. Bytes processed (GB/hour)**
```
300 KB/sec = 1.08 GB/hour = 1.08 LCUs
```

**4. Rule evaluations**
```
10 rules free  0 LCU
```

#### Final Cost

**Maximum LCU**: 1.08 LCUs (bytes processed)

**Fee per hour**:
```
- ALB hour: $0.0225
- Trust store: $0.0056
- LCU: 1.08  $0.008 = $0.00864
- Total: $0.03674/hour
```

**Fee per month**:
```
$0.03674  24 hours  30 days = $26.45/month
```

---

## Conclusion

### Key Points

ALB mTLS support provides a robust foundation for deploying principal propagation in SAP landscapes:

 **Simplifies deployment** of principal propagation  
 **Seamless integration** with SAP Web Dispatcher  
 **Reduces operational overhead** with managed services  
 **Enhanced security** via certificate-based authentication  
 **Secure, scalable, and maintainable** SSO solutions  

### Recommendations

By deploying mTLS authentication using ALB, organizations can:

- Improve SAP application security posture
- Provide better user experience for employees
- Maintain secure and efficient authentication mechanisms

This solution is **especially suitable** for enterprises:
- Running SAP workloads on AWS
- Need secure authentication across application landscape
- Support **RISE with SAP on AWS**

### Best Practices

Always follow [AWS Well Architected Framework (SAP Lens)](https://docs.aws.amazon.com/wellarchitected/latest/sap-lens/sap-lens.html):

- Regularly update certificates and trust store
- Monitor CRLs and certificate rotation
- Conduct periodic security audits
- Use AWS Systems Manager for configuration management

---

## Additional Resources

### AWS for SAP
- [AWS for SAP Blogs](https://aws.amazon.com/blogs/awsforsap/)
- [SAP on AWS Documentation](https://docs.aws.amazon.com/sap/)

### Join Discussion
[AWS re:Post - SAP on AWS](https://repost.aws/tags/TA4IvCeWI1TE-K9P7SDvhh9g/aws-for-sap)

---

## Acknowledgments

The authors would like to thank team members:  
Derek Ewell, Sreenath Middhi, Rajendra Narikimelli, Joachim Aumman, Arne Knoeller, and Adam Hill

---

## About the Authors

**Ferry Mulyadi** - Senior Solutions Architect at AWS, specializing in SAP and Enterprise Applications with over 15 years of experience helping customers design and implement scalable, secure solutions on AWS.

**Mohit Biyani** - Principal Solutions Architect at AWS, focusing on security and DevOps for enterprise workloads. He helps customers build secure, automated, and efficient cloud architectures.

**Krishnakumar Ramadoss** - Senior Solutions Architect at AWS, specializing in networking and load balancing solutions. He works with customers to design highly available and scalable network architectures.

---

**Tags**: #saponaws, #security, RISE with SAP, Application Load Balancer, mTLS, Principal Propagation, Single Sign-On
