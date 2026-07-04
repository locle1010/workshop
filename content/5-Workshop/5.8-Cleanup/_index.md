---
title: "Clean Up Resources"
date: 2026-07-04
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

#### Overview

> **Important:** After completing the workshop, **always delete the AWS resources** to avoid unexpected charges.

Follow the steps **in exact order** below to ensure everything is cleaned up completely.

---

### Step 1: Delete Backend (Lambda + API Gateway + CloudFormation)

Serverless Framework manages the entire backend through CloudFormation. A single command handles everything:

```bash
cd backend
npx serverless remove --stage prod
```

This command will automatically delete:
-  Lambda Function
-  API Gateway (HTTP API)
-  CloudFormation Stack
-  Temporary S3 deployment bucket
-  CloudWatch Log Groups

![Delete Backend via Serverless Framework](/images/5-Workshop/5.8-Cleanup/5.8.1.png)

---

### Step 2: Delete Frontend S3 Bucket

```bash
# Delete all content in the bucket first
aws s3 rm s3://ai-assistant-frontend-prod --recursive

# Delete the bucket
aws s3api delete-bucket \
  --bucket ai-assistant-frontend-prod \
  --region ap-southeast-1
```

![Delete Frontend S3 Bucket content](/images/5-Workshop/5.8-Cleanup/5.8.2.png)

![Frontend S3 Bucket deleted](/images/5-Workshop/5.8-Cleanup/5.8.3.png)

---

### Step 3: Delete Upload S3 Bucket

```bash
aws s3 rm s3://ai-assistant-uploads --recursive
aws s3api delete-bucket \
  --bucket ai-assistant-uploads \
  --region ap-southeast-1
```

![Delete Upload S3 Bucket](/images/5-Workshop/5.8-Cleanup/5.8.4.png)

---

### Step 4: Delete CloudFront Distribution

CloudFront must be **Disabled** before it can be deleted. This process takes ~5-15 minutes.

> **Recommendation:** Do this via Console to monitor the status more easily:
> CloudFront → Distributions → Select distribution → **Disable** → Wait until status = `Disabled` → **Delete**

![Select distribution to delete](/images/5-Workshop/5.8-Cleanup/5.8.5.png)

![Disable Distribution](/images/5-Workshop/5.8-Cleanup/5.8.6.png)

![Wait for status to change to Disabled](/images/5-Workshop/5.8-Cleanup/5.8.7.png)

![Distribution deleted successfully](/images/5-Workshop/5.8-Cleanup/5.8.8.png)

---

### Step 5: Delete DynamoDB Table

```bash
aws dynamodb delete-table \
  --table-name AIAssistant \
  --region ap-southeast-1
```

![Delete DynamoDB Table](/images/5-Workshop/5.8-Cleanup/5.8.9.png)

Verify deletion:
```bash
aws dynamodb list-tables \
  --region ap-southeast-1 \
  --query "TableNames"
```

![Confirm DynamoDB Table deleted](/images/5-Workshop/5.8-Cleanup/5.8.10.png)

---

### Step 6: Delete Cognito User Pool

```bash
# Delete App Client first
aws cognito-idp delete-user-pool-client \
  --user-pool-id <UserPoolId> \
  --client-id <ClientId>

# Delete custom domain
aws cognito-idp delete-user-pool-domain \
  --domain "ai-assistant-auth" \
  --user-pool-id <UserPoolId>

# Delete User Pool
aws cognito-idp delete-user-pool \
  --user-pool-id <UserPoolId>
```

![Delete Cognito User Pool](/images/5-Workshop/5.8-Cleanup/5.8.11.png)

---

### Verify Cleanup Complete

```bash
# Check Lambda
aws lambda list-functions --region ap-southeast-1 \
  --query "Functions[?starts_with(FunctionName,'AIAssistant')]"

# Check S3
aws s3 ls | grep ai-assistant

# Check DynamoDB
aws dynamodb list-tables --region ap-southeast-1 \
  --query "TableNames[?contains(@,'AIAssistant')]"

# Check Cognito
aws cognito-idp list-user-pools \
  --max-results 20 \
  --region ap-southeast-1 \
  --query "UserPools[?contains(Name,'AIAssistant')]"
```

All commands above must return empty `[]` results to confirm cleanup is complete.

![Confirm all resources have been deleted](/images/5-Workshop/5.8-Cleanup/5.8.12.png)
