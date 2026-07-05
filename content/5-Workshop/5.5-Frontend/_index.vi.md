---
title: "Deploy Frontend"
date: 2026-07-04
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

#### Tổng quan

Trong bước này, bạn sẽ build React app và deploy lên **Amazon S3** kết hợp **Amazon CloudFront** (CDN) để phân phối nhanh chóng và bảo mật đến người dùng toàn cầu.

---

### Cấu hình Frontend

Di chuyển vào thư mục frontend:
```bash
cd ../frontend
```

Tạo file `frontend/.env.production` với thông tin từ các bước trước:

```env
# --- API Backend URL (từ Bước 5.4) ---
VITE_API_URL=https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com

# --- Amazon Cognito (từ Bước 5.3) ---
VITE_COGNITO_REGION=ap-southeast-1
VITE_COGNITO_USER_POOL_ID=ap-southeast-1_XXXXXXXXX
VITE_COGNITO_CLIENT_ID=XXXXXXXXXXXXXXXXXXXXX
VITE_COGNITO_DOMAIN=ai-assistant-auth.auth.ap-southeast-1.amazoncognito.com
```

![Cấu hình file .env.production cho Frontend](/images/5-Workshop/5.5-Frontend/5.5.1.png)

---

### Build Frontend

```bash
npm install
npm run build
```

Sau khi build, thư mục `frontend/dist/` sẽ được tạo ra chứa toàn bộ file HTML, JS, CSS đã được tối ưu hóa.

![Build Frontend thành công](/images/5-Workshop/5.5-Frontend/5.5.2.png)

---

### Tạo S3 Bucket cho Frontend

```bash
aws s3api create-bucket \
  --bucket ai-assistant-frontend-prod \
  --region ap-southeast-1 \
  --create-bucket-configuration LocationConstraint=ap-southeast-1
```

> **Lưu ý:** Bucket này sẽ **không** mở public trực tiếp — chỉ CloudFront mới được phép đọc file.

![Tạo S3 Bucket cho Frontend](/images/5-Workshop/5.5-Frontend/5.5.3.png)

---

### Tạo CloudFront Distribution

#### Bước 1: Tạo Origin Access Control (OAC)

OAC đảm bảo chỉ CloudFront mới có thể đọc file từ S3 bucket, không ai truy cập trực tiếp được.

Tạo file `oac-config.json`:

```json
{
  "Name": "ai-assistant-oac",
  "Description": "OAC for AI Assistant Frontend S3",
  "SigningProtocol": "sigv4",
  "SigningBehavior": "always",
  "OriginAccessControlOriginType": "s3"
}
```

![Tạo file oac-config.json](/images/5-Workshop/5.5-Frontend/5.5.4.png)

```bash
aws cloudfront create-origin-access-control \
  --origin-access-control-config file://oac-config.json \
  --query "OriginAccessControl.Id" \
  --output text
```

 Lưu lại `OAC_ID`.

![Tạo OAC thành công](/images/5-Workshop/5.5-Frontend/5.5.5.png)

#### Bước 2: Tạo Distribution

> **Khuyến nghị:** Bước này nên làm qua **AWS Console** do cấu hình JSON khá dài và phức tạp.

**[Khuyến nghị - AWS Console]:**

Truy cập [CloudFront Console](https://console.aws.amazon.com/cloudfront/v3/home) → **Create distribution**

![CloudFront Console](/images/5-Workshop/5.5-Frontend/5.5.6.png)

Cấu hình như sau:

+ **Choose a plan:** Chọn **Free**

![Chọn gói Free](/images/5-Workshop/5.5-Frontend/5.5.7.png)

+ **Distribution name:** Nhập `ai-assistant-frontend`

![Nhập tên Distribution](/images/5-Workshop/5.5-Frontend/5.5.8.png)

+ **Specify origin:** Chọn **Amazon S3**
+ **Origin:** chọn **Browse S3** → chọn bucket `ai-assistant-frontend-prod`

![Cấu hình Origin S3](/images/5-Workshop/5.5-Frontend/5.5.9.png)

![Chọn bucket Frontend](/images/5-Workshop/5.5-Frontend/5.5.10.png)

![Xác nhận Origin đã được cấu hình](/images/5-Workshop/5.5-Frontend/5.5.11.png)

+ **Enable security:** chọn **Use monitor mode**

![Cấu hình bảo mật](/images/5-Workshop/5.5-Frontend/5.5.12.png)

+ Nhấn **Create distribution**

![Tạo Distribution](/images/5-Workshop/5.5-Frontend/5.5.13.png)

 **Lưu lại:**
- `Distribution ID` (dạng: `EXXXXXXXXXXXX`)
- `Distribution Domain Name` (dạng: `dxxxx.cloudfront.net`)

>  CloudFront cần **10–20 phút** để deploy toàn cầu sau khi tạo.

![Distribution đang được tạo](/images/5-Workshop/5.5-Frontend/5.5.14.png)

---

### Cấu hình Error Pages cho React Router (SPA)

React dùng Client-side routing. Khi người dùng tải lại trang con (ví dụ: `/login`), S3 trả về lỗi `403` vì file không tồn tại. Cần cấu hình CloudFront redirect về `index.html`.

**Qua Console:** CloudFront → Distribution → tab **Error pages** → **Create custom error response**

| HTTP error code | Customize? | Response page path | HTTP Response code |
|-----------------|------------|-------------------|-------------------|
| `403: Forbidden` | Yes | `/index.html` | `200: OK` |
| `404: Not Found` | Yes | `/index.html` | `200: OK` |

![Tab Error pages](/images/5-Workshop/5.5-Frontend/5.5.15.png)

![Tạo custom error response cho 403](/images/5-Workshop/5.5-Frontend/5.5.16.png)

![Tạo custom error response cho 404](/images/5-Workshop/5.5-Frontend/5.5.17.png)

![Cả 2 error pages đã được cấu hình](/images/5-Workshop/5.5-Frontend/5.5.18.png)

---

### Upload Frontend lên S3

```bash
cd ../frontend
aws s3 sync dist/ s3://ai-assistant-frontend-prod --delete
```

Output sẽ hiển thị danh sách các file được upload:
```
upload: dist/index.html to s3://ai-assistant-frontend-prod/index.html
upload: dist/assets/index-xxx.js to s3://ai-assistant-frontend-prod/assets/index-xxx.js
...
```

![Upload Frontend lên S3](/images/5-Workshop/5.5-Frontend/5.5.19.png)

---

### Kiểm tra Frontend

Truy cập CloudFront URL: `https://dxxxx.cloudfront.net`

Trang web AI Assistant phải hiển thị đầy đủ và hoạt động bình thường.

![Trang web AI Assistant chạy thành công](/images/5-Workshop/5.5-Frontend/5.5.20.png)

---

### Cấu hình Tự động Đóng gói & Lưu trữ dữ liệu tải lên (S3 Lifecycle Rules - Data Archive)

Để tối ưu chi phí lưu trữ trên S3, các tệp tin hình ảnh tải lên bởi người dùng sau 30 ngày sẽ tự động được chuyển đổi sang kho lưu trữ **S3 Glacier Flexible Retrieval (Data Archive)** với mức phí rẻ hơn nhiều lần.

Chúng ta sẽ thiết lập quy trình này cho bucket lưu trữ hình ảnh `ai-assistant-uploads` đã tạo ở Bước 5.4.

**Thao tác qua AWS CLI:**

1. Tạo file cấu hình vòng đời `lifecycle.json` ở thư mục dự án:
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
![Tạo file cấu hình vòng đời lifecycle.json](/images/5-Workshop/5.5-Frontend/5.5.20.1.png)
2. Áp dụng cấu hình lên bucket `ai-assistant-uploads`:
```bash
aws s3api put-bucket-lifecycle-configuration \
  --bucket ai-assistant-uploads \
  --lifecycle-configuration file://lifecycle.json \
  --region ap-southeast-1
```
![Áp dụng cấu hình Lifecycle lên bucket](/images/5-Workshop/5.5-Frontend/5.5.20.2.png)

**Thao tác qua AWS Console:**
1. Truy cập dịch vụ **S3** -> Chọn bucket **`ai-assistant-uploads`**.
![Truy cập bucket trên S3 Console](/images/5-Workshop/5.5-Frontend/5.5.20.3.png)
2. Chọn tab **Management** -> Tại mục **Lifecycle rules** chọn **Create lifecycle rule**.
![Tạo Lifecycle rule](/images/5-Workshop/5.5-Frontend/5.5.20.4.png)
3. Cấu hình:
   - **Lifecycle rule name**: `ArchiveOldUploads`.
   - **Choose a rule scope**: Chọn **Apply to all objects in the bucket** (Áp dụng toàn bộ tệp).
   - Tích chọn **Transition current versions of objects between storage classes**.
   ![Cấu hình Storage Class Transition](/images/5-Workshop/5.5-Frontend/5.5.20.5.png)
   - Mục **Transition current versions of objects**:
     - **Storage class transitions**: Chọn **Glacier Flexible Retrieval**.
     - **Days after object creation**: Nhập `30` ngày.
4. Nhấn **Create rule** để hoàn tất.
![Lifecycle rule được tạo thành công](/images/5-Workshop/5.5-Frontend/5.5.20.6.png)
---

### Cập nhật CORS Backend

Bây giờ đã có CloudFront URL, cập nhật lại CORS cho Backend.

Chỉnh sửa `backend/.env.production`:
```env
FRONTEND_URL=https://dxxxx.cloudfront.net
```

![Cập nhật FRONTEND_URL trong .env.production](/images/5-Workshop/5.5-Frontend/5.5.21.png)

Re-deploy Backend:
```bash
cd ../backend
npx serverless deploy --stage prod
```

![Re-deploy Backend với CORS mới](/images/5-Workshop/5.5-Frontend/5.5.22.png)

---

#### Tóm tắt thông tin cần lưu lại

```
CLOUDFRONT_DISTRIBUTION_ID = EXXXXXXXXXXXX
CLOUDFRONT_URL             = https://dxxxx.cloudfront.net
```
