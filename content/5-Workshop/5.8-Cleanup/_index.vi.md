---
title: "Dọn dẹp tài nguyên"
date: 2026-07-04
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

#### Tổng quan

> **Quan trọng:** Sau khi hoàn thành workshop, hãy **luôn xóa các tài nguyên AWS** để tránh phát sinh chi phí không mong muốn.

Thực hiện **theo đúng thứ tự** dưới đây để đảm bảo xóa sạch toàn bộ.

---

### Bước 1: Xóa Backend (Lambda + API Gateway + CloudFormation)

Serverless Framework quản lý toàn bộ backend qua CloudFormation. Chỉ cần một lệnh:

```bash
cd backend
npx serverless remove --stage prod
```

Lệnh này sẽ tự động xóa:
-  Lambda Function
-  API Gateway (HTTP API)
-  CloudFormation Stack
-  S3 deployment bucket tạm thời
-  CloudWatch Log Groups

![Xóa Backend qua Serverless Framework](/images/5-Workshop/5.8-Cleanup/5.8.1.png)

---

### Bước 2: Xóa Frontend S3 Bucket

```bash
# Xóa toàn bộ nội dung trong bucket trước
aws s3 rm s3://ai-assistant-frontend-prod --recursive

# Xóa bucket
aws s3api delete-bucket \
  --bucket ai-assistant-frontend-prod \
  --region ap-southeast-1
```

![Xóa nội dung Frontend S3 Bucket](/images/5-Workshop/5.8-Cleanup/5.8.2.png)

![Frontend S3 Bucket đã được xóa](/images/5-Workshop/5.8-Cleanup/5.8.3.png)

---

### Bước 3: Xóa Upload S3 Bucket

```bash
aws s3 rm s3://ai-assistant-uploads --recursive
aws s3api delete-bucket \
  --bucket ai-assistant-uploads \
  --region ap-southeast-1
```

![Xóa Upload S3 Bucket](/images/5-Workshop/5.8-Cleanup/5.8.4.png)

---

### Bước 4: Xóa CloudFront Distribution

CloudFront cần được **Disabled** trước khi có thể Delete. Quá trình này mất ~5-15 phút.

> **Khuyến nghị:** Làm qua Console để theo dõi trạng thái dễ hơn:
> CloudFront → Distributions → Chọn distribution → **Disable** → Chờ status = `Disabled` → **Delete**

![Chọn distribution cần xóa](/images/5-Workshop/5.8-Cleanup/5.8.5.png)

![Disable Distribution](/images/5-Workshop/5.8-Cleanup/5.8.6.png)

![Chờ status chuyển thành Disabled](/images/5-Workshop/5.8-Cleanup/5.8.7.png)

![Xóa Distribution thành công](/images/5-Workshop/5.8-Cleanup/5.8.8.png)

---

### Bước 5: Xóa DynamoDB Table

```bash
aws dynamodb delete-table \
  --table-name AIAssistant \
  --region ap-southeast-1
```

![Xóa DynamoDB Table](/images/5-Workshop/5.8-Cleanup/5.8.9.png)

Xác nhận đã xóa:
```bash
aws dynamodb list-tables \
  --region ap-southeast-1 \
  --query "TableNames"
```

![Xác nhận DynamoDB Table đã được xóa](/images/5-Workshop/5.8-Cleanup/5.8.10.png)

---

### Bước 6: Xóa Cognito User Pool

```bash
# Xóa App Client trước
aws cognito-idp delete-user-pool-client \
  --user-pool-id <UserPoolId> \
  --client-id <ClientId>

# Xóa custom domain
aws cognito-idp delete-user-pool-domain \
  --domain "ai-assistant-auth" \
  --user-pool-id <UserPoolId>

# Xóa User Pool
aws cognito-idp delete-user-pool \
  --user-pool-id <UserPoolId>
```

![Xóa Cognito User Pool](/images/5-Workshop/5.8-Cleanup/5.8.11.png)

---

### Kiểm tra dọn dẹp hoàn tất

```bash
# Kiểm tra Lambda
aws lambda list-functions --region ap-southeast-1 \
  --query "Functions[?starts_with(FunctionName,'AIAssistant')]"

# Kiểm tra S3
aws s3 ls | grep ai-assistant

# Kiểm tra DynamoDB
aws dynamodb list-tables --region ap-southeast-1 \
  --query "TableNames[?contains(@,'AIAssistant')]"

# Kiểm tra Cognito
aws cognito-idp list-user-pools \
  --max-results 20 \
  --region ap-southeast-1 \
  --query "UserPools[?contains(Name,'AIAssistant')]"
```

Tất cả các lệnh trên phải trả về kết quả rỗng `[]` là đã dọn dẹp hoàn tất.

![Xác nhận tất cả tài nguyên đã được xóa](/images/5-Workshop/5.8-Cleanup/5.8.12.png)
