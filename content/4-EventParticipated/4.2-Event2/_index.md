---
title: "Event 2"
date: 2026-05-09
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---
# SUMMARY REPORT: AUTOMATED PROMPT ENGINEERING - ENHANCING LLM OUTPUT QUALITY

### Event Objectives

* Share the art of communicating with AI and methods to optimize the output quality of Large Language Models (LLMs).
* Highlight the importance of Prompt Engineering and the negative impacts of poorly designed prompts.
* Provide structures, principles, and techniques from basic to advanced levels for effective interaction with LLMs.
* Introduce the "Proptimizer" solution—an automated prompt optimization tool running on a Serverless architecture on AWS.

### Speakers

* **Nguyen Tuan Thinh** - DevOps/Cloud Engineer, member of the First Cloud AI Journey project.

### Key Highlights

#### 1. Negative Impacts of Poor-Quality Prompts
* **Generic Results:** Using generic prompts will only yield generic responses from the AI.
* **Wasted Costs:** Token waste due to verbose and unoptimized prompts significantly increases operating costs.
* **Inconsistency:** Ambiguous instructions cause the AI to return inconsistent results across runs.
* **Reduced Productivity:** Inefficient communication with AI wastes time spent revising and regenerating responses.

#### 2. Cấu trúc của một câu lệnh chuẩn (Great Prompt Structure)
A high-quality prompt should include the following core components:
* **Role:** The role the AI should assume (e.g., Career Consultant).
* **Instruction:** The specific task the AI needs to perform.
* **Context:** Relevant background information.
* **Input Data:** The raw data or text to be processed.
* **Output Format:** The structure, format, and tone of the expected output.
* **Examples:** Sample inputs and outputs (few-shot prompting) to guide the AI.
* **Constraints/Guidelines:** Limitations (length, words to avoid, specific areas to focus on).

#### 3. Token Economics
* **The Concept of Tokens:** LLMs process text based on tokens (sub-word units). The number of tokens consumed varies by language (Vietnamese typically consumes more tokens than English for the same meaning).
* **Cost Discrepancy:** Input tokens are significantly cheaper than output tokens (e.g., input tokens cost around \$5 per million, while output tokens can cost up to \$25 per million).

#### 4. Advanced Prompting Techniques
* **Chain-of-Thought (CoT):** Directs the AI to explain its step-by-step reasoning logic before presenting the final answer.
* **Self-Consistency:** Combines with CoT to run multiple independent reasoning paths and select the most common and logical answer.
* **Tree-of-Thoughts (ToT):** Structures reasoning as a decision tree to evaluate multiple optimal paths.
* Techniques such as **Retrieval-Augmented Generation (RAG)** and **Role Prompting**.

#### 5. Proptimizer Architecture
The Proptimizer solution is a browser extension that automates prompt optimization, built 100% Serverless on AWS with extremely optimized infrastructure costs:
* **Frontend & Distribution:** Uses AWS CloudFront (CDN) combined with Amazon S3 to store and distribute static web content.
* **Authentication & API:** Amazon Cognito manages user identities; Amazon API Gateway routes traffic and AWS Lambda processes backend logic without server management.
* **AI & Data Integration:** Connects to multiple AI models (Claude, GPT) via Amazon Bedrock, while storing prompt history at high speed using Amazon DynamoDB.
* **Monitoring:** Manages application logs and performance metrics using Amazon CloudWatch.

### Key Takeaways

#### Design Mindset
* **Focus on Positive Instructions:** Clearly describe what the AI **should do** (DOs) instead of only listing what it should not do (DON'Ts).
* **Structural Optimization:** Use clear delimiters to separate parts of the prompt and break down long inputs to help the AI ingest information.
* **Hallucination Prevention:** Instruct the AI to respond with "I don't know" if the source data is missing, and avoid assigning complex mathematical calculations directly to raw LLMs.

#### Technical Architecture
* Understood how to integrate AWS cloud infrastructure with Generative AI applications.
* Learned to use **Amazon Bedrock** as a secure API gateway to access multiple foundation models.
* Designed **NoSQL DynamoDB** database tables optimized for prompt history queries with single-digit millisecond latency.

#### Applying to Work
* **Apply the 7-component prompt structure** to daily interactions with AI to maximize accuracy.
* **Optimize Token Costs:** Write concise instructions and eliminate redundant words to reduce unnecessary token consumption.
* **Leverage Advanced Prompting:** Apply Chain-of-Thought (CoT) to tasks requiring logical analysis and code review.
* **Design Serverless Applications:** Formulate plans to develop internal helper tools utilizing Serverless architecture (S3, Lambda, Bedrock).

### Event Experience

#### Learning from Experts
* Gained a combined perspective of software engineering and model prompting, helping clarify the flow of building products integrated with GenAI.

#### Visual Learning
* Visualized the reasoning process of advanced prompting techniques through clear CoT and ToT diagrams.
* Absorbed the real-world architectural design of the Proptimizer solution on AWS.

#### Lessons Learned
* Prompt Engineering skills are key to unlocking the practical potential of AI; optimized prompts maximize work efficiency.
* Serverless architecture on AWS is an excellent fit for AI projects due to its minimal startup cost (\$0) and seamless scalability.

> Overall, the event helped me significantly improve my prompt design skills and expanded my mindset for building efficient AI products on the cloud.
