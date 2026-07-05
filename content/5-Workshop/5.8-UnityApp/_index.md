---
title: "Install Unity App"
date: 2026-07-04
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

#### Overview

The Desktop App is a Unity application running on the user's computer, allowing:

+ Chat with AI Assistant through an on-screen chat window
+ Sync data to the Cloud (AWS Lambda + DynamoDB)
+ User authentication via Cognito Hosted UI

In this step, you will install Unity Hub, download the exact Unity Editor version required for the project, open the project, and update the API connection settings.

---

#### Install Unity Hub

Unity Hub is a tool for managing Unity Editor versions and projects.

**Download Unity Hub at:** [https://unity.com/download](https://unity.com/download)

---

#### Sign in to Unity Hub

After opening Unity Hub, you need to sign in with your Unity ID:

1. Click **Sign in** in the top right corner
2. Sign in with your Unity ID (register for free at [https://id.unity.com](https://id.unity.com) if you don't have one)
3. After signing in successfully, return to Unity Hub

> The software allows individuals to use it **for free (Personal plan)** — sufficient to open and run this project.

---

#### Open the Project in Unity Editor

**Step 1:** In Unity Hub, switch to the **Projects** tab

**Step 2:** Click **Add** → Select **Add project from disk...**

![Add project from disk](/images/5-Workshop/5.7-UnityApp/5.7.1.png)

**Step 3:** Navigate to the project directory:
```
<project-directory>/app/
```
Select the `app` folder and click **Open**

**Step 4:** Unity Hub may ask about the Editor version. Select **2022.3.62f2** from the dropdown (download and install this version if not already installed)

**Step 5:** Click on the project to open it

![Open project in Unity Hub](/images/5-Workshop/5.7-UnityApp/5.7.2.png)

---

#### Update API URL in Unity Inspector

After the project opens and finishes compiling, you need to update the URLs in the following inspectors.

---

##### Script 1: ZME_CloudSync (Cloud Sync)

This script handles Cognito login and data synchronization to the backend.

**Step 1:** In the **Hierarchy** tab (left side of the screen), find and select the GameObject named **AWS_Manager**

**Step 2:** Switch to the **Inspector** tab (right side of the screen)

**Step 3:** Find the **"Backend API"** section and fill in the fields:

| Inspector Field | Value to Enter |
|-----------------|---------------|
| **Backend Api Url** | `https://<API_ID>.execute-api.ap-southeast-1.amazonaws.com/api/pet/profile` |
| **Cognito Domain** | `https://ai-assistant-auth.auth.ap-southeast-1.amazoncognito.com` |
| **Client Id** | `<ClientId from Step 5.3>` |

> Replace `<API_ID>` with the endpoint ID from the `serverless deploy` output in Step 5.4.

**Step 4:** Save the Scene with **Ctrl+S** (Windows) or **Cmd+S** (macOS)

![Update API URL in Unity Inspector](/images/5-Workshop/5.7-UnityApp/5.7.3.png)

---

#### Build Application as Standalone Executable

If you want to package it as an `.exe` file to run without Unity Editor:

1. Go to **File** > **Build Settings**

![Open Build Settings](/images/5-Workshop/5.7-UnityApp/5.7.4.png)

2. Select Platform: **PC, Mac and Linux Standalone**
3. Click **Switch Platform**
4. **Target Platform:** Windows
5. Click **Build** and choose the output directory

![Configure Build and export](/images/5-Workshop/5.7-UnityApp/5.7.5.png)

6. Run the `.exe` file just created

![Unity application running successfully](/images/5-Workshop/5.7-UnityApp/5.7.6.png)
