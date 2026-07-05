---
title: "Thiết lập S3 & Deploy Backend"
date: 2026-07-04
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

#### Tổng quan

Trong bước này, bạn sẽ:
+ Tạo **S3 Bucket** lưu trữ ảnh upload của người dùng
+ Cấu hình file `.env.production` cho Backend
+ Deploy **Backend API** lên **AWS Lambda** thông qua **Serverless Framework**

---

### Thiết lập S3 Bucket Upload

#### Tạo Bucket

```bash
aws s3api create-bucket \
  --bucket ai-assistant-uploads \
  --region ap-southeast-1 \
  --create-bucket-configuration LocationConstraint=ap-southeast-1
```

> **Lưu ý:** Tên S3 Bucket là **toàn cầu duy nhất**. Nếu gặp lỗi `BucketAlreadyExists`, hãy thêm suffix vào tên (ví dụ: `ai-assistant-uploads-2026`).

![Tạo S3 Bucket](/images/5-Workshop/5.4-Backend/5.4.1.png)

#### Cấu hình CORS

Tạo file `cors.json` tại thư mục gốc dự án:
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

![Tạo file cors.json](/images/5-Workshop/5.4-Backend/5.4.2.png)

Áp dụng:
```bash
aws s3api put-bucket-cors \
  --bucket ai-assistant-uploads \
  --cors-configuration file://cors.json
```

![Áp dụng cấu hình CORS](/images/5-Workshop/5.4-Backend/5.4.3.png)

#### Tắt Block Public Access

```bash
aws s3api put-public-access-block \
  --bucket ai-assistant-uploads \
  --public-access-block-configuration \
    "BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=false,RestrictPublicBuckets=false"
```

![Tắt Block Public Access](/images/5-Workshop/5.4-Backend/5.4.4.png)

#### Cho phép đọc công khai

Tạo file `bucket-policy.json`:
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

![Tạo file bucket-policy.json](/images/5-Workshop/5.4-Backend/5.4.5.png)

```bash
aws s3api put-bucket-policy \
  --bucket ai-assistant-uploads \
  --policy file://bucket-policy.json
```

![Áp dụng bucket policy](/images/5-Workshop/5.4-Backend/5.4.6.png)

**Xác nhận bucket đã cấu hình:**
```bash
aws s3api get-bucket-cors --bucket ai-assistant-uploads
```

![Xác nhận cấu hình CORS](/images/5-Workshop/5.4-Backend/5.4.7.png)

**[AWS Console]:**
> + Truy cập [S3 Console](https://s3.console.aws.amazon.com/s3/home)

![S3 Console](/images/5-Workshop/5.4-Backend/5.4.8.png)

> + **Create bucket** → Tên: `ai-assistant-uploads`, Region: `ap-southeast-1`

![Tạo bucket qua Console](/images/5-Workshop/5.4-Backend/5.4.9.png)

> + Tab **Permissions** → Tắt "Block all public access"

![Tắt Block all public access](/images/5-Workshop/5.4-Backend/5.4.10.png)

> + Chọn **Create bucket**

![Tạo bucket thành công](/images/5-Workshop/5.4-Backend/5.4.11.png)

> + Tab **Permissions** → **Cross-origin resource sharing (CORS)** → Paste JSON CORS ở trên

![Cấu hình CORS qua Console](/images/5-Workshop/5.4-Backend/5.4.12.png)

![Lưu cấu hình CORS](/images/5-Workshop/5.4-Backend/5.4.13.png)

![Xác nhận CORS đã được áp dụng](/images/5-Workshop/5.4-Backend/5.4.14.png)

---

### Deploy Backend Lambda

#### Cấu hình file `.env.production`

Di chuyển vào thư mục backend:
```bash
cd backend
```

Tạo file `backend/.env.production` (copy từ `.env.production.example` và điền thông tin):

```env
NODE_ENV=production

# --- DynamoDB ---
DYNAMODB_TABLE=AIAssistant

# --- Amazon Cognito (lấy từ Bước 5.3) ---
COGNITO_REGION=ap-southeast-1
COGNITO_USER_POOL_ID=ap-southeast-1_XXXXXXXXX
COGNITO_CLIENT_ID=XXXXXXXXXXXXXXXXXXXXX

# --- CORS --- (tạm để *, cập nhật sau khi có CloudFront URL)
FRONTEND_URL=*

# --- AWS S3 ---
AWS_S3_BUCKET=ai-assistant-uploads
AWS_S3_REGION=ap-southeast-1

# --- Google Gemini AI ---
# Lấy tại: https://aistudio.google.com/app/apikey
GEMINI_API_KEY=<Your-Gemini-API-Key>
```

![Cấu hình file .env.production](/images/5-Workshop/5.4-Backend/5.4.15.png)

#### Cài đặt dependencies

```bash
npm install
npm install --save-dev serverless-dotenv-plugin
```

#### Deploy lên AWS

```bash
npx serverless deploy --stage prod
```

Quá trình deploy sẽ:
1.  Đóng gói toàn bộ code thành file ZIP
2.  Upload lên S3 deployment bucket tạm thời
3.  Tạo/cập nhật CloudFormation Stack
4.  Tạo Lambda Function `petweb-backend-prod-api`
5.  Tạo HTTP API Gateway với endpoint công khai

> **Lần đầu deploy:** ~3–5 phút

![Deploy Backend lên AWS](/images/5-Workshop/5.4-Backend/5.4.16.png)

#### Lưu lại Endpoint URL

Sau khi deploy thành công, terminal hiển thị:
```
 Service deployed to stack petweb-backend-prod (45s)

endpoint: ANY - https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com
functions:
  api: petweb-backend-prod-api (6.2 MB)
```

 **Lưu lại URL endpoint** — đây là Backend API URL sẽ dùng ở bước tiếp theo.

---

#### Bảo mật thông tin cấu hình nhạy cảm với AWS KMS & AWS Secrets Manager (Khuyên dùng)

Lưu trữ các khóa bí mật (như `GEMINI_API_KEY`) trực tiếp trong biến môi trường dưới dạng văn bản thuần túy (plain text) là một rủi ro bảo mật lớn. Thay vào đó, chúng ta sẽ mã hóa khóa bí mật bằng **AWS KMS** và quản lý nó thông qua **AWS Secrets Manager**.

##### Bước 1: Tạo KMS Key (Customer Managed Key)
Khóa KMS này được dùng để mã hóa Secret của bạn.

**Qua AWS CLI:**
```bash
aws kms create-key \
  --description "KMS Key for AI Assistant Secrets" \
  --region ap-southeast-1 \
  --query "KeyMetadata.KeyId" \
  --output text
```
*Lưu lại KeyId (dạng UUID).*

![Tạo KMS Key thành công](/images/5-Workshop/5.4-Backend/5.4.17.png)

##### Bước 2: Lưu API Key vào AWS Secrets Manager
Chúng ta sẽ tạo một Secret để chứa `GEMINI_API_KEY`, sử dụng khóa KMS vừa tạo để mã hóa.

**Qua AWS CLI:**
```bash
aws secretsmanager create-secret \
  --name "ai-assistant/gemini-key" \
  --description "Google Gemini API Key for AI Assistant" \
  --secret-string '{"GEMINI_API_KEY":"YOUR_ACTUAL_GEMINI_API_KEY"}' \
  --kms-key-id <Your-KMS-Key-Id> \
  --region ap-southeast-1
```
*Thay thế YOUR_ACTUAL_GEMINI_API_KEY bằng khóa API thật của bạn.*

![Lưu API Key vào Secrets Manager](/images/5-Workshop/5.4-Backend/5.4.18.png)

##### Bước 3: Cấu hình phân quyền (IAM Policy) cho Lambda để đọc Secret
Lambda của bạn cần được cấp quyền giải mã khóa KMS và đọc Secret từ Secrets Manager. Trong Serverless Framework, cấu hình quyền này trong file `serverless.yml`:

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

##### Bước 4: Đọc Secret trong Source Code của Backend
Trong backend NodeJS, thay vì đọc từ `process.env.GEMINI_API_KEY`, sử dụng thư viện `@aws-sdk/client-secrets-manager` để lấy giá trị tại thời điểm chạy (runtime):

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

#### Giám sát và Cảnh báo với Amazon CloudWatch

Mặc định, khi ứng dụng deploy qua Serverless Framework, AWS sẽ tự động gửi log của Lambda về **Amazon CloudWatch Logs**.

##### 1. Cấu hình thời gian lưu trữ Log (Log Retention)
Mặc định, log sẽ được lưu trữ mãi mãi (Never Expire), điều này có thể phát sinh chi phí lớn theo thời gian. Cần thiết lập thời gian lưu trữ log ngắn lại (ví dụ: 14 ngày) trong `serverless.yml`:

```yaml
provider:
  name: aws
  logRetentionInDays: 14
```

##### 2. Tạo CloudWatch Alarm cảnh báo lỗi Lambda
Để nhận email thông báo ngay lập tức nếu Lambda bị lỗi liên tục (ví dụ: lỗi gọi API Gemini hoặc lỗi database):

**Thao tác qua AWS Console:**
1. Tìm kiếm và mở dịch vụ **CloudWatch**.
2. Chọn **Alarms** -> **All alarms** -> **Create alarm**.
![Tạo CloudWatch Alarm](/images/5-Workshop/5.4-Backend/5.4.19.png)
![Thiết lập Metric cho Alarm](/images/5-Workshop/5.4-Backend/5.4.20.png)
3. Chọn **Select metric**:
![Chọn Metric từ Lambda](/images/5-Workshop/5.4-Backend/5.4.21.png)
   - Chọn mục **Lambda** -> **By Function Name**.
   ![Tìm kiếm Lambda Function](/images/5-Workshop/5.4-Backend/5.4.22.png)
   ![Chọn Metric Errors](/images/5-Workshop/5.4-Backend/5.4.23.png)
   - Tìm kiếm hàm Lambda backend của bạn, tích chọn metric **Errors** -> nhấn **Select metric**.
   ![Xác nhận Metric đã chọn](/images/5-Workshop/5.4-Backend/5.4.24.png)
4. Cấu hình Alarm:
   - **Statistic**: Chọn **Sum**.
   - **Period**: Chọn **5 minutes**.
   - **Threshold type**: Static.
   - **Whenever Errors is...**: Chọn **Greater/Equal (>=)** -> điền `5` (cảnh báo khi có từ 5 lỗi trở lên trong 5 phút).
   ![Cấu hình điều kiện cảnh báo](/images/5-Workshop/5.4-Backend/5.4.25.png)
5. Nhấn **Next**.
6. Tại mục **Notification** (Cảnh báo):
   - Chọn **In alarm**.
   - Chọn **Create new topic** (Tạo chủ đề SNS mới): Nhập tên `ai-assistant-lambda-errors`.
   - **Email endpoints...**: Nhập địa chỉ email nhận cảnh báo của bạn.
   - Nhấn **Create topic** -> Nhấn **Next**.
   ![Cấu hình Notification gửi email](/images/5-Workshop/5.4-Backend/5.4.26.png)
   ![Tạo SNS Topic mới](/images/5-Workshop/5.4-Backend/5.4.27.png)
7. Điền tên Alarm: `LambdaBackendErrorsAlarm` -> Nhấn **Create alarm**.
![Xem lại cấu hình Alarm](/images/5-Workshop/5.4-Backend/5.4.28.png)
![Alarm được tạo thành công](/images/5-Workshop/5.4-Backend/5.4.29.png)
8. **Quan trọng:** Kiểm tra hòm thư email của bạn và nhấn link **Confirm subscription** trong email gửi từ AWS để xác nhận nhận thông báo.

---

#### Tóm tắt thông tin cần lưu lại

```
BACKEND_API_URL = https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com
S3_UPLOADS_BUCKET = ai-assistant-uploads
```
