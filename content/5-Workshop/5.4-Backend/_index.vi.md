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

#### Tóm tắt thông tin cần lưu lại

```
BACKEND_API_URL = https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com
S3_UPLOADS_BUCKET = ai-assistant-uploads
```
