---
title: "Environment Setup"
date: 2026-07-04
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

#### Overview

Before beginning the deployment, you need to install the necessary tools and configure your AWS access permissions.

---

#### Software Requirements

You must install the following 3 tools in order:

| # | Software | Version |
|---|----------|---------|
| 1 | **Git** | 2.x or later |
| 2 | **Node.js** | 2x.x |
| 3 | **AWS CLI** | 2.x |

---

#### Install Git

Download the installer from: [https://git-scm.com/downloads](https://git-scm.com/downloads)

+ **Windows:** Download the `.exe` file → run the installer → keep all defaults → click Next until Finish
+ **macOS:** Run `brew install git` (if using Homebrew) or download the `.dmg` from the link above
+ **Linux (Ubuntu/Debian):** Run `sudo apt-get install git -y`

Verify:
```bash
git --version
# Expected output: git version 2.x.x
```

---

#### Install Node.js 2x.x

Download the installer from: [https://nodejs.org/en/download](https://nodejs.org/en/download)

+ **Windows:** Download the `.msi` file → run the installer → click Next until Finish (make sure "Add to PATH" is checked)
+ **macOS:** Download the `.pkg` file → run the installer
+ **Linux:**
```bash
curl -fsSL https://deb.nodesource.com/setup_2x.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Verify:
```bash
node --version
# Expected output: v2x.x.x
npm --version
# Expected output: 1x.x.x
```

---

#### Install AWS CLI v2

**Windows:**
1. Download the MSI installer from: [https://awscli.amazonaws.com/AWSCLIV2.msi](https://awscli.amazonaws.com/AWSCLIV2.msi)
2. Run the downloaded `.msi` file
3. Click **Next** → **Next** → **Install** → **Finish**
4. Restart your **Command Prompt** or **PowerShell** (needed to refresh PATH environment variables)

**macOS:**
```bash
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
```

Or using Homebrew:
```bash
brew install awscli
```

**Linux (Ubuntu/Debian):**
```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

**Verify installation:**
```bash
aws --version
# Expected output: aws-cli/2.x.x Python/3.x.x ...
```

> Official documentation: [https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

---

#### Verify All Tools

Once all three are installed, run a comprehensive check:
```bash
git --version
node --version
npm --version
aws --version
```

![Verify tool versions](/images/5-Workshop/5.2-Prerequisite/5.2.1.png)

---

#### Create IAM User and Configure AWS CLI

> **Important:** Never use Root Account credentials for AWS CLI. You must create a dedicated IAM User in the AWS Console first, then use that user's access keys to log in via CLI.

> **Prerequisite:** You need an AWS account. If you don't have one, register for free at [https://aws.amazon.com/free](https://aws.amazon.com/free)

---

##### Step 1: Log in to AWS Console with Root Account

Go to [https://console.aws.amazon.com](https://console.aws.amazon.com) and log in using your Root Account email and password.

> This should be the only time you use the Root Account. Once the IAM User is created, log out of Root immediately.

---

##### Step 2: Create IAM User for Deployment

**Step 2.1:** In the Console, search for and open the **IAM** service.

> Type "IAM" in the top search bar and click the first result.

> Or go directly to: [https://console.aws.amazon.com/iam/home#/users](https://console.aws.amazon.com/iam/home#/users)

![Search IAM in Console](/images/5-Workshop/5.2-Prerequisite/5.2.2.png)

![IAM Console Home](/images/5-Workshop/5.2-Prerequisite/5.2.3.png)

**Step 2.2:** In the left navigation menu, click **Users** → click **Create user**.

![Users List](/images/5-Workshop/5.2-Prerequisite/5.2.4.png)

![Create User Button](/images/5-Workshop/5.2-Prerequisite/5.2.5.png)

**Step 2.3:** Enter user details:
- **User name:** `ai-assistant-deployer`
- Click **Next**

![Enter user name](/images/5-Workshop/5.2-Prerequisite/5.2.6.png)

**Step 2.4:** Set permissions — select **Attach policies directly**.

In the search box, type `AdministratorAccess`, check the policy, and click **Next**.

![Select AdministratorAccess policy](/images/5-Workshop/5.2-Prerequisite/5.2.7.png)

**Step 2.5:** Review information → click **Create user**.

![Review and create user](/images/5-Workshop/5.2-Prerequisite/5.2.8.png)

---

##### Step 3: Create Access Keys for the IAM User

**Step 3.1:** Once created, click on the username `ai-assistant-deployer` to open its details page.

![IAM User Details](/images/5-Workshop/5.2-Prerequisite/5.2.9.png)

**Step 3.2:** Select the **Security credentials** tab.

![Security Credentials Tab](/images/5-Workshop/5.2-Prerequisite/5.2.10.png)

**Step 3.3:** Scroll down to the **Access keys** section → click **Create access key**.

![Access Keys Section](/images/5-Workshop/5.2-Prerequisite/5.2.11.png)

**Step 3.4:** Select the **Command Line Interface (CLI)** use case → check the confirmation box below → click **Next** → click **Create access key**.

![Select CLI use case](/images/5-Workshop/5.2-Prerequisite/5.2.12.png)

![Confirm and create access key](/images/5-Workshop/5.2-Prerequisite/5.2.13.png)

**Step 3.5:** The credentials will be displayed — **save them immediately:**
- `Access Key ID` (e.g., `AKIAIOSFODNN7EXAMPLE`)
- `Secret Access Key` (e.g., `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`)

Or click **Download .csv file** to save both values.

> The Secret Access Key **is only displayed once**. If you close the window without saving it, you must delete the key and create a new one.

![Credentials created successfully](/images/5-Workshop/5.2-Prerequisite/5.2.14.png)

---

##### Step 4: Configure AWS CLI with the IAM User

Open your Terminal (PowerShell on Windows, Terminal on macOS/Linux) and run:

```bash
aws configure
```

Enter the values line-by-line as prompted:

```
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: ap-southeast-1
Default output format [None]: json
```

> These settings are stored in `~/.aws/credentials` and `~/.aws/config` on your local machine.

![Configure AWS CLI](/images/5-Workshop/5.2-Prerequisite/5.2.15.png)

---

##### Step 5: Verify Successful Authentication

```bash
aws sts get-caller-identity
```

Expected output — the `Arn` field must contain your IAM username, **not root**:

```json
{
    "UserId": "AIDAXXXXXXXXXXXXXXXXX",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/ai-assistant-deployer"
}
```

If you see `user/ai-assistant-deployer` in the Arn, your configuration is correct and ready for deployment.

![Verify successful login](/images/5-Workshop/5.2-Prerequisite/5.2.16.png)

---

#### Clone Repository

```bash
git clone https://github.com/LeQu0cAnh/PetWebAWSProject.git
cd PetWebAWSProject
```

![Clone repository](/images/5-Workshop/5.2-Prerequisite/5.2.17.png)

---

#### Install Serverless Framework

Serverless Framework enables deploying Lambda and API Gateway with a single command.

```bash
npm install -g serverless@3
serverless --version
```

![Serverless Framework installed successfully](/images/5-Workshop/5.2-Prerequisite/5.2.18.png)

> On macOS/Linux if you encounter a permissions error: `sudo npm install -g serverless@3`

---

#### Security Baseline (Amazon GuardDuty & AWS Config)

To ensure your AWS account is secure and continuously monitored for unusual activities, enable **Amazon GuardDuty** and **AWS Config**.

##### 1. Enable Amazon GuardDuty (Threat Detection)

Amazon GuardDuty continuously monitors for suspicious behavior (e.g., unusual logins, network attacks, unauthorized crypto mining) based on account logs.

**Via AWS CLI:**
```bash
aws guardduty create-detector --enable --region ap-southeast-1
```

**Via AWS Console:**
1. Search for and open the **GuardDuty** service in the AWS Web Console.
2. Click **Get started** -> select **Enable GuardDuty**.

![Enable GuardDuty](/images/5-Workshop/5.2-Prerequisite/5.2.19.png)

##### 2. Setup AWS Config (Resource Configuration Tracking)

AWS Config tracks the history of configuration changes for your AWS resources and compares them against security policies.

**Via AWS Console (Recommended):**
1. Search for the **Config** service -> click **Get started**.
2. Under **Settings**:
   - **Resource types to record**: Select **Record all resources supported in this region**.
   - **Amazon S3 bucket**: Select **Create a bucket** (enter a unique name to store configuration logs).
   - **AWS Config role**: Select **Create AWS Config service-linked role**.
3. Click **Next** -> Click **Next** -> Click **Confirm** to complete.

![Setup AWS Config](/images/5-Workshop/5.2-Prerequisite/5.2.20.png)

---

After completing this step, you are ready to start setting up the AWS services.
