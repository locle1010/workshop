---
title: "AI Assistant Workshop"
date: 2026-07-04
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Deploy AI Assistant to AWS Cloud

#### Overview

**AI Assistant** is an AI-integrated virtual assistant application. This workshop guides you step-by-step through deploying the entire system onto AWS using a modern **Serverless** architecture — no server management required, auto-scaling, and cost-optimized.

All steps in this workshop prioritize using the **AWS CLI**. In cases where CLI is impractical (complex configuration, CLI errors), alternative instructions via the **AWS Web Console** will be provided.

#### Content

1. [Introduction & Architecture](5.1-Introduction/)
2. [Environment Setup & Security Baseline](5.2-Prerequisite/)
3. [Setup Cognito, DynamoDB & AWS Backup](5.3-Database/)
4. [Setup KMS, Secrets Manager, S3 & Deploy Backend](5.4-Backend/)
5. [Deploy Frontend to CloudFront & Configure AWS WAF](5.5-Frontend/)
6. [Configure Custom Domain (Optional)](5.6-Domain/)
7. [Setup Automated CI/CD](5.7-CICD/)
8. [Install Unity App](5.8-UnityApp/)
9. [Clean Up Resources](5.9-Cleanup/)