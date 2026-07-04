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

#### Solution

Deploy the entire system to AWS with a Serverless architecture:

| Component | AWS Solution |
|-----------|-------------|
| User Authentication | **Amazon Cognito** — Hosted UI + JWT Token |
| Backend API | **AWS Lambda** + **API Gateway** (Node.js/Express) |
| Database | **Amazon DynamoDB** — Single-Table Design |
| Image Storage | **Amazon S3** (Upload Bucket) |
| Frontend Hosting | **Amazon S3** + **Amazon CloudFront** (CDN) |
| Domain & SSL | **AWS Route 53** + **AWS ACM** (optional) |

---

### 3. System Architecture

The AI Assistant system uses a Serverless architecture with the following AWS services:

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
+ Configure **Serverless Framework** to deploy Lambda and API Gateway

#### Phase 3: Frontend & Desktop App Development

+ Build the Web App with React (Vite) and Cognito Hosted UI
+ Develop features: AI chat, profile management, image upload
+ Configure the Unity Desktop App to connect to the Backend API and Cognito
+ Implement two-way data synchronization between Desktop App and Web App

#### Phase 4: Deployment & Testing

+ Deploy Backend to AWS Lambda via Serverless Framework
+ Deploy Frontend to S3 + CloudFront
+ Test the end-to-end authentication flow (sign up, sign in, token refresh)
+ Test AI chat functionality and data sync between both platforms
+ Configure custom domain and SSL certificate (if applicable)

---

### 5. Cost Estimation

> Estimated costs are based on the AWS Pricing Calculator at low usage levels (workshop/personal use).

| Service | Estimated Cost |
|---------|---------------|
| **AWS Lambda** | $0.00/month (< 1M requests/month free tier) |
| **API Gateway** | ~$0.01/month (1,000 requests) |
| **Amazon DynamoDB** | $0.00/month (On-demand, under free tier) |
| **Amazon S3** | ~$0.02/month (small storage) |
| **Amazon CloudFront** | $0.00/month (first 1 TB free) |
| **Amazon Cognito** | $0.00/month (first 50,000 MAU free) |

**Total:** ~$0.03–0.05 USD/month *(nearly free at low traffic)*

> ⚠️ **Note:** Always delete resources after finishing the workshop to avoid unexpected charges.

---

### 6. Risk Assessment

| Risk | Level | Mitigation |
|------|-------|------------|
| Cognito authentication flow complexity (OAuth 2.0, callback URL, token handling) | High | Study AWS Cognito documentation thoroughly; test each step in isolation |
| Data sync inconsistency between Desktop App and Web | High | Design a clear API contract; use DynamoDB as the single source of truth |
| Gemini API rate limits or unexpected response format changes | Medium | Implement robust error handling in Backend; provide fallback responses |
| Unity REST client CORS issues with API Gateway | Medium | Configure CORS correctly from the start; test early before deep development |
| Poor DynamoDB design making future scaling difficult | Medium | Analyze access patterns carefully before creating the table |
| Unexpected cost spikes from sudden traffic increase | Low | Enable AWS Budget Alerts; monitor CloudWatch Metrics regularly |

---

### 7. Expected Outcomes

+ A complete **AI Assistant** system running on AWS Cloud with Serverless architecture
+ **Web App** accessible from any device via CloudFront URL
+ **Desktop App (Unity)** syncing data to the Cloud in real time
+ Users can **register and log in** securely via Cognito Hosted UI
+ Operating costs **nearly zero** at personal usage levels
+ Platform can be **easily scaled** as the number of users grows

### 8. Source Code & Live Demo

*   **Project Source Code (GitHub):** [https://github.com/LeQu0cAnh/PetWebAWSProject](https://github.com/LeQu0cAnh/PetWebAWSProject)
*   **Workshop Documentation (GitHub Pages):** [https://locle1010.github.io/workshop/](https://locle1010.github.io/workshop/)
*   **Application Live Demo (Web):** [https://aa.locle1010.dpdns.org](https://aa.locle1010.dpdns.org)