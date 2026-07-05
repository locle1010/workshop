---
title: "Deploy Frontend"
date: 2026-07-04
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

#### Overview

In this step, you will build the React app and deploy it to **Amazon S3** combined with **Amazon CloudFront** (CDN) for fast and secure content delivery to users globally.

---

### Configure Frontend

Navigate to the frontend directory:
```bash
cd ../frontend
```

Create a `frontend/.env.production` file using the information from the previous steps:

```env
# --- API Backend URL (from Step 5.4) ---
VITE_API_URL=https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com

# --- Amazon Cognito (from Step 5.3) ---
VITE_COGNITO_REGION=ap-southeast-1
VITE_COGNITO_USER_POOL_ID=ap-southeast-1_XXXXXXXXX
VITE_COGNITO_CLIENT_ID=XXXXXXXXXXXXXXXXXXXXX
VITE_COGNITO_DOMAIN=ai-assistant-auth.auth.ap-southeast-1.amazoncognito.com
```

![Configure frontend .env.production](/images/5-Workshop/5.5-Frontend/5.5.1.png)

---

### Build Frontend

```bash
npm install
npm run build
```

After building, the `frontend/dist/` directory will be created, containing the optimized HTML, JS, and CSS files.

![Frontend built successfully](/images/5-Workshop/5.5-Frontend/5.5.2.png)

---

### Create Frontend S3 Bucket

```bash
aws s3api create-bucket \
  --bucket ai-assistant-frontend-prod \
  --region ap-southeast-1 \
  --create-bucket-configuration LocationConstraint=ap-southeast-1
```

> **Note:** This bucket will **not** be configured for public access — only CloudFront will have permissions to read files from it.

![Create Frontend S3 Bucket](/images/5-Workshop/5.5-Frontend/5.5.3.png)

---

### Create CloudFront Distribution

#### Step 1: Create Origin Access Control (OAC)

OAC ensures only CloudFront can read files from your S3 bucket, preventing direct user access to the bucket.

Create `oac-config.json`:

```json
{
  "Name": "ai-assistant-oac",
  "Description": "OAC for AI Assistant Frontend S3",
  "SigningProtocol": "sigv4",
  "SigningBehavior": "always",
  "OriginAccessControlOriginType": "s3"
}
```

![Create oac-config.json](/images/5-Workshop/5.5-Frontend/5.5.4.png)

```bash
aws cloudfront create-origin-access-control \
  --origin-access-control-config file://oac-config.json \
  --query "OriginAccessControl.Id" \
  --output text
```

 Save the `OAC_ID`.

![OAC created successfully](/images/5-Workshop/5.5-Frontend/5.5.5.png)

#### Step 2: Create Distribution

> **Recommendation:** Using the **AWS Console** is highly recommended for this step due to the complex JSON configuration structure.

**[AWS Console]:**

Go to [CloudFront Console](https://console.aws.amazon.com/cloudfront/v3/home) → click **Create distribution**.

![CloudFront Console Home](/images/5-Workshop/5.5-Frontend/5.5.6.png)

Configure the following:

+ **Choose a plan:** Select **Free**

![Select Free Plan](/images/5-Workshop/5.5-Frontend/5.5.7.png)

+ **Distribution name:** Enter `ai-assistant-frontend`

![Enter distribution name](/images/5-Workshop/5.5-Frontend/5.5.8.png)

+ **Specify origin:** Select **Amazon S3**
+ **Origin:** Click **Browse S3** → select the `ai-assistant-frontend-prod` bucket

![Configure S3 Origin](/images/5-Workshop/5.5-Frontend/5.5.9.png)

![Select S3 Bucket](/images/5-Workshop/5.5-Frontend/5.5.10.png)

![S3 Origin applied](/images/5-Workshop/5.5-Frontend/5.5.11.png)

+ **Enable security:** Select **Use monitor mode**

![Configure Web Application Firewall](/images/5-Workshop/5.5-Frontend/5.5.12.png)

+ Click **Create distribution**

![Create Distribution](/images/5-Workshop/5.5-Frontend/5.5.13.png)

 **Record the following:**
- `Distribution ID` (e.g., `EXXXXXXXXXXXX`)
- `Distribution Domain Name` (e.g., `dxxxx.cloudfront.net`)

>  CloudFront takes **10–20 minutes** to deploy globally.

![Distribution status deploying](/images/5-Workshop/5.5-Frontend/5.5.14.png)

---

### Configure Error Pages for React Router (SPA)

React uses client-side routing. When a user refreshes a sub-route (e.g., `/login`), S3 returns a `403` error because the file does not exist. We need CloudFront to redirect these requests to `index.html`.

**Via AWS Console:** CloudFront → select your Distribution → open **Error pages** tab → click **Create custom error response**.

| HTTP error code | Customize? | Response page path | HTTP Response code |
|-----------------|------------|-------------------|-------------------|
| `403: Forbidden` | Yes | `/index.html` | `200: OK` |
| `404: Not Found` | Yes | `/index.html` | `200: OK` |

![Error Pages Tab](/images/5-Workshop/5.5-Frontend/5.5.15.png)

![Create custom error response for 403](/images/5-Workshop/5.5-Frontend/5.5.16.png)

![Create custom error response for 404](/images/5-Workshop/5.5-Frontend/5.5.17.png)

![Error responses configured successfully](/images/5-Workshop/5.5-Frontend/5.5.18.png)

---

### Upload Frontend to S3

```bash
cd ../frontend
aws s3 sync dist/ s3://ai-assistant-frontend-prod --delete
```

The output will display the uploaded files:
```
upload: dist/index.html to s3://ai-assistant-frontend-prod/index.html
upload: dist/assets/index-xxx.js to s3://ai-assistant-frontend-prod/assets/index-xxx.js
...
```

![Upload frontend to S3](/images/5-Workshop/5.5-Frontend/5.5.19.png)

---

### Verify Frontend

Access the CloudFront URL: `https://dxxxx.cloudfront.net`

The AI Assistant website should load and function normally.

![Verify deployed website](/images/5-Workshop/5.5-Frontend/5.5.20.png)

---

### Configure Auto-archiving of Uploads (S3 Lifecycle Rules - Data Archive)

To optimize storage costs on S3, objects uploaded by users can be moved to **S3 Glacier Flexible Retrieval (Data Archive)** automatically after 30 days.

We will configure this lifecycle policy on the `ai-assistant-uploads` bucket created in Step 5.4.

**Via AWS CLI:**

1. Create a lifecycle configuration file named `lifecycle.json` at the root of the project:
```json
{
  "Rules": [
    {
      "ID": "ArchiveOldUploadsAfter30Days",
      "Status": "Enabled",
      "Filter": {
        "Prefix": ""
      },
      "Transitions": [
        {
          "Days": 30,
          "StorageClass": "GLACIER"
        }
      ]
    }
  ]
}
```

2. Apply the configuration to the `ai-assistant-uploads` bucket:
```bash
aws s3api put-bucket-lifecycle-configuration \
  --bucket ai-assistant-uploads \
  --lifecycle-configuration file://lifecycle.json \
  --region ap-southeast-1
```

**Via AWS Console:**
1. Open the **S3** service -> click on the **`ai-assistant-uploads`** bucket.
2. Select the **Management** tab -> under **Lifecycle rules**, click **Create lifecycle rule**.
3. Configure:
   - **Lifecycle rule name**: `ArchiveOldUploads`.
   - **Rule scope**: Select **Apply to all objects in the bucket**.
   - Check **Transition current versions of objects between storage classes**.
   - Under **Transition current versions of objects**:
     - **Storage class transitions**: Select **Glacier Flexible Retrieval**.
     - **Days after object creation**: Enter `30` days.
4. Click **Create rule** to complete.

![Configure S3 Lifecycle Rule](/images/5-Workshop/5.5-Frontend/5.5.20.1.png)

---

### Update Backend CORS

Now that we have the CloudFront URL, update the CORS settings in the Backend environment configuration.

Edit `backend/.env.production`:
```env
FRONTEND_URL=https://dxxxx.cloudfront.net
```

![Update environment file](/images/5-Workshop/5.5-Frontend/5.5.21.png)

Re-deploy the Backend:
```bash
cd ../backend
npx serverless deploy --stage prod
```

![Re-deploy backend](/images/5-Workshop/5.5-Frontend/5.5.22.png)

---

#### Summary — Information to Save

```
CLOUDFRONT_DISTRIBUTION_ID = EXXXXXXXXXXXX
CLOUDFRONT_URL             = https://dxxxx.cloudfront.net
```
