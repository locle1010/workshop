---
title: "Environment Setup"
date: 2026-07-04
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

#### Overview

Before starting the deployment, you need to install the required tools and configure AWS access permissions.

---

#### Software Requirements

You need to install all 3 tools below in order:

| # | Software | Version |
|---|----------|---------|
| 1 | **Git** | 2.x or higher |
| 2 | **Node.js** | 2x.x |
| 3 | **AWS CLI** | 2.x |

---

#### Install Git

Download the installer at: [https://git-scm.com/downloads](https://git-scm.com/downloads)

+ **Windows:** Download `.exe` file → run installer → keep defaults → Next to Finish
+ **macOS:** Run `brew install git` (if Homebrew is installed) or download `.dmg` from the page above
+ **Linux (Ubuntu/Debian):** `sudo apt-get install git -y`

Verify:
```bash
git --version
# Expected output: git version 2.x.x
```

---

#### Install Node.js 2x.x

Download the installer at: [https://nodejs.org/en/download](https://nodejs.org/en/download)

+ **Windows:** Download `.msi` file → run installer → Next to Finish (check "Add to PATH")
+ **macOS:** Download `.pkg` file → run installer
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
1. Download the MSI installer at: [https://awscli.amazonaws.com/AWSCLIV2.msi](https://awscli.amazonaws.com/AWSCLIV2.msi)
2. Run the downloaded `.msi` file
3. Click **Next** → **Next** → **Install** → **Finish**
4. Reopen **Command Prompt** or **PowerShell** (required to pick up the new PATH)

**macOS:**
```bash
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
```

Or use Homebrew:
```bash
brew install awscli
```

**Linux (Ubuntu/Debian):**
```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

**Verify successful installation:**
```bash
aws --version
# Expected output: aws-cli/2.x.x Python/3.x.x ...
```

> Official documentation: [https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

---

#### Verify All Tools

After installing all 3, run the combined verification command:
```bash
git --version
node --version
npm --version
aws --version
```

![Verify tool versions](/images/5-Workshop/5.2-Prerequisite/5.2.1.png)

---

#### Create IAM User and Configure AWS CLI

> **Important:** The AWS CLI should never use Root Account credentials. You must create a separate IAM User in the AWS Console first, then use that IAM User's keys to authenticate the CLI.

> **Requirement:** You need an existing AWS account. If you don't have one, sign up for free at [https://aws.amazon.com/free](https://aws.amazon.com/free)

---

##### Step 1: Log in to AWS Console with Root Account

Go to [https://console.aws.amazon.com](https://console.aws.amazon.com) and sign in with your Root Account email and password.

> This is the only time you should use the Root Account. After creating the IAM User, log out of Root.

---

##### Step 2: Create an IAM User for Deployment

**Step 2.1:** In the Console, find and open the **IAM** service

> Type "IAM" in the search bar at the top and select the first result

> Or navigate directly to: [https://console.aws.amazon.com/iam/home#/users](https://console.aws.amazon.com/iam/home#/users)

![Search for IAM in Console](/images/5-Workshop/5.2-Prerequisite/5.2.2.png)

![IAM Console page](/images/5-Workshop/5.2-Prerequisite/5.2.3.png)

**Step 2.2:** In the left menu, select **Users** → click **Create user**

![Users list](/images/5-Workshop/5.2-Prerequisite/5.2.4.png)

![Create user button](/images/5-Workshop/5.2-Prerequisite/5.2.5.png)

**Step 2.3:** Fill in user information:
- **User name:** `ai-assistant-deployer`
- Click **Next**

![Enter user name](/images/5-Workshop/5.2-Prerequisite/5.2.6.png)

**Step 2.4:** Set permissions — select **Attach policies directly**

In the search box, type `AdministratorAccess`, check that policy, then click **Next**

![Select AdministratorAccess policy](/images/5-Workshop/5.2-Prerequisite/5.2.7.png)

**Step 2.5:** Review information → click **Create user**

![Review and create user](/images/5-Workshop/5.2-Prerequisite/5.2.8.png)

---

##### Step 3: Create an Access Key for the New IAM User

**Step 3.1:** After creation, click the username `ai-assistant-deployer` to go to the detail page

![IAM User details](/images/5-Workshop/5.2-Prerequisite/5.2.9.png)

**Step 3.2:** Select the **Security credentials** tab

![Security credentials tab](/images/5-Workshop/5.2-Prerequisite/5.2.10.png)

**Step 3.3:** Scroll down to **Access keys** → click **Create access key**

![Access keys section](/images/5-Workshop/5.2-Prerequisite/5.2.11.png)

**Step 3.4:** Select use case **Command Line Interface (CLI)** → check the acknowledgement below → click **Next** → click **Create access key**

![Select CLI use case](/images/5-Workshop/5.2-Prerequisite/5.2.12.png)

![Confirm and create access key](/images/5-Workshop/5.2-Prerequisite/5.2.13.png)

**Step 3.5:** The screen displays credentials — **save immediately:**
- `Access Key ID` (format: `AKIAIOSFODNN7EXAMPLE`)
- `Secret Access Key` (format: `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`)

Or click **Download .csv file** to download a file containing both values.

> The Secret Access Key is **shown only once**. If you miss it, you must delete the old key and create a new one.

![Credentials created successfully](/images/5-Workshop/5.2-Prerequisite/5.2.14.png)

---

##### Step 4: Configure AWS CLI with the IAM User

Open Terminal (PowerShell on Windows, Terminal on macOS/Linux) and run:

```bash
aws configure
```

Fill in each line:

```
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: ap-southeast-1
Default output format [None]: json
```

> This information is saved at `~/.aws/credentials` and `~/.aws/config` on your machine.

![Configure AWS CLI](/images/5-Workshop/5.2-Prerequisite/5.2.15.png)

---

##### Step 5: Verify Successful Login

```bash
aws sts get-caller-identity
```

Expected output — the `Arn` field must contain the IAM User name, **not root**:

```json
{
    "UserId": "AIDAXXXXXXXXXXXXXXXXX",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/ai-assistant-deployer"
}
```

If you see `user/ai-assistant-deployer` in the Arn → configuration is correct, ready to deploy.

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

After completing this step, you are ready to start setting up the AWS services.
