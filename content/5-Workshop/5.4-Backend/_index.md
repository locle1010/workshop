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

### Setup Upload S3 Bucket

#### Create Bucket

```bash
aws s3api create-bucket \
  --bucket ai-assistant-uploads \
  --region ap-southeast-1 \
  --create-bucket-configuration LocationConstraint=ap-southeast-1
```

> **Note:** S3 Bucket names are **globally unique**. If you encounter a `BucketAlreadyExists` error, add a unique suffix to the name (e.g., `ai-assistant-uploads-2026`).

![Create S3 Bucket](/images/5-Workshop/5.4-Backend/5.4.1.png)

#### Configure CORS

Create a `cors.json` file at the root of your project:
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

![Create cors.json](/images/5-Workshop/5.4-Backend/5.4.2.png)

Apply the configuration:
```bash
aws s3api put-bucket-cors \
  --bucket ai-assistant-uploads \
  --cors-configuration file://cors.json
```

![Apply CORS Configuration](/images/5-Workshop/5.4-Backend/5.4.3.png)

#### Disable Block Public Access

```bash
aws s3api put-public-access-block \
  --bucket ai-assistant-uploads \
  --public-access-block-configuration \
    "BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=false,RestrictPublicBuckets=false"
```

![Disable Block Public Access](/images/5-Workshop/5.4-Backend/5.4.4.png)

#### Grant Public Read Access

Create a `bucket-policy.json` file:
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

![Create bucket-policy.json](/images/5-Workshop/5.4-Backend/5.4.5.png)

Apply the policy:
```bash
aws s3api put-bucket-policy \
  --bucket ai-assistant-uploads \
  --policy file://bucket-policy.json
```

![Apply Bucket Policy](/images/5-Workshop/5.4-Backend/5.4.6.png)

**Verify Bucket Configuration:**
```bash
aws s3api get-bucket-cors --bucket ai-assistant-uploads
```

![Verify CORS settings](/images/5-Workshop/5.4-Backend/5.4.7.png)

**[AWS Console]:**
> + Go to the [S3 Console](https://s3.console.aws.amazon.com/s3/home)

![S3 Console Home](/images/5-Workshop/5.4-Backend/5.4.8.png)

> + Click **Create bucket** → Name: `ai-assistant-uploads`, Region: `ap-southeast-1`

![Create Bucket via Console](/images/5-Workshop/5.4-Backend/5.4.9.png)

> + Open **Permissions** tab → Turn off "Block all public access"

![Disable Block all public access](/images/5-Workshop/5.4-Backend/5.4.10.png)

> + Click **Create bucket**

![Bucket created successfully](/images/5-Workshop/5.4-Backend/5.4.11.png)

> + Open **Permissions** tab → scroll down to **Cross-origin resource sharing (CORS)** → Paste the CORS JSON code above

![Configure CORS via Console](/images/5-Workshop/5.4-Backend/5.4.12.png)

![Save CORS Configuration](/images/5-Workshop/5.4-Backend/5.4.13.png)

![Verify CORS is applied](/images/5-Workshop/5.4-Backend/5.4.14.png)

---

### Deploy Backend Lambda

#### Configure `.env.production`

Navigate to the backend directory:
```bash
cd backend
```

Create `backend/.env.production` (copy from `.env.production.example` and populate it):

```env
NODE_ENV=production

# --- DynamoDB ---
DYNAMODB_TABLE=AIAssistant

# --- Amazon Cognito (from Step 5.3) ---
COGNITO_REGION=ap-southeast-1
COGNITO_USER_POOL_ID=ap-southeast-1_XXXXXXXXX
COGNITO_CLIENT_ID=XXXXXXXXXXXXXXXXXXXXX

# --- CORS --- (Temporary set to *, update after obtaining CloudFront URL)
FRONTEND_URL=*

# --- AWS S3 ---
AWS_S3_BUCKET=ai-assistant-uploads
AWS_S3_REGION=ap-southeast-1

# --- Google Gemini AI ---
# Generate key at: https://aistudio.google.com/app/apikey
GEMINI_API_KEY=<Your-Gemini-API-Key>
```

![Configure .env.production](/images/5-Workshop/5.4-Backend/5.4.15.png)

#### Install Dependencies

```bash
npm install
npm install --save-dev serverless-dotenv-plugin
```

#### Deploy to AWS

```bash
npx serverless deploy --stage prod
```

The deployment process will:
1. Package the source code into a ZIP archive
2. Upload it to a temporary S3 deployment bucket
3. Create/update the CloudFormation Stack
4. Create the Lambda Function `petweb-backend-prod-api`
5. Create the HTTP API Gateway with a public endpoint

> **Initial Deployment:** Takes ~3–5 minutes

![Deploy Backend to AWS](/images/5-Workshop/5.4-Backend/5.4.16.png)

#### Record the Endpoint URL

Once the deployment completes successfully, the terminal will display:
```
 Service deployed to stack petweb-backend-prod (45s)

endpoint: ANY - https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com
functions:
  api: petweb-backend-prod-api (6.2 MB)
```

 **Record the endpoint URL** — this is the Backend API URL to be used in the next step.

---

#### Secure Sensitive Configurations with AWS KMS & AWS Secrets Manager (Recommended)

Storing secrets (like `GEMINI_API_KEY`) directly as environment variables in plain text is a security risk. Instead, encrypt the secret using **AWS KMS** and manage it securely through **AWS Secrets Manager**.

##### Step 1: Create a KMS Key (Customer Managed Key)
This KMS key will be used to encrypt your secret.

**Via AWS CLI:**
```bash
aws kms create-key \
  --description "KMS Key for AI Assistant Secrets" \
  --region ap-southeast-1 \
  --query "KeyMetadata.KeyId" \
  --output text
```
*Save the KeyId (UUID format).*

##### Step 2: Store API Key in AWS Secrets Manager
Create a secret to store `GEMINI_API_KEY`, using the KMS key created above for encryption.

**Via AWS CLI:**
```bash
aws secretsmanager create-secret \
  --name "ai-assistant/gemini-key" \
  --description "Google Gemini API Key for AI Assistant" \
  --secret-string '{"GEMINI_API_KEY":"YOUR_ACTUAL_GEMINI_API_KEY"}' \
  --kms-key-id <Your-KMS-Key-Id> \
  --region ap-southeast-1
```
*Replace YOUR_ACTUAL_GEMINI_API_KEY with your real API key.*

##### Step 3: Configure Permissions (IAM Policy) for Lambda to Read the Secret
Your Lambda function needs permissions to decrypt using the KMS key and read the secret value from Secrets Manager. In your `serverless.yml`, add these permissions:

```yaml
provider:
  name: aws
  runtime: nodejs20.x
  region: ap-southeast-1
  iam:
    role:
      statements:
        - Effect: Allow
          Action:
            - secretsmanager:GetSecretValue
          Resource: "arn:aws:secretsmanager:ap-southeast-1:123456789012:secret:ai-assistant/gemini-key-*"
        - Effect: Allow
          Action:
            - kms:Decrypt
          Resource: "arn:aws:kms:ap-southeast-1:123456789012:key/<Your-KMS-Key-Id>"
```

##### Step 4: Retrieve the Secret in the Backend Code
In your Node.js backend, instead of reading from `process.env.GEMINI_API_KEY`, use `@aws-sdk/client-secrets-manager` to fetch the secret at runtime:

```javascript
const { SecretsManagerClient, GetSecretValueCommand } = require("@aws-sdk/client-secrets-manager");

async function getGeminiApiKey() {
  if (process.env.NODE_ENV !== "production") {
    return process.env.GEMINI_API_KEY;
  }
  
  const client = new SecretsManagerClient({ region: "ap-southeast-1" });
  const response = await client.send(
    new GetSecretValueCommand({ SecretId: "ai-assistant/gemini-key" })
  );
  const secrets = JSON.parse(response.SecretString);
  return secrets.GEMINI_API_KEY;
}
```

---

#### Monitoring and Alerting with Amazon CloudWatch

By default, when you deploy via Serverless Framework, AWS forwards Lambda logs to **Amazon CloudWatch Logs**.

##### 1. Configure Log Retention Period
By default, logs are stored indefinitely (Never Expire), which can lead to high storage costs. You should set a log retention limit (e.g., 14 days) in `serverless.yml`:

```yaml
provider:
  name: aws
  logRetentionInDays: 14
```

##### 2. Create a CloudWatch Alarm for Lambda Failures
Receive an email notification immediately if the Lambda backend encounters errors continuously:

**Via AWS Console:**
1. Open the **CloudWatch** console.
2. Select **Alarms** -> **All alarms** -> **Create alarm**.
3. Click **Select metric**:
   - Go to **Lambda** -> **By Function Name**.
   - Search for your backend Lambda function, check the **Errors** metric -> click **Select metric**.
4. Configure the Alarm:
   - **Statistic**: Select **Sum**.
   - **Period**: Select **5 minutes**.
   - **Threshold type**: Static.
   - **Whenever Errors is...**: Select **Greater/Equal (>=)** -> enter `5` (trigger if 5 or more errors occur in 5 minutes).
5. Click **Next**.
6. Under **Notification**:
   - Select **In alarm**.
   - Select **Create new topic**: Enter name `ai-assistant-lambda-errors`.
   - **Email endpoints...**: Enter your email address to receive notifications.
   - Click **Create topic** -> click **Next**.
7. Alarm name: `LambdaBackendErrorsAlarm` -> click **Create alarm**.
8. **Important:** Check your email inbox and click **Confirm subscription** in the confirmation email from AWS to activate alerts.

![Create CloudWatch Alarm](/images/5-Workshop/5.4-Backend/5.4.17.png)

---

#### Summary — Information to Save

```
BACKEND_API_URL = https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com
S3_UPLOADS_BUCKET = ai-assistant-uploads
```
