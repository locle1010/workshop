---
title: "Event 3"
date: 2026-06-27
weight: 1
chapter: false
pre: " <b> 4.3. </b> "
---
# SUMMARY REPORT: BUILDING ENTERPRISE-GRADE AI VOICE AGENTS AT SCALE

### Event Objectives

* Provide an overview of the inner workings of Voice AI and how to integrate voice capabilities into AI systems.
* Analyze the limitations of current Voice AI models for the Vietnamese language and solve language processing challenges in corporate environments (especially banking and finance).
* Explore the architectural standards required to move a Voice Agent from Proof of Concept (POC) to production.
* Inspire the development of natural, secure, and reliable human-computer voice interaction experiences.

### Speakers

* **Hieu Nghi**
* **Kiet**
* **Trung Do**

### Key Highlights

#### 1. Decoupled AI Architecture for Low-Resource Languages
* Most speech-to-speech models are optimized for English. Vietnamese is a low-resource language; hence, to operate reliably in enterprise environments, the system architecture is split into three components: **Speech-to-Text (STT) -> LLM Text Processing -> Text-to-Speech (TTS)**.
* This decoupled architecture allows businesses to strictly control AI responses (minimizing hallucinations) and execute complex tasks via Tool Calling (e.g., auto-blocking credit cards or checking account balances).

#### 2. Handling Cultural Context and Communication Flow
* **Accurate Addressing:** The system must recognize gender and age clues from the voice to address the customer appropriately and politely (e.g., "Anh" or "Chi" in Vietnamese).
* **Interruption Handling:** The AI must be trained to distinguish when a customer is pausing to think (e.g., reading a phone number in segments) versus when they have finished speaking, avoiding "talking over" the user.
* **Dialect & Accent Recognition:** The STT training dataset should contain 10% to 20% local dialects to ensure accurate transcription. However, the AI response voice (TTS) should remain in standard broadcast pronunciation to maintain professionalism.

#### 3. Optimizing Latency for Production Use
* **Latency:** Low latency is critical for Voice AI. The entire pipeline (STT, LLM, TTS) must be executed using continuous streaming rather than batch processing.
* **Human-in-the-Loop Safeguards:** The system must automatically detect when the conversation exceeds AI capabilities (e.g., customer expressing frustration) to seamlessly hand over the call to a human agent.

### Key Takeaways

#### Design Mindset
* Great AI products are not just about the core technology; they must focus on user experience, understanding natural human behaviors and communication cultures.
* When designing automation systems, always build a smooth fallback mechanism to transition to human agents at critical touchpoints.

#### Technical Architecture
* Mastered the trade-offs of the decoupled STT-LLM-TTS architecture for specialized language processing.
* Understood the vital roles of streaming and tool calling in building an active, real-time interactive Agent.

#### Applying to Work
* **Enhancing App Experience:** Apply the decoupled STT-LLM-TTS architecture to research voice assistant integrations in cross-platform mobile apps to support hands-free user interactions.
* **Infrastructure Optimization:** Apply streaming principles and latency optimization techniques to AWS cloud architectures to build high-speed, scalable automation flows.

### Event Experience

#### Learning from Industry Leaders
* Real-world insights highlighted the gap between building a simple toy AI demo and deploying a robust Voice Agent system serving millions of users in large commercial banks.

#### Practical Technical Exposure
* Observed a live demo of the Voice Agent system and learned how engineers tackle complex audio processing challenges for low-resource languages.
* Expanded knowledge on the potential of Generative AI when empowered with Tool Calling to automate complex business workflows.

#### Lessons Learned
* The decoupled STT-LLM-TTS architecture combined with Tool Calling is the optimal solution to overcome language resource limitations and ensure AI accuracy.
* Context-aware conversation handling and human-fallback mechanisms are essential for successful, real-world Voice AI operations.

> Overall, the event provided a comprehensive view of Voice AI technology, equipping me with practical knowledge to apply to my current and future projects.
