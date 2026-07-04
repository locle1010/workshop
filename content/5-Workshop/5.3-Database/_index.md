---
title: "Setup Cognito & DynamoDB"
date: 2026-07-04
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

#### Overview

In this step, you will create:
+ **Amazon Cognito User Pool** — handles registration, login, and JWT token verification
+ **Amazon DynamoDB Table** — stores all application data using Single-Table Design

---

### Amazon Cognito

#### Create User Pool

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

 **Save the `UserPoolId`** (format: `ap-southeast-1_XXXXXXXXX`)

![User Pool created successfully](/images/5-Workshop/5.3-Database/5.3.1.png)

#### Create App Client

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

 **Save the `ClientId`**

![App Client created successfully](/images/5-Workshop/5.3-Database/5.3.2.png)

#### Create Cognito Domain (Hosted UI)

```bash
aws cognito-idp create-user-pool-domain \
  --domain "ai-assistant-auth" \
  --user-pool-id <UserPoolId>
```

![Create Cognito Domain](/images/5-Workshop/5.3-Database/5.3.3.png)

After creation, the Hosted UI will be available at:
`https://ai-assistant-auth.auth.ap-southeast-1.amazoncognito.com`

**Verify:**
```bash
aws cognito-idp describe-user-pool \
  --user-pool-id <UserPoolId> \
  --query "UserPool.{Name:Name,Status:Status}" \
  --output table
```

![Verify User Pool](/images/5-Workshop/5.3-Database/5.3.4.png)

**[AWS Console]:**
> + Go to [Cognito Console](https://ap-southeast-1.console.aws.amazon.com/cognito/v2/home)

![Cognito Console](/images/5-Workshop/5.3-Database/5.3.5.png)

> + **User pools** → **Create user pool**

![Create User Pool via Console](/images/5-Workshop/5.3-Database/5.3.6.png)

> + **Step 1:** Application type → select **Single-page application (SPA)**
> + **Step 2:** Name Application → `AIAssistantUsers`
> + **Step 3:** Sign-in options → select **Email**

![Configure User Pool](/images/5-Workshop/5.3-Database/5.3.7.png)

> + **Step 4:** Self-registration → keep defaults
> + **Step 5:** Review → **Create user pool**

![Review configuration](/images/5-Workshop/5.3-Database/5.3.8.png)

![User Pool created successfully](/images/5-Workshop/5.3-Database/5.3.9.png)

---

### Amazon DynamoDB

The AI Assistant project uses **Single-Table Design** — a single `AIAssistant` table stores all entities (User, Assistant Profile, ...) with keys `PK` (Partition Key) and `SK` (Sort Key).

Example data structure:
| PK | SK | Description |
|----|----|----|
| `USER#123` | `PROFILE` | User information |
| `USER#123` | `PET_PROFILE` | App configuration |

#### Create DynamoDB Table

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

> **PAY_PER_REQUEST** (on-demand): Suitable for workshops and small production — no need to provision throughput, only pay per request.

![Create DynamoDB table](/images/5-Workshop/5.3-Database/5.3.10.png)

#### Verify Table is Ready

```bash
aws dynamodb describe-table \
  --table-name AIAssistant \
  --region ap-southeast-1 \
  --query "Table.TableStatus"
```

Wait until the output returns `"ACTIVE"`.

![DynamoDB table in ACTIVE state](/images/5-Workshop/5.3-Database/5.3.11.png)

**[AWS Console]:**
> + Go to [DynamoDB Console](https://ap-southeast-1.console.aws.amazon.com/dynamodbv2/home)
> + **Tables** → **Create table**

![DynamoDB Console](/images/5-Workshop/5.3-Database/5.3.12.png)

![Create table via Console](/images/5-Workshop/5.3-Database/5.3.13.png)

> + **Table name:** `AIAssistant`
> + **Partition key:** `PK` (String)
> + **Sort key:** `SK` (String)

![Configure table keys](/images/5-Workshop/5.3-Database/5.3.14.png)

> + Click **Create table**

![Table created successfully](/images/5-Workshop/5.3-Database/5.3.15.png)

![DynamoDB table ready](/images/5-Workshop/5.3-Database/5.3.16.png)

---

#### Summary — Information to Save

After this step, record the following information for use in subsequent steps:

```
COGNITO_USER_POOL_ID   = ap-southeast-1_XXXXXXXXX
COGNITO_CLIENT_ID      = XXXXXXXXXXXXXXXXXXXXX
COGNITO_DOMAIN         = ai-assistant-auth.auth.ap-southeast-1.amazoncognito.com
DYNAMODB_TABLE         = AIAssistant
```
