---
title: "Setup Cognito & DynamoDB"
date: 2026-07-04
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

#### Overview

In this step, you will create:
+ **Amazon Cognito User Pool** — handles registration, login, and JWT token authentication
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

 **Save the `UserPoolId`** (e.g., `ap-southeast-1_XXXXXXXXX`)

![UserPool created successfully](/images/5-Workshop/5.3-Database/5.3.1.png)

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

![Cognito Domain created](/images/5-Workshop/5.3-Database/5.3.3.png)

Once created, the Hosted UI address will be:
`https://ai-assistant-auth.auth.ap-southeast-1.amazoncognito.com`

**Verify details:**
```bash
aws cognito-idp describe-user-pool \
  --user-pool-id <UserPoolId> \
  --query "UserPool.{Name:Name,Status:Status}" \
  --output table
```

![Verify User Pool](/images/5-Workshop/5.3-Database/5.3.4.png)

**[AWS Console]:**
> + Go to the [Cognito Console](https://ap-southeast-1.console.aws.amazon.com/cognito/v2/home)

![Cognito Console Home](/images/5-Workshop/5.3-Database/5.3.5.png)

> + **User pools** → **Create user pool**

![Create User Pool via Console](/images/5-Workshop/5.3-Database/5.3.6.png)

> + **Step 1:** Application type → select **Single-page application (SPA)**
> + **Step 2:** Name Application → `AIAssistantUsers`
> + **Step 3:** Sign-in options → select **Email**

![Configure User Pool](/images/5-Workshop/5.3-Database/5.3.7.png)

> + **Step 4:** Self-registration → keep defaults
> + **Step 5:** Review → **Create user pool**

![Review configuration](/images/5-Workshop/5.3-Database/5.3.8.png)

![User Pool created successfully via Console](/images/5-Workshop/5.3-Database/5.3.9.png)

---

### Amazon DynamoDB

The AI Assistant project uses a **Single-Table Design** — a single `AIAssistant` table stores all entities (User, Assistant Profile, etc.) using a Partition Key (`PK`) and a Sort Key (`SK`).

Example data structure:
| PK | SK | Description |
|----|----|-------------|
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

> **PAY_PER_REQUEST** (on-demand): Best suited for workshops and small production environments — no need to configure provisioned throughput, pay only for what you request.

![Create DynamoDB Table](/images/5-Workshop/5.3-Database/5.3.10.png)

#### Verify Table is Ready

```bash
aws dynamodb describe-table \
  --table-name AIAssistant \
  --region ap-southeast-1 \
  --query "Table.TableStatus"
```

Wait until the command returns `"ACTIVE"`.

![DynamoDB table status ACTIVE](/images/5-Workshop/5.3-Database/5.3.11.png)

**[AWS Console]:**
> + Go to [DynamoDB Console](https://ap-southeast-1.console.aws.amazon.com/dynamodbv2/home)
> + **Tables** → **Create table**

![DynamoDB Console Home](/images/5-Workshop/5.3-Database/5.3.12.png)

![Create Table via Console](/images/5-Workshop/5.3-Database/5.3.13.png)

> + **Table name:** `AIAssistant`
> + **Partition key:** `PK` (String)
> + **Sort key:** `SK` (String)

![Configure Keys](/images/5-Workshop/5.3-Database/5.3.14.png)

> + Click **Create table**

![Table created successfully](/images/5-Workshop/5.3-Database/5.3.15.png)

![DynamoDB table ready](/images/5-Workshop/5.3-Database/5.3.16.png)

---

### AWS Backup for DynamoDB

To protect your database against accidental deletion or system failure, configure **AWS Backup** to automatically back up the `AIAssistant` DynamoDB table daily.

#### 1. Create a Backup Vault (Secure Storage)

A Backup Vault is a container that stores your backups securely.

**Via AWS CLI:**
```bash
aws backup create-backup-vault \
  --backup-vault-name "AIAssistantBackupVault" \
  --region ap-southeast-1
```

#### 2. Create a Backup Plan

A Backup Plan defines when backups should be run (e.g., daily) and how long they should be retained.

**Via AWS Console (Recommended):**
1. Search for and open the **AWS Backup** service in the Console.
2. Select **Backup plans** -> **Create Backup plan**.
3. Choose **Build a new plan**:
   - **Backup plan name**: `ai-assistant-backup-plan`.
4. Configure the **Backup rule**:
   - **Rule name**: `DailyBackup`.
   - **Backup vault**: Select the `AIAssistantBackupVault` created above.
   - **Backup frequency**: Select **Daily**.
   - **Retention period**: Select **Days** and enter `7` days (to automatically delete backups older than 7 days to save costs).
5. Click **Create plan**.

![Create Backup Plan](/images/5-Workshop/5.3-Database/5.3.17.png)

#### 3. Assign the DynamoDB Table to the Backup Plan

Once the plan is created, associate the DynamoDB table as the target resource for backups.

**Via AWS Console:**
1. In the details page of your new Backup Plan, scroll down to **Resource assignments** -> click **Assign resources**.
2. Configure:
   - **Resource assignment name**: `assign-dynamodb-table`.
   - **IAM role**: Select **Default role**.
   - **Assign resources**: Select **Include specific resource types**.
   - **Select specific resource types**: Select **DynamoDB**.
   - **Table name**: Select the `AIAssistant` table.
3. Click **Assign resources** to complete.

![Assign DynamoDB to Backup Plan](/images/5-Workshop/5.3-Database/5.3.18.png)

---

#### Summary — Information to Save

After this step, record the following information for use in subsequent steps:

```
COGNITO_USER_POOL_ID   = ap-southeast-1_XXXXXXXXX
COGNITO_CLIENT_ID      = XXXXXXXXXXXXXXXXXXXXX
COGNITO_DOMAIN         = ai-assistant-auth.auth.ap-southeast-1.amazoncognito.com
DYNAMODB_TABLE         = AIAssistant
```
