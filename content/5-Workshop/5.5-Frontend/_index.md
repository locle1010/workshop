---
title: "Deploy Frontend"
date: 2026-07-04
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

#### Overview

In this step, you will build the React app and deploy it to **Amazon S3** combined with **Amazon CloudFront** (CDN) for fast and secure global delivery.

---

### Configure Frontend

Navigate to the frontend directory:
```bash
cd ../frontend
```

Create `frontend/.env.production` with information from previous steps:

```env
# --- API Backend URL (from Step 5.4) ---
VITE_API_URL=https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com

# --- Amazon Cognito (from Step 5.3) ---
VITE_COGNITO_REGION=ap-southeast-1
VITE_COGNITO_USER_POOL_ID=ap-southeast-1_XXXXXXXXX
VITE_COGNITO_CLIENT_ID=XXXXXXXXXXXXXXXXXXXXX
VITE_COGNITO_DOMAIN=ai-assistant-auth.auth.ap-southeast-1.amazoncognito.com
```

![Configure .env.production for Frontend](/images/5-Workshop/5.5-Frontend/5.5.1.png)

---

### Build Frontend

```bash
npm install
npm run build
```

After building, the `frontend/dist/` directory will be created containing all optimized HTML, JS, and CSS files.

![Frontend built successfully](/images/5-Workshop/5.5-Frontend/5.5.2.png)

---

### Create S3 Bucket for Frontend

```bash
aws s3api create-bucket \
  --bucket ai-assistant-frontend-prod \
  --region ap-southeast-1 \
  --create-bucket-configuration LocationConstraint=ap-southeast-1
```

> **Note:** This bucket will **not** be publicly accessible directly — only CloudFront is allowed to read files from it.

![Create S3 Bucket for Frontend](/images/5-Workshop/5.5-Frontend/5.5.3.png)

---

### Create CloudFront Distribution

#### Step 1: Create Origin Access Control (OAC)

OAC ensures only CloudFront can read files from the S3 bucket; direct access is blocked.

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

![Create oac-config.json file](/images/5-Workshop/5.5-Frontend/5.5.4.png)

```bash
aws cloudfront create-origin-access-control \
  --origin-access-control-config file://oac-config.json \
  --query "OriginAccessControl.Id" \
  --output text
```

 Save the `OAC_ID`.

![OAC created successfully](/images/5-Workshop/5.5-Frontend/5.5.5.png)

#### Step 2: Create Distribution

> **Recommendation:** This step is best done via **AWS Console** as the JSON configuration is quite lengthy and complex.

**[Recommended - AWS Console]:**

Go to [CloudFront Console](https://console.aws.amazon.com/cloudfront/v3/home) → **Create distribution**

![CloudFront Console](/images/5-Workshop/5.5-Frontend/5.5.6.png)

Configure as follows:

+ **Choose a plan:** Select **Free**

![Select Free plan](/images/5-Workshop/5.5-Frontend/5.5.7.png)

+ **Distribution name:** Enter `ai-assistant-frontend`

![Enter Distribution name](/images/5-Workshop/5.5-Frontend/5.5.8.png)

+ **Specify origin:** Select **Amazon S3**
+ **Origin:** click **Browse S3** → select bucket `ai-assistant-frontend-prod`

![Configure S3 Origin](/images/5-Workshop/5.5-Frontend/5.5.9.png)

![Select Frontend bucket](/images/5-Workshop/5.5-Frontend/5.5.10.png)

![Origin configured successfully](/images/5-Workshop/5.5-Frontend/5.5.11.png)

+ **Enable security:** select **Use monitor mode**

![Configure security settings](/images/5-Workshop/5.5-Frontend/5.5.12.png)

+ Click **Create distribution**

![Create Distribution](/images/5-Workshop/5.5-Frontend/5.5.13.png)

 **Save:**
- `Distribution ID` (format: `EXXXXXXXXXXXX`)
- `Distribution Domain Name` (format: `dxxxx.cloudfront.net`)

>  CloudFront takes **10–20 minutes** to deploy globally after creation.

![Distribution being created](/images/5-Workshop/5.5-Frontend/5.5.14.png)

---

### Configure Error Pages for React Router (SPA)

React uses Client-side routing. When users reload a sub-page (e.g., `/login`), S3 returns a `403` error because the file doesn't exist. You need to configure CloudFront to redirect to `index.html`.

**Via Console:** CloudFront → Distribution → **Error pages** tab → **Create custom error response**

| HTTP error code | Customize? | Response page path | HTTP Response code |
|-----------------|------------|-------------------|-------------------|
| `403: Forbidden` | Yes | `/index.html` | `200: OK` |
| `404: Not Found` | Yes | `/index.html` | `200: OK` |

![Error pages tab](/images/5-Workshop/5.5-Frontend/5.5.15.png)

![Create custom error response for 403](/images/5-Workshop/5.5-Frontend/5.5.16.png)

![Create custom error response for 404](/images/5-Workshop/5.5-Frontend/5.5.17.png)

![Both error pages configured](/images/5-Workshop/5.5-Frontend/5.5.18.png)

---

### Upload Frontend to S3

```bash
cd ../frontend
aws s3 sync dist/ s3://ai-assistant-frontend-prod --delete
```

Output will show the list of files being uploaded:
```
upload: dist/index.html to s3://ai-assistant-frontend-prod/index.html
upload: dist/assets/index-xxx.js to s3://ai-assistant-frontend-prod/assets/index-xxx.js
...
```

![Upload Frontend to S3](/images/5-Workshop/5.5-Frontend/5.5.19.png)

---

### Test Frontend

Visit the CloudFront URL: `https://dxxxx.cloudfront.net`

The AI Assistant website should display fully and work normally.

![AI Assistant website running successfully](/images/5-Workshop/5.5-Frontend/5.5.20.png)

---

### Update Backend CORS

Now that you have the CloudFront URL, update the CORS settings for the Backend.

Edit `backend/.env.production`:
```env
FRONTEND_URL=https://dxxxx.cloudfront.net
```

![Update FRONTEND_URL in .env.production](/images/5-Workshop/5.5-Frontend/5.5.21.png)

Re-deploy Backend:
```bash
cd ../backend
npx serverless deploy --stage prod
```

![Re-deploy Backend with new CORS](/images/5-Workshop/5.5-Frontend/5.5.22.png)

---

#### Summary — Information to Save

```
CLOUDFRONT_DISTRIBUTION_ID = EXXXXXXXXXXXX
CLOUDFRONT_URL             = https://dxxxx.cloudfront.net
```
