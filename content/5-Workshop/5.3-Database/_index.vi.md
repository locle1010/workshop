---
title: "Thiết lập Cognito & DynamoDB"
date: 2026-07-04
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

#### Tổng quan

Trong bước này, bạn sẽ tạo:
+ **Amazon Cognito User Pool** — xử lý đăng ký, đăng nhập và xác thực JWT token
+ **Amazon DynamoDB Table** — lưu trữ toàn bộ dữ liệu ứng dụng theo Single-Table Design

---

### Amazon Cognito

#### Tạo User Pool

```bash
aws cognito-idp create-user-pool \
  --pool-name "AIAssistantUsers" \
  --region ap-southeast-1 \
  --auto-verified-attributes email \
  --username-attributes email \
  --policies "PasswordPolicy={MinimumLength=8,RequireUppercase=true,RequireLowercase=true,RequireNumbers=true,RequireSymbols=false}" \
  --query "UserPool.Id" \
  --output text
```

 **Lưu lại `UserPoolId`** (dạng: `ap-southeast-1_XXXXXXXXX`)

![Tạo User Pool thành công](/images/5-Workshop/5.3-Database/5.3.1.png)

#### Tạo App Client

```bash
aws cognito-idp create-user-pool-client \
  --user-pool-id <UserPoolId> \
  --client-name "ai-assistant-web-client" \
  --no-generate-secret \
  --allowed-o-auth-flows "implicit" "code" \
  --allowed-o-auth-scopes "email" "openid" "profile" \
  --callback-urls "http://localhost:5173/callback" "https://<your-domain>/callback" \
  --logout-urls "http://localhost:5173" "https://<your-domain>" \
  --supported-identity-providers "COGNITO" \
  --allowed-o-auth-flows-user-pool-client \
  --query "UserPoolClient.ClientId" \
  --output text
```

 **Lưu lại `ClientId`**

![Tạo App Client thành công](/images/5-Workshop/5.3-Database/5.3.2.png)

#### Tạo Cognito Domain (Hosted UI)

```bash
aws cognito-idp create-user-pool-domain \
  --domain "ai-assistant-auth" \
  --user-pool-id <UserPoolId>
```

![Tạo Cognito Domain](/images/5-Workshop/5.3-Database/5.3.3.png)

Sau khi tạo xong, Hosted UI sẽ có địa chỉ:
`https://ai-assistant-auth.auth.ap-southeast-1.amazoncognito.com`

**Kiểm tra xác nhận:**
```bash
aws cognito-idp describe-user-pool \
  --user-pool-id <UserPoolId> \
  --query "UserPool.{Name:Name,Status:Status}" \
  --output table
```

![Kiểm tra User Pool](/images/5-Workshop/5.3-Database/5.3.4.png)

**[AWS Console]:**
> + Truy cập [Cognito Console](https://ap-southeast-1.console.aws.amazon.com/cognito/v2/home)

![Cognito Console](/images/5-Workshop/5.3-Database/5.3.5.png)

> + **User pools** → **Create user pool**

![Tạo User Pool qua Console](/images/5-Workshop/5.3-Database/5.3.6.png)

> + **Step 1:** Application type → chọn **Single-page application (SPA)**
> + **Step 2:** Name Application → `AIAssistantUsers`
> + **Step 3:** Sign-in options → chọn **Email**

![Cấu hình User Pool](/images/5-Workshop/5.3-Database/5.3.7.png)

> + **Step 4:** Self-registration → giữ mặc định
> + **Step 5:** Review → **Create user pool**

![Review cấu hình](/images/5-Workshop/5.3-Database/5.3.8.png)

![User Pool được tạo thành công](/images/5-Workshop/5.3-Database/5.3.9.png)

---

### Amazon DynamoDB

Dự án AI Assistant sử dụng **Single-Table Design** — một bảng `AIAssistant` duy nhất lưu tất cả entity (User, Assistant Profile, ...) với khóa `PK` (Partition Key) và `SK` (Sort Key).

Ví dụ cấu trúc dữ liệu:
| PK | SK | Mô tả |
|----|----|----|
| `USER#123` | `PROFILE` | Thông tin người dùng |
| `USER#123` | `PET_PROFILE` | Cấu hình app |

#### Tạo bảng DynamoDB

```bash
aws dynamodb create-table \
  --table-name AIAssistant \
  --attribute-definitions \
    AttributeName=PK,AttributeType=S \
    AttributeName=SK,AttributeType=S \
  --key-schema \
    AttributeName=PK,KeyType=HASH \
    AttributeName=SK,KeyType=RANGE \
  --billing-mode PAY_PER_REQUEST \
  --region ap-southeast-1
```

> **PAY_PER_REQUEST** (on-demand): Phù hợp cho workshop và production nhỏ — không cần provision throughput, chỉ trả tiền theo lượng request.

![Tạo bảng DynamoDB](/images/5-Workshop/5.3-Database/5.3.10.png)

#### Kiểm tra bảng đã sẵn sàng

```bash
aws dynamodb describe-table \
  --table-name AIAssistant \
  --region ap-southeast-1 \
  --query "Table.TableStatus"
```

Chờ cho đến khi output trả về `"ACTIVE"`.

![Bảng DynamoDB ở trạng thái ACTIVE](/images/5-Workshop/5.3-Database/5.3.11.png)

**[AWS Console]:**
> + Truy cập [DynamoDB Console](https://ap-southeast-1.console.aws.amazon.com/dynamodbv2/home)
> + **Tables** → **Create table**

![DynamoDB Console](/images/5-Workshop/5.3-Database/5.3.12.png)

![Tạo bảng qua Console](/images/5-Workshop/5.3-Database/5.3.13.png)

> + **Table name:** `AIAssistant`
> + **Partition key:** `PK` (String)
> + **Sort key:** `SK` (String)

![Cấu hình khóa bảng](/images/5-Workshop/5.3-Database/5.3.14.png)

> + Nhấn **Create table**

![Tạo bảng thành công](/images/5-Workshop/5.3-Database/5.3.15.png)

![Bảng DynamoDB đã sẵn sàng](/images/5-Workshop/5.3-Database/5.3.16.png)

---

#### Tóm tắt thông tin cần lưu lại

Sau bước này, hãy ghi lại các thông tin sau để dùng cho các bước tiếp theo:

```
COGNITO_USER_POOL_ID   = ap-southeast-1_XXXXXXXXX
COGNITO_CLIENT_ID      = XXXXXXXXXXXXXXXXXXXXX
COGNITO_DOMAIN         = ai-assistant-auth.auth.ap-southeast-1.amazoncognito.com
DYNAMODB_TABLE         = AIAssistant
```
