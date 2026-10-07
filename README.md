# AI Email Assistant with n8n

An AI-powered email automation workflow built with **n8n, Gmail, OpenAI, Supabase Vector Store, and Telegram**.

The goal is to reduce the manual workload created by high-volume email support. When a company receives hundreds or thousands of emails, manually reading, understanding, and replying to every message can slow response times and create repetitive work. This workflow automates the repetitive part: **classifying incoming email, retrieving relevant knowledge, generating a reply draft, and sending that draft to Telegram for human review before it is used.**

> **Important:** This project is designed as a **human-in-the-loop** system. The AI generates and prepares the response; a person reviews the draft before the final reply is sent.

---

## What Problem Does It Solve?

Customer-support and operations teams can receive a very large number of emails every day. A large portion of those messages can be repetitive questions such as:

- Refund and return requests
- Order and delivery questions
- Product or service information
- General support requests
- Policy questions

Handling each message manually can lead to:

- Slow response times
- Repetitive work for support teams
- Inconsistent replies
- Difficulty scaling support as email volume grows

This workflow acts as an **AI-assisted first layer of support**. It prepares a context-aware response quickly while keeping a human in control of the final communication.

---

## Workflow Overview

```text
Incoming Gmail Email
        ↓
   Gmail Trigger
        ↓
     Email Body
        ↓
    Mail Classifier
        │
        └── Support Service
                ↓
             AI Agent
          ↙      ↓       ↘
        LLM    Knowledge   Gmail
               Retrieval     Draft
                 ↓             ↓
        Supabase Vector     Store Draft
             Store              ↓
                 ↑         Telegram Review
                 │              ↓
        OpenAI Embeddings   Human Review
                                ↓
                         Approve / Send
```

The workflow separates **classification, reasoning, knowledge retrieval, drafting, storage, and human review** instead of putting everything into one step.

---

## Architecture

![Workflow architecture](assets/01-workflow-architecture.png)

### What the architecture shows

**1. Gmail Trigger**  
Starts the workflow when a new email arrives.

**2. Email Body**  
Extracts and prepares the email content for downstream processing.

**3. Mail Classifier**  
Determines which type of email has arrived. The demonstrated workflow includes routes such as **Promotional** and **Support Service**.

**4. AI Agent**  
Handles the response-generation process for the relevant support route.

**5. LLM**  
Provides the language-model reasoning used by the AI Agent.

**6. Brain / Knowledge Retrieval**  
The AI Agent is connected to a knowledge-retrieval component so that responses can use relevant stored information instead of relying only on the model's general knowledge.

**7. Supabase Vector Store**  
Stores vectorized knowledge that can be retrieved based on semantic similarity.

**8. OpenAI Embeddings**  
Converts knowledge into numerical vector representations that can be searched semantically.

**9. Drafting Gmail**  
Creates a response as a Gmail draft rather than immediately sending an automated message.

**10. Store Draft**  
Records the generated draft as part of the workflow before review.

**11. Telegram Review**  
Sends the generated response to Telegram so a human can inspect the draft and decide what happens next.

---

# Workflow Explained Step by Step

## 1. A new email arrives

The **Gmail Trigger** detects a new incoming message and starts the automation.

This makes the workflow **event-driven**: there is no need for a person to manually copy the email into an AI tool.

## 2. Email content is extracted

The **Email Body** step prepares the message content so the downstream nodes can work with the actual request.

## 3. The email is classified

The **Mail Classifier** decides what kind of message it is.

This is important because not every email should follow the same path. A promotional message, for example, may not need the same response workflow as a customer-support request.

Classification allows the automation to **route emails to the appropriate logic**.

## 4. The AI Agent processes the support request

For the support route, the message is passed to the **AI Agent**.

The agent uses an LLM together with the connected knowledge source to understand the request and produce a useful response.

## 5. Relevant knowledge is retrieved

The workflow uses a **Supabase Vector Store + OpenAI Embeddings** to support semantic retrieval.

Instead of searching for only exact keywords, vector search represents text numerically and can retrieve information that is conceptually related to the customer's question.

For example, a customer may ask:

> “Can I send this product back and get my money returned?”

while the knowledge base may contain a document titled:

> “Refund and Return Policy”

Semantic retrieval helps connect those two concepts.

This approach is commonly described as **Retrieval-Augmented Generation (RAG)** because the model generates the answer using retrieved external context.

## 6. A Gmail draft is created

The AI does **not** directly send the customer-facing message in the demonstrated workflow.

Instead, the response is created as a **Gmail draft**.

This gives the support team an opportunity to:

- Check accuracy
- Change the tone
- Add missing details
- Remove incorrect information
- Decide whether the response should be sent at all

## 7. The draft is sent to Telegram for review

The workflow sends the draft to Telegram, creating a quick review channel for the human operator.

This is the **human-in-the-loop** part of the system.

The AI handles repetitive drafting, while the human keeps control over the final response.

## 8. The human reviews and sends the reply

After reviewing the draft, the operator can edit it if necessary and send the final response.

This creates a balance between **automation and control**.

---

# Key Concepts Demonstrated

## Event-Driven Automation

A new Gmail message acts as the event that starts the workflow.

## Email Classification and Routing

Incoming messages are categorized before deeper processing. This avoids applying the same logic to every type of email.

## AI Agent

The AI Agent combines the language model with additional context and tools to complete a task rather than simply producing an isolated text response.

## Embeddings

OpenAI Embeddings transform text into vectors that capture semantic relationships between pieces of information.

## Vector Search

The Supabase Vector Store provides semantic retrieval of relevant knowledge.

## Retrieval-Augmented Generation (RAG)

Relevant information is retrieved from an external knowledge source and supplied to the model while generating the response.

## Human-in-the-Loop

The generated response is reviewed by a person before the final message is sent.

## Draft-Based Automation

Instead of fully autonomous communication, the workflow creates a draft first. This reduces the risk of sending an incorrect or inappropriate response automatically.

## Workflow Orchestration

n8n connects the different services and manages the flow from email ingestion to classification, AI processing, knowledge retrieval, drafting, storage, and notification.

---

# Why This Approach Matters

The value is not simply **“AI writes emails.”**

The more useful idea is combining several components into a controlled workflow:

**Detect → Classify → Retrieve → Generate → Draft → Review → Send**

This can help support teams respond faster while reducing repetitive manual work.

For a business handling a high volume of customer email, the system can act as an **AI-assisted support layer** that prepares responses quickly and consistently, while humans remain responsible for the final communication.

---

# Example Flow

A simple support request can move through the workflow like this:

```text
Customer sends email
        ↓
Gmail detects the message
        ↓
Email is classified as Support Service
        ↓
AI Agent analyzes the request
        ↓
Relevant policy information is retrieved
        ↓
AI generates a contextual response
        ↓
Gmail draft is created
        ↓
Draft is stored
        ↓
Telegram notification is sent
        ↓
Human reviews / edits
        ↓
Final reply is sent
```

---

# Screenshots

## 1. Full workflow architecture

The complete n8n workflow showing the connection between Gmail, classification, the AI Agent, vector retrieval, Gmail drafting, and Telegram review.

![Full workflow](assets/01-workflow-architecture.png)

## 2. Test email / input

A sample support request is used to test the automation from the incoming-email stage.

![Input email](assets/02-input-email.png)

## 3. Draft generated by the workflow

The workflow produces a response draft based on the incoming request and retrieved context.

![Draft generated](assets/03-draft-saved.png)

## 4. Draft visible in Gmail

The generated response is stored in Gmail as a draft rather than being sent immediately.

![Gmail draft](assets/04-gmail-draft.png)

## 5. Reviewing the draft

The response can be inspected before the final message is sent.

![Review draft](assets/05-review-draft.png)

## 6. Telegram notification

Telegram acts as the review channel and receives the generated draft notification.

![Telegram notification](assets/06-telegram-notification.png)

## 7. Telegram draft review

The operator can review the generated content before deciding to send it.

![Telegram review](assets/07-telegram-draft-review.png)

## 8. Sending the approved reply

The final action is triggered only after the human review step.

![Send reply](assets/08-telegram-send.png)

## 9. Reply sent confirmation

The workflow confirms that the reply action has been completed.

![Reply sent](assets/09-reply-sent.png)

## 10. Final email result

The final Gmail state demonstrates the end-to-end result of the workflow.

![Email delivered](assets/10-email-delivered.png)

---

# Technology Stack

| Technology | Purpose |
|---|---|
| **n8n** | Workflow orchestration and automation |
| **Gmail** | Email trigger and draft creation |
| **OpenAI** | LLM reasoning and text embeddings |
| **Supabase** | Vector storage and semantic retrieval |
| **Telegram** | Human review and notification channel |

---

# Security and Production Considerations

This project is a portfolio/demo implementation. A production deployment should additionally consider:

- Secure credential and API-key management
- Access control for Gmail and Telegram
- Protection of customer PII and confidential email content
- Logging and monitoring
- Rate limits and retry handling
- Prompt-injection protection
- Validation of retrieved knowledge
- Approval rules for high-risk requests
- Data retention and deletion policies

A production system should also define which email categories are safe to automate and which must always be escalated to a human.

---

# Future Improvements

Potential improvements include:

- Automatic priority detection for urgent messages
- Sentiment analysis and escalation for frustrated customers
- Multiple knowledge bases for different departments
- Confidence scoring for generated drafts
- Automatic assignment to support teams
- Analytics on response time and ticket categories
- Feedback loops to improve response quality
- Additional channels such as Slack or Microsoft Teams
- Automatic handling for low-risk requests with configurable approval rules

---

# What I Learned

This project helped me understand how **workflow automation, LLMs, vector databases, embeddings, RAG, classification, and human review** can be combined into one practical business system.

The main lesson was that an effective AI application is not just about generating text. The real value comes from designing a reliable workflow around the model: **getting the right input, retrieving the right context, generating a useful response, and keeping humans in control where needed.**

---

## Author

**Nikhil Singh Mahar**

