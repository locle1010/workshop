---
title: "Blog 1"
date: 2026-04-26
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
# Experience Amazon Nova Act: UI Automation with AI, No More XPath Fixing

> **Original Post:** [AWS Study Group - FCAJ](https://www.facebook.com/groups/awsstudygroupfcj/posts/2175053513259609)

![Amazon Nova Act](/images/3-BlogsTranslated/Blog1_photo.jpg)

Hello everyone! Today, I want to share my experience with an automation service I recently discovered and explored. I was struggling with writing automation scripts to test UI for our team project. The Frontend developer kept updating the interface constantly, causing my Selenium script to fail after a short run because it couldn't find the Selectors or XPath. While feeling reluctant to continue, I found Amazon Nova Act. Initially, I thought it was just a prompt wrapped around Selenium. However, after experimenting with it to solve my tasks, it proved to be completely different. Today, I will provide a detailed review of this highly promising tool.

## The Pain of Maintaining Scripts in Traditional Web Automation

Before going into detail, let me quickly explain for those who are unfamiliar with the concept. **Web Automation** is simply writing code to direct a computer to automatically open a browser and perform actions (click, scroll, fill text...) just like a human. This task is extremely common in the software industry:

*   **Automation Testing:** Automatically running websites to test for bugs before releasing them to users.
*   **Web Scraping:** Automatically gathering information from various websites for analysis.
*   **Office Automation:** Automatically performing repetitive tasks like reading data from Excel and entering it into internal admin websites.

Normally, to do this, we use traditional tools like Selenium, Playwright, or Puppeteer. They work by requiring us to specify the exact "coordinates" of buttons or inputs on the webpage (known as XPath or CSS Selectors) so the code knows where to click.

It sounds simple, but it stops being simple when the Frontend developer updates the UI, renames a CSS class, or wraps a button in a different HTML tag. This immediately breaks the XPath we worked hard to find. When you run the script after a short break, you see red errors filling the screen. Consequently, we must constantly chase UI changes just to fix the code and maintain those scripts, leaving little time for sleep.

## How Does Nova Act Solve This Problem?

Essentially, **Amazon Nova Act** is a service that helps you build AI Agents capable of controlling web browsers just like a real human. The key difference is that it does not use XPath or CSS Selectors at all. Instead, you control it using English—natural language. For example, instead of finding the ID of a search bar and writing `.fill()`, you only need to write:

```python
nova.act("Type 'iPhone' into the search bar and then press enter")
```

The Nova Act system uses the Amazon Nova 2 Lite foundation model, which is specially trained using Reinforcement Learning in simulated environments called "Web Gyms." It automatically identifies the search bar or the login button based on the visual layout and page structure at that moment, rather than relying strictly on HTML code. According to AWS, its accuracy in real-world environments exceeds 90%—a score good enough for production use.

## From Playing in the Playground to Deploying to the Cloud

Getting started with Nova Act is quite smooth. Here are the steps I went through:

### Step 1: Experimenting on the Web Playground
Simply visit [nova.amazon.com/act](https://nova.amazon.com/act). The interface is completely no-code. You enter a URL (e.g., `amazon.com`), then type your request. You will see a simulated screen showing the AI automatically scrolling, moving the cursor, and clicking boxes. The technology is truly impressive!

### Step 2: Writing Python Code with the SDK & IDE Extension
After playing enough on the web, I wanted to write real code. I installed the library using:
```bash
pip install nova-act
```
AWS also provides a VS Code extension that supports this exceptionally well.

Nova Act also supports **Hybrid** workflows. You can combine AI actions (`nova.act(...)`) with pure Python code logic between steps. For example, you can write a script to auto-fill a customer registration form using data from an Excel file. The AI handles the form filling, while Python reads the Excel file and executes the `for` loop.

Here is a mock-up of the code I tried writing:
```python
for customer in customer_list:
    nova.act(f"Fill name {customer['name']} and email {customer['email']} into form register")
    nova.act("Click Submit button and wait page reload")
```

While coding, the Extension opens a small embedded browser window right next to your IDE for "live debugging." Whichever line of code you execute, the adjacent browser runs the action immediately.

### Step 3: Deploying to AWS and Running "Fleet"
Once your code runs successfully locally, instead of manually setting up Docker, building headless Chrome on EC2 or Lambda, etc., you just click the Deploy button on the Extension. Nova Act packages everything into a container, pushes it to Amazon ECR, and runs it on the Bedrock AgentCore infrastructure. Whenever the script triggers, AWS automatically creates an isolated browser sandbox environment. You can scale up to run hundreds of workflows concurrently without worrying about overloading your local machine.

## 3 Core Features That Set Nova Act Apart

1.  **Human-in-the-Loop (HITL):** This is an incredibly smart feature. When the AI runs automatically and encounters a CAPTCHA or a locked account, it doesn't stop or throw an error to crash the system. Instead, it sends a notification via Amazon SNS to the administrator containing a secure DevTools link. You simply click the link, solve the CAPTCHA manually, and the AI automatically resumes the subsequent steps.
2.  **Visual Observability:** On the AWS Console, you can review the execution history as video recordings or view screenshots at each action step taken by the AI to verify if it performed correctly.
3.  **Enterprise-Grade Security:** Because it runs entirely inside AWS sandbox environments, you can define very granular IAM permissions for each workflow, eliminating the risk of session data or cookie leakage.

## Conclusion

Encountering the concept of "Agentic AI" for the first time, I was thoroughly convinced by how AWS designed Amazon Nova Act. It is not just a demo for fun, but a well-architected system aimed at solving actual enterprise operational challenges.

If you are a QA engineer, DevOps practitioner, or looking to automate repetitive office tasks, you should definitely try Nova Act. Here are the official documentation links to get started:

*   **Playground Homepage:** <https://nova.amazon.com/act>
*   **AWS News Blog - Build Reliable AI Agents for UI Workflow Automation:** [AWS News Blog](https://aws.amazon.com/blogs/aws/introducing-amazon-nova-models-in-amazon-bedrock/)
*   **AWS Technical Documentation:** <https://docs.aws.amazon.com/nova-act/latest/userguide/what-is-nova-act.html>

What do you think about this service? Will it be capable of replacing Selenium entirely in the future? Leave your comments below to discuss! Happy coding!
