---
title: "Introduction"
date: 2026-07-04
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

#### Introduction to the AI Assistant Workshop

In this workshop, you will deploy the entire **AI Assistant** system onto AWS Cloud using a modern **Serverless** architecture — no server management required, automatically scalable, and you only pay for what you use.

#### About the AI Assistant Project

**AI Assistant** is an AI-integrated application with the following features:

+  Chat with AI (Google Gemini)
+  Data synchronization between Desktop App (Unity) and Web
+  Data storage on AWS

#### System Architecture

The AI Assistant system uses the following AWS services:

| Service | Role |
|---------|------|
| **Amazon Cognito** | User authentication and authorization |
| **AWS Lambda** | Run Backend API (Node.js/Express) without servers |
| **API Gateway** | HTTP API endpoint connecting to Lambda |
| **Amazon DynamoDB** | NoSQL database for data storage |
| **Amazon S3** | Static file storage for Frontend and uploaded images |
| **Amazon CloudFront** | CDN for global Frontend distribution |
| **AWS Route 53** | DNS management and domain names |
| **AWS ACM** | SSL/TLS Certificate for HTTPS |

#### Workshop Overview

In this workshop, you will go through the following steps:

1. **Environment Setup** — Install Git, Node.js, AWS CLI, and Serverless Framework
2. **Setup Cognito** — Create a User Pool for user authentication
3. **Setup DynamoDB** — Create the database table
4. **Setup S3 Upload** — Configure bucket for image storage
5. **Deploy Backend** — Deploy the API to AWS Lambda via Serverless Framework
6. **Deploy Frontend** — Build React app and host on S3 + CloudFront
7. **Configure Domain** — Attach custom domain and SSL certificate (optional)
8. **Clean Up** — Delete resources to avoid unwanted charges

> **Note:** This workshop prioritizes using **AWS CLI** for all steps. In cases where CLI is impractical (complex config, CLI errors), alternative instructions via **AWS Console** will be provided.

![AI Assistant System Architecture](/images/5-Workshop/5.1-Introduction/architec.jpg)
