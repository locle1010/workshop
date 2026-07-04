---
title: "Blog 2"
date: 2026-04-26
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---
# Combining Amazon Cognito and Amazon Verified Permissions for Fine-grained Access Control

> **Original Post:** [AWS Study Group - FCAJ](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2185975575500736)

![Amazon Cognito & Amazon Verified Permissions](/images/3-BlogsTranslated/Blog2_photo.jpg)

Hello AWS Study Group VN members!

While researching security for B2C applications, I read an interesting article from the AWS Security Blog on how to combine Amazon Cognito and Amazon Verified Permissions to implement fine-grained access control for applications.

Typically, when building applications, we need to address two distinct challenges:
*   **Authentication (AuthN):** Who is the user?
*   **Authorization (AuthZ):** What is the user allowed to do?

Amazon Cognito has long been a familiar service for managing user registration, login, and authentication. However, as systems grow to include more user roles, diverse resource types, and complex access rules, handling the authorization logic directly within the application source code makes it difficult to maintain and scale.

That is why AWS introduced **Amazon Verified Permissions**—a centralized access management service based on the Cedar policy language. Instead of writing scattered authorization conditions in code, we can define separate policies and let Verified Permissions evaluate access requests.

In the AWS-demonstrated example, Cognito handles user authentication and issues a JWT token. Upon successful login, the application sends the user information along with the access request to Amazon Verified Permissions. The service evaluates the request based on Cedar Policies to determine whether the action is permitted.

### Popular Access Control Models Discussed:
*   **Resource Ownership:** Users can only manipulate data they own.
*   **Role-Based Access Control (RBAC):** Permissions based on roles like Student, Faculty, Admin, etc.
*   **Hierarchical Permissions:** Inheriting permissions based on organizational levels.
*   **Administrative Override:** Administrators have the authority to bypass standard rules.
*   **Explicit Deny:** Denial policies always take the highest precedence.

I found it fascinating that AWS illustrated this using an academic system containing roles such as student, teaching assistant, instructor, dean, and administrator. Each role has a different access scope, yet all authorization logic is managed centrally via Verified Permissions instead of being hardcoded in the application.

After reading the article, the most notable takeaway for me is how AWS decouples authentication and authorization into two independent layers. For systems with multiple user groups or distinct access rules, this approach makes access management much clearer and more flexible.

### Key Benefits Include:
*   Complete separation of Authentication and Authorization.
*   Significant reduction of authorization code in the application.
*   Easy modifications to access rules without changing core business logic.
*   Enhanced auditing and governance of access permissions.
*   Well-suited for SaaS or B2C systems with large user bases and complex authorization hierarchies.

In my view, Amazon Cognito and Amazon Verified Permissions complement each other exceptionally well in solving authentication and authorization challenges. This is a highly recommended approach when building B2C applications that require fine-grained access control while ensuring scalability for future growth.

**Reference Source:** <https://aws.amazon.com/blogs/security/building-secure-b2c-applications-with-fine-grained-access-control-using-amazon-cognito-and-amazon-verified-permissions/>
