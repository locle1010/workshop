---
title: "Setup S3 & Deploy Backend"
date: 2026-07-04
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

#### Overview

In this step, you will:
+ Create an **S3 Bucket** to store user-uploaded images
+ Configure the `.env.production` file for the Backend
+ Deploy the **Backend API** to **AWS Lambda** via **Serverless Framework**

---

### Setup S3 Upload Bucket

#### Create Bucket

```bash
aws s3api create-bucket \
  --bucket ai-assistant-uploads \
  --region ap-southeast-1 \
  --create-bucket-configuration LocationConstraint=ap-southeast-1
```

> **Note:** S3 Bucket names are **globally unique**. If you encounter a `BucketAlreadyExists` error, add a suffix to the name (e.g., `ai-assistant-uploads-2026`).

![Create S3 Bucket](/images/5-Workshop/5.4-Backend/5.4.1.png)

#### Configure CORS

Create a `cors.json` file at the project root:
```json
{
  "CORSRules": [
    {
      "AllowedOrigins": ["*"],
      "AllowedHeaders": ["*"],
      "AllowedMethods": ["GET", "PUT", "POST", "DELETE", "HEAD"],
      "MaxAgeSeconds": 3000
    }
  ]
}
```

![Create cors.json file](/images/5-Workshop/5.4-Backend/5.4.2.png)

Apply:
```bash
aws s3api put-bucket-cors \
  --bucket ai-assistant-uploads \
  --cors-configuration file://cors.json
```

![Apply CORS configuration](/images/5-Workshop/5.4-Backend/5.4.3.png)

#### Disable Block Public Access

```bash
aws s3api put-public-access-block \
  --bucket ai-assistant-uploads \
  --public-access-block-configuration \
    "BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=false,RestrictPublicBuckets=false"
```

![Disable Block Public Access](/images/5-Workshop/5.4-Backend/5.4.4.png)

#### Allow Public Read

Create `bucket-policy.json`:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::ai-assistant-uploads/*"
    }
  ]
}
```

![Create bucket-policy.json file](/images/5-Workshop/5.4-Backend/5.4.5.png)

```bash
aws s3api put-bucket-policy \
  --bucket ai-assistant-uploads \
  --policy file://bucket-policy.json
```

![Apply bucket policy](/images/5-Workshop/5.4-Backend/5.4.6.png)

**Verify bucket configuration:**
```bash
aws s3api get-bucket-cors --bucket ai-assistant-uploads
```

![Verify CORS configuration](/images/5-Workshop/5.4-Backend/5.4.7.png)

**[AWS Console]:**
> + Go to [S3 Console](https://s3.console.aws.amazon.com/s3/home)

![S3 Console](/images/5-Workshop/5.4-Backend/5.4.8.png)

> + **Create bucket** → Name: `ai-assistant-uploads`, Region: `ap-southeast-1`

![Create bucket via Console](/images/5-Workshop/5.4-Backend/5.4.9.png)

> + **Permissions** tab → Disable "Block all public access"

![Disable Block all public access](/images/5-Workshop/5.4-Backend/5.4.10.png)

> + Click **Create bucket**

![Bucket created successfully](/images/5-Workshop/5.4-Backend/5.4.11.png)

> + **Permissions** tab → **Cross-origin resource sharing (CORS)** → Paste the CORS JSON above

![Configure CORS via Console](/images/5-Workshop/5.4-Backend/5.4.12.png)

![Save CORS configuration](/images/5-Workshop/5.4-Backend/5.4.13.png)

![CORS applied successfully](/images/5-Workshop/5.4-Backend/5.4.14.png)

---

### Deploy Backend Lambda

#### Configure `.env.production` File

Navigate to the backend directory:
```bash
cd backend
```

Create `backend/.env.production` (copy from `.env.production.example` and fill in the values):

```env
NODE_ENV=production

# --- DynamoDB ---
DYNAMODB_TABLE=AIAssistant

# --- Amazon Cognito (from Step 5.3) ---
COGNITO_REGION=ap-southeast-1
COGNITO_USER_POOL_ID=ap-southeast-1_XXXXXXXXX
COGNITO_CLIENT_ID=XXXXXXXXXXXXXXXXXXXXX

# --- CORS --- (set to * for now, update after getting CloudFront URL)
FRONTEND_URL=*

# --- AWS S3 ---
AWS_S3_BUCKET=ai-assistant-uploads
AWS_S3_REGION=ap-southeast-1

# --- Google Gemini AI ---
# Get at: https://aistudio.google.com/app/apikey
GEMINI_API_KEY=<Your-Gemini-API-Key>
```

![Configure .env.production file](/images/5-Workshop/5.4-Backend/5.4.15.png)

#### Install Dependencies

```bash
npm install
npm install --save-dev serverless-dotenv-plugin
```

#### Deploy to AWS

```bash
npx serverless deploy --stage prod
```

The deploy process will:
1.  Package all code into a ZIP file
2.  Upload to a temporary S3 deployment bucket
3.  Create/update CloudFormation Stack
4.  Create Lambda Function `petweb-backend-prod-api`
5.  Create HTTP API Gateway with a public endpoint

> **First deployment:** ~3–5 minutes

![Deploy Backend to AWS](/images/5-Workshop/5.4-Backend/5.4.16.png)

#### Save the Endpoint URL

After successful deployment, the terminal displays:
```
 Service deployed to stack petweb-backend-prod (45s)

endpoint: ANY - https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com
functions:
  api: petweb-backend-prod-api (6.2 MB)
```

 **Save the endpoint URL** — this is the Backend API URL to be used in the next step.

#### Summary — Information to Save

```
BACKEND_API_URL = https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com
S3_UPLOADS_BUCKET = ai-assistant-uploads
```
