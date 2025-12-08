---
title: "Blog 3 - Node.js 22 runtime in AWS Lambda"
date: 2024-11-21
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Node.js 22 runtime now available in AWS Lambda

**Authors**: Julian Wood and Andrea Amorosi  
**Published**: November 21, 2024  
**Categories**: Amazon CloudFront, Announcements, AWS Cloud Development Kit, AWS Lambda, AWS SDK for JavaScript in Node.js, AWS Serverless Application Model, AWS Systems Manager, Lambda@Edge, Serverless

---

## Introduction

You can now develop AWS Lambda functions using the Node.js 22 runtime, a version in active LTS status ready for production use. Node.js 22 includes several language additions, including require() for ES modules, as well as changes to runtime implementation and standard library. With this release, Node.js developers can leverage these new features and improvements when building serverless applications on Lambda.

You can develop Lambda functions with Node.js 22 using AWS Management Console, AWS Command Line Interface (AWS CLI), AWS SDK for JavaScript, AWS Serverless Application Model (AWS SAM), AWS Cloud Development Kit (AWS CDK), and other infrastructure as code tools.

To use this new version, specify the runtime parameter value as `nodejs22.x` when creating or updating functions, or by using the appropriate container base image.

You can use Node.js 22 with Powertools for AWS Lambda (TypeScript), a developer toolkit for implementing serverless best practices and accelerating development. Powertools for AWS Lambda includes libraries to support common tasks like observability, AWS Systems Manager Parameter Store integration, idempotency, batch processing, and more. You can also use Node.js 22 with Lambda@Edge to customize low-latency content delivered through Amazon CloudFront.

This blog post highlights important changes to the Node.js runtime, notable Node.js 22 language updates, and how you can use the new Node.js 22 runtime in your serverless applications.

---

## Node.js 22 Language Updates

Node.js 22 introduces several language updates and features aimed at enhancing developer productivity and improving application performance.

### require() for ECMAScript Modules

This release adds support for loading ECMAScript modules (ESM) using `require()`. You can enable this feature using the `--experimental-require-module` flag through the NODE_OPTIONS environment variable configuration. Support for `require()` for synchronous ESM graphs creates a bridge between CommonJS and ESM, providing more flexibility in module loading. Its important to note this feature is currently experimental and may change in future releases.

### WebSocket Support

WebSocket support, previously available behind the `--experimental-websocket` flag, is now enabled by default in Node.js 22. This brings browser-compatible WebSocket client implementation to Node.js without external dependencies. Native support simplifies building real-time applications and enhances the overall WebSocket experience in Node.js environments.

### Performance Improvements

The new runtime also includes performance improvements for AbortSignal creation. This makes network operations faster and more efficient for the Fetch API and test runner. The Fetch API is also now considered stable in Node.js 22.

### TypeScript Support

For TypeScript users, Node.js 22 introduces experimental support for transforming TypeScript-only syntax into JavaScript code. Using the `--experimental-transform-types` flag, you can enable this feature to support TypeScript syntax like Enum and namespace directly. While you can enable this feature in Lambda, your function entrypoint (i.e., index.mjs or app.cjs) currently cannot be written in TypeScript as the runtime expects a file with JavaScript extension. You can use TypeScript for any other modules imported in your codebase.

For a detailed overview of Node.js 22 language features, see the Node.js 22 release blog post and Node.js 22 changelog.

---

## Experimental Features Not Available

Node.js 22 includes an experimental feature to automatically detect module syntax (CommonJS or ES Modules). This feature must be enabled when the Node.js runtime is compiled. Since the Node.js 22 runtime provided by Lambda is intended for production workloads, this experimental feature is not enabled in the Lambda build and cannot be enabled through execution-time flags. To use this feature in Lambda, you need to deploy your own Node.js runtime using custom runtime or container image with the experimental module syntax detection feature enabled.

---

## Performance Considerations

At launch, new Lambda runtimes receive less usage compared to existing established runtimes. This can lead to longer cold start times due to reduced cache residency in internal Lambda subsystems. Cold start times typically improve in the weeks following launch as usage increases. Therefore, AWS recommends not drawing conclusions from side-by-side performance comparisons with other Lambda runtimes until performance has stabilized. Since performance depends heavily on workload, customers with performance-sensitive workloads should conduct their own testing rather than relying on general test benchmarks.

Builders should continue to measure and test function performance and optimize function code and configuration for any impacts. To learn more about optimizing Node.js performance in Lambda, see Performance optimization in the Lambda Operator Guide, and the Optimizing Node.js dependencies in AWS Lambda blog post.

---

## Migration from Previous Node.js Runtimes

### AWS SDK for JavaScript

Until Node.js 16, Lambda Node.js runtimes included AWS SDK for JavaScript version 2. This was replaced by AWS SDK for JavaScript version 3, released in December 2022. Starting with Node.js 18, and continuing with Node.js 22, Lambda Node.js runtimes include version 3. When upgrading from Node.js 16 or earlier runtimes and using the included version 2, you must upgrade your code to use v3 SDK.

For optimal performance, and to have full control over your code dependencies, we recommend bundling and minifying the AWS SDK in your deployment package rather than using the SDK included in the runtime. For more information, see Optimizing Node.js dependencies in AWS Lambda.

### Amazon Linux 2023

The Node.js 22 runtime is based on the `provided.al2023` runtime, built on the Amazon Linux 2023 minimal container image. Amazon Linux 2023 minimal image uses microdnf as package manager, symlinked as dnf. This replaces the yum package manager used in Node.js 18 and earlier AL2-based images. If you deploy your Lambda function as a container image, you must update your Dockerfile to use dnf instead of yum when upgrading to Node.js 22 base image from Node.js 18 or earlier.

Additionally, AL2 includes curl and gnupg2 as minimal versions curl-minimal and gnupg2-minimal.

Learn more about the provided.al2023 runtime in the Introducing the Amazon Linux 2023 runtime for AWS Lambda blog post and Amazon Linux 2023 launch blog post.

---

## Using Node.js 22 Runtime in AWS Lambda

### AWS Management Console

To use the Node.js 22 runtime to develop your Lambda functions, specify the Node.js 22.x runtime parameter value when creating or updating a function. The Node.js 22 runtime version is now available in the Runtime dropdown on the Create function page in the AWS Lambda console.

![Creating Node.js function in AWS Management Console](/images/3.3-Blog3/image1.png)
*Creating Node.js function in AWS Management Console*

To update an existing Lambda function to Node.js 22, navigate to the function in the Lambda console, then select Node.js 22.x in the Runtime settings panel. The new version of Node.js is available in the Runtime dropdown:

![Changing a function to Node.js 22](/images/3.3-Blog3/image2.png)
*Changing a function to Node.js 22*

### AWS Lambda Container Image

Change the Node.js base image version by modifying the FROM statement in your Dockerfile.

```dockerfile
FROM public.ecr.aws/lambda/nodejs:22
```

### AWS Serverless Application Model (AWS SAM)

In AWS SAM, set the Runtime property to `nodejs22.x` to use this version.

```yaml
Resources:
  MyFunction:
    Type: AWS::Serverless::Function
    Properties:
      Runtime: nodejs22.x
      Handler: index.handler
```

When you add function code directly in the AWS SAM or AWS CloudFormation template as an inline function, it is treated as CommonJS.

AWS SAM supports creating this template with Node.js 22 for new serverless applications using the `sam init` command. See AWS SAM documentation.

### AWS Cloud Development Kit (AWS CDK)

In AWS CDK, set the runtime property to `Runtime.NODEJS_22_X` to use this version.

```typescript
import * as lambda from 'aws-cdk-lib/aws-lambda';

new lambda.Function(this, 'MyFunction', {
  runtime: lambda.Runtime.NODEJS_22_X,
  handler: 'index.handler',
  code: lambda.Code.fromAsset('lambda')
});
```

---

## Conclusion

Lambda now supports Node.js 22 as a managed language runtime. This release uses Amazon Linux 2023 OS as well as other improvements detailed in this blog post.

You can build and deploy functions using Node.js 22 using AWS Management Console, AWS CLI, AWS SDK, AWS SAM, AWS CDK, or your infrastructure as code tool of choice. You can also use the Node.js 22 container base image if you prefer to build and deploy your functions using container images.

The Node.js 22 runtime helps developers build more efficient, robust, and scalable serverless applications. Read about the Node.js programming model in Lambda documentation to learn more about writing functions in Node.js 22. Try the Node.js runtime in Lambda today.

For more serverless learning resources, visit Serverless Land.

---

## About the Authors

**Julian Wood** - Principal Developer Advocate for Serverless at AWS, helping developers build and optimize serverless applications. He is passionate about serverless technologies and shares best practices through blogs, workshops, and community engagement.

**Andrea Amorosi** - Senior Solutions Architect at AWS, specializing in serverless architectures and developer tools. He helps customers design and build scalable, event-driven applications using AWS Lambda and other serverless technologies.

---

**Tags**: serverless, Node.js, AWS Lambda, runtime, Amazon Linux 2023

