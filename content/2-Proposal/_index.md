---
title: "Proposal"
date: 2026-07-04
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# AI Assistant — Deploying an AI Application to AWS Cloud
## A Serverless AWS Solution for an AI-Powered Virtual Assistant

---

### 1. Summary

**AI Assistant** is a virtual assistant application combining a Desktop App (Unity) and a Web App (React), allowing users to chat with AI (Google Gemini), manage their profiles, and sync data across platforms.

This project deploys the entire system to **AWS Cloud** using a modern **Serverless** architecture — no server management required, automatically scalable, and you only pay for what you use.

---

### 2. Problem & Solution

#### Current Problem

The AI Assistant application currently only runs locally on the user's computer (Unity Desktop App), lacking:
+ A Backend API for centralized logic processing and data storage
+ A secure user authentication system
+ A Web interface for access from any device
+ Data synchronization between the Desktop App and Web
+ Automated pipeline for deployment (CI/CD), logging, auditing, and platform security baseline detection

#### Solution

Deploy the entire system to AWS with a Serverless architecture and secure practices:

| Component | AWS Solution |
|-----------|-------------|
| User Authentication | **Amazon Cognito** — Hosted UI + JWT Token |
| Backend API | **AWS Lambda** + **API Gateway** (Node.js/Express) |
| Database | **Amazon DynamoDB** — Single-Table Design |
| Image Storage | **Amazon S3** (Upload Bucket) |
| Frontend Hosting | **Amazon S3** + **Amazon CloudFront** (CDN) |
| Domain & SSL | **AWS Route 53** + **AWS ACM** (optional) |
| IAM Principle of Least Privilege | **AWS IAM** — IAM Deployer & Service-Linked roles |
| Secrets Management | **AWS KMS** + **AWS Secrets Manager** — Encrypt API Key |
| Security Threat Detection | **Amazon GuardDuty** + **AWS Config** — Auditing and monitoring |
| S3 Lifecycle Policy | **S3 Lifecycle Rules** — Auto-transition to S3 Glacier |
| Web Application Firewall | **AWS WAF** — Protecting CloudFront CDN from web attacks |
| Automated CI/CD | **AWS CodePipeline** + **AWS CodeBuild** — GitOps from GitHub |
| Automated Database Backups | **AWS Backup** — Automated daily backup for DynamoDB |
| Logging & Monitoring | **Amazon CloudWatch** — Log groups and alarms for Lambda |

---

### 3. System Architecture

The AI Assistant system uses a Serverless architecture integrating all 17 AWS services:

| Service | Role |
|---------|------|
| **Amazon Cognito** | User authentication and authorization |
| **AWS Lambda** | Run Backend API without servers |
| **Amazon API Gateway** | HTTP API endpoint connecting to Lambda |
| **Amazon DynamoDB** | NoSQL database for data storage |
| **Amazon S3** | Frontend file storage and image uploads |
| **Amazon CloudFront** | CDN for global Frontend distribution |
| **AWS Route 53** | DNS management and custom domains |
| **AWS ACM** | SSL/TLS Certificate for HTTPS |
| **AWS IAM** | User access credentials and role configurations |
| **AWS KMS** | Customer-managed key to encrypt secret strings |
| **AWS Secrets Manager** | Secure storage and encryption of Google Gemini API Key |
| **Amazon CloudWatch** | Logs ingestion and CloudWatch Alarm for error notification |
| **Amazon GuardDuty** | Continuous security threat detection |
| **AWS Config** | Resource auditing and configuration tracking |
| **AWS WAF** | Web Access Control Lists protecting CloudFront CDN |
| **AWS Backup** | Centralized backups coordinator for DynamoDB |
| **AWS CodePipeline** | Release pipeline pipeline orchestrator |
| **AWS CodeBuild** | Managed code compilation and deployment script runner |

![AI Assistant System Architecture](/images/2-Proposal/architec.jpg)

---

### 4. Implementation Plan

#### Phase 1: Research & Design

+ Survey AWS services suitable for the project requirements (Cognito, Lambda, DynamoDB, S3, CloudFront)
+ Design the overall Serverless system architecture
+ Define Single-Table Design for DynamoDB — determine `PK`/`SK` for each entity
+ Design the authentication flow (Cognito Hosted UI → JWT → API Gateway)
+ Design the REST API Backend (endpoints, request/response structure)

#### Phase 2: Backend Development

+ Build the Backend API with Node.js/Express running on AWS Lambda
+ Integrate **Amazon Cognito** for user authentication and authorization
+ Integrate **Amazon DynamoDB** to store user data and AI assistant profiles
+ Integrate **Amazon S3** for the image upload feature
+ Integrate **Google Gemini API** for the AI chat feature
+ Integrate **AWS KMS** and **AWS Secrets Manager** to secure configuration credentials
+ Configure **Serverless Framework** to deploy Lambda and API Gateway

#### Phase 3: Frontend & Desktop App Development

+ Build the Web App with React (Vite) and Cognito Hosted UI
+ Develop features: AI chat, profile management, image upload
+ Configure the Unity Desktop App to connect to the Backend API and Cognito
+ Implement two-way data synchronization between Desktop App and Web App

#### Phase 4: Deployment, Security & Automation

+ Deploy Backend to AWS Lambda via Serverless Framework
+ Deploy Frontend to S3 + CloudFront
+ Configure custom domain and SSL certificate (Route 53 & ACM)
+ Configure **AWS CodePipeline** and **AWS CodeBuild** for automated CI/CD from GitHub `main` branch
+ Configure security layers using **Amazon GuardDuty**, **AWS Config**, and **AWS WAF**
+ Set up **AWS Backup** for DynamoDB and **CloudWatch Alarms** for error detection

---

### 5. Cost Estimation

> Detailed cost estimation exported from **AWS Pricing Calculator** based on experimental usage levels (Calculator ID: `c964d0cc-491c-4141-a922-fa56a58fae21`).

| Service | Estimated Configuration Details | Estimated Cost (USD/month) |
|---------|--------------------------------|---------------------------|
| **AWS WAF** | 1 WebACL ($5.00) + 1 Rule ($1.00) + 1,000 requests | $6.00 |
| **Amazon CloudFront** | 20 GB Outbound Data Transfer + 1,000 HTTPS Requests | $2.65 |
| **Amazon CloudWatch** | 5 Custom Metrics ($1.50) + 2 Alarms ($0.20) + 0.15 GB Log storage | $1.70 |
| **AWS CodePipeline** | 1 Active Pipeline | $1.00 |
| **AWS Backup** | DynamoDB table backups (~7 GB-month Warm Storage) | $0.79 |
| **Amazon Route 53** | 1 Hosted Zone ($0.50) + 1,000 DNS Queries | $0.50 |
| **AWS Secrets Manager** | 1 Secret ($0.40) + 1,000 API Requests | $0.41 |
| **AWS Config** | 50 Configuration Items Recorded + 10 Rule Evaluations | $0.16 |
| **Amazon S3** | 5 GB-month Standard Storage + 1,000 Tier 1 Requests | $0.13 |
| **AWS CodeBuild** | 10 build minutes (Linux ARM) | $0.02 |
| **Amazon Cognito** | Cognito User Pool MAU tracking (1 MAU) | $0.01 |
| **Amazon GuardDuty** | Analyze 1,000 S3 + CloudTrail log events | $0.01 |
| **Amazon API Gateway** | 1,000 REST/HTTP API Requests | $0.01 |
| **AWS Lambda** | 1,000 Requests (ARM64, 50 GB-seconds) | $0.00 (Within Free Tier) |
| **AWS KMS** | 1,000 decrypt API key requests | $0.00 (Within Free Tier) |
| **Amazon DynamoDB** | 10 GB Storage + 1,000 WCU + 500 RCU | $0.00 (Within Free Tier) |

**Total Estimated Cost:** **~$13.39 USD/month**

---

### 6. Risk Assessment

| Risk | Level | Mitigation |
|------|-------|------------|
| Cognito authentication flow complexity (OAuth 2.0, callback URL, token handling) | High | Study AWS Cognito documentation thoroughly; test each step in isolation |
| Data sync inconsistency between Desktop App and Web | High | Design a clear API contract; use DynamoDB as the single source of truth |
| Gemini API key exposure in public repository | High | Use AWS Secrets Manager and AWS KMS to encrypt and load the API key at runtime |
| CI/CD pipeline deployment failures | Medium | Verify buildspec files locally; provision correct IAM permissions to CodeBuild role |
| Unexpected costs from forgot-to-delete resources or traffic spikes | Medium | Configure AWS Budget Alerts; set up CloudWatch Alarms; run clean up in Step 5.9 |

---

### 7. Expected Outcomes

+ A complete **AI Assistant** system running on AWS Cloud with Serverless architecture
+ **Web App** accessible from any device via CloudFront URL protected by AWS WAF
+ **Desktop App (Unity)** syncing data to the Cloud in real time
+ Users can **register and log in** securely via Cognito Hosted UI
+ **CI/CD Automated** deployments running instantly on Git push
+ **Data archive policies, backup, security audits, and secrets protection** configured cleanly

---

### 8. Source Code & Live Demo

*   **Project Source Code (GitHub):** [https://github.com/LeQu0cAnh/PetWebAWSProject](https://github.com/LeQu0cAnh/PetWebAWSProject)
*   **Workshop Documentation (GitHub Pages):** [https://locle1010.github.io/workshop/](https://locle1010.github.io/workshop/)
*   **Application Live Demo (Web):** [https://aa.locle1010.dpdns.org](https://aa.locle1010.dpdns.org)