---
title: "Event 1"
date: 2026-05-09
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---
# SUMMARY REPORT: AUTOMATED PROMPT ENGINEERING - ENHANCING LLM OUTPUT QUALITY

### I. Event Objectives & Target

*   **Mastering Communication**: Explore the art of directing AI (Prompting) and methods to enhance the response quality of Large Language Models (LLMs).
*   **Analyzing Errors**: Highlight the critical role of prompt design and the negative consequences of generic, unoptimized prompts.
*   **Skill Building**: Provide a structural framework and techniques from basic to advanced levels to maximize AI interaction quality.
*   **Application Showcase**: Introduce the "Proptimizer" tool—an automated prompt optimization extension built on an AWS Serverless architecture.

### II. Speaker

*   **Speaker**: **Nguyen Tuan Thinh** (DevOps/Cloud Engineer, member of the First Cloud AI Journey project).

### III. Key Technical Highlights

#### A. The Real Cost of Poor-Quality Prompts
*   **Superficial Content**: Sending a generic prompt will only yield shallow, generic responses from the AI.
*   **Financial Waste**: Using verbose, repetitive prompts results in unnecessary token consumption, raising API billing costs.
*   **Result Inconsistency**: Vague instructions cause the AI to generate inconsistent answers across different runs.
*   **Time Loss**: Spending valuable time revising answers and request regenerations manually lowers overall productivity.

#### B. The Standard Great Prompt Structure
A high-quality prompt must be organized systematically using the following components:
1.  **Role**: Establish the persona for the AI (e.g., Security Analysis Expert).
2.  **Instruction**: Precisely define what the AI needs to execute.
3.  **Context**: Background information that helps the AI understand the task's environment.
4.  **Input Data**: The raw text or data to be processed.
5.  **Output Format**: Expected structure of the output (JSON, Markdown, Table...).
6.  **Examples**: Provide few-shot sample pairs to guide the AI on tone and style.
7.  **Constraints**: The rules the AI must follow (e.g., maximum length, terms to avoid).

#### C. Token Economics
*   **Definition**: LLMs process text in sub-word units called tokens. Notably, accented languages like Vietnamese consume significantly more tokens than English for the same meaning.
*   **Cost Management**: Processing input tokens is much cheaper than generating output tokens; thus, optimizing response length is critical for controlling API expenses.

#### D. Specialized Prompting Methods
*   **Chain-of-Thought (CoT)**: Prompt the AI to explain its step-by-step reasoning logic before showing the final result.
*   **Self-Consistency**: Run multiple independent reasoning paths in parallel and select the answer with the highest consensus.
*   **Tree-of-Thoughts (ToT)**: Model reasoning as a decision tree to evaluate and choose optimal analysis branches.
*   Integrating **RAG** (Retrieval-Augmented Generation) and **Role Prompting**.

#### E. Proptimizer Solution Architecture
Proptimizer is a browser extension that automates prompt optimization, running 100% Serverless on AWS to minimize operating costs:
*   **Static Distribution**: Uses Amazon S3 for frontend storage combined with Amazon CloudFront as a CDN for global delivery.
*   **API Orchestration**: Uses Amazon Cognito for identity management, Amazon API Gateway for routing, and AWS Lambda to run backend logic without server maintenance.
*   **AI Integration & Databases**: Accesses foundation models (Claude, GPT) via Amazon Bedrock, and stores prompt history at high speed using DynamoDB.
*   **System Monitoring**: Uses Amazon CloudWatch to collect logs and monitor performance.

### IV. Acquired Knowledge & Application

#### Prompt Design Principles
*   Focus on positive instructions (DOs) that guide the AI on what to do, rather than vague negative warnings (DON'Ts).
*   Optimize prompt layout using clear delimiters (such as `---`, `###`) and split long input files.
*   Reduce hallucinations by instructing the AI to say "I don't know" when information is missing.

#### Cloud System Techniques
*   Understand the integration methods between AWS Serverless infrastructure and Generative AI models.
*   Leverage **Amazon Bedrock** as a secure, centralized API gateway to manage connections to multiple foundation model vendors.
*   Design **DynamoDB** tables optimized for prompt history queries with single-digit millisecond latency.

#### Actions at Work
*   **Apply the 7-component prompt structure** in daily AI interactions to maximize response quality.
*   **Reduce Operating Costs**: Refine prompt wording to save unnecessary token consumption.
*   **Utilize CoT** for source code analysis and code logic reviews.
*   **Develop Internal Tools**: Plan the creation of internal automation tools utilizing Serverless architectures (S3, Lambda, Bedrock).

### V. On-Site Practical Experience

#### Perspective from the Expert
*   The presentation offered a complete view of combining software engineering with AI prompting, clarifying the development pipeline of practical GenAI integrations.

#### Visual Exposure
*   Visualized the execution paths of CoT and ToT techniques through clear structural diagrams.
*   Grasped the real-world architectural design of the Proptimizer solution deployed on AWS.

#### Key Takeaway
*   Prompt Engineering is not just about asking questions, but the key to unlocking AI's practical value in daily work.
*   AWS Serverless is highly suited for AI integrations due to its minimal initial cost (\$0) and seamless scaling capabilities.

> The event provided valuable insights, improving my prompt design skills and expanding my mindset for building efficient AI products on the cloud.

![Event Participation Image](/images/4-EventParticipated/event1_picture.jpg)

