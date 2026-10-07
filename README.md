# 🤖 AI Email Automation with n8n

An AI-powered email automation workflow built with **n8n, OpenAI, Supabase, Gmail, and Telegram** to automate repetitive email processing while keeping a human in control of the final response.

This project demonstrates how **AI, vector search, workflow automation, and human oversight** can be combined to create a practical business email automation system.

---

## 📌 Problem Statement

Businesses can receive hundreds or thousands of emails every day from customers, users, and other stakeholders.

Manually reading, understanding, researching, and replying to every email can become:

- Time-consuming
- Repetitive
- Difficult to scale
- Slow for customer support teams
- Inefficient during high-volume periods

The objective of this project is to automate the repetitive parts of email processing while keeping a human involved before the final response is sent.

---

## 🎯 Project Objectives

The workflow is designed to:

- Automatically detect incoming emails
- Extract and process email content
- Classify incoming emails
- Use an AI Agent to understand the request
- Retrieve relevant information from a knowledge base
- Generate contextual email responses
- Create Gmail drafts automatically
- Notify a human reviewer through Telegram
- Allow the reviewer to review and edit the response
- Send the final response only after human approval

---

# 🏗️ Workflow Architecture

![Complete Workflow](<automation-setup.png>)

The workflow connects multiple services into a single automated email-processing pipeline.

### High-Level Flow

```text
Incoming Email
      ↓
Gmail Trigger
      ↓
Email Body
      ↓
Mail Classifier
      ↓
AI Agent
      ↓
Knowledge Retrieval
      ↓
Supabase Vector Store
      +
OpenAI Embeddings
      ↓
Relevant Context
      ↓
Generate Response
      ↓
Create Gmail Draft
      ↓
Telegram Notification
      ↓
Human Review
      ↓
Edit / Approve
      ↓
Send Final Response
```

The core design follows:

```text
AI Generation
      ↓
Gmail Draft
      ↓
Human Review
      ↓
Final Response
```

This ensures that the AI-generated response is reviewed before being sent to the customer.

---

# ⚙️ How the Workflow Works

## 1. Gmail Trigger

The workflow starts when a new email arrives in the connected Gmail inbox.

The **Gmail Trigger** detects the incoming message and starts the automation.

![Incoming Email](delivered.png)

The screenshot demonstrates the incoming email that initiates the workflow.

---

## 2. Email Body Processing

After the email is detected, the workflow extracts the relevant message content.

The email information is then passed through the workflow so that the classifier and AI Agent can process the request.

The workflow works with information such as:

- Sender
- Subject
- Email body
- Message content
- Relevant email information

---

## 3. Mail Classification

The incoming email is passed through the **Mail Classifier**.

The classifier determines the type of request being received and helps prepare the message for the next stage of processing.

This is particularly useful for businesses handling large volumes of different types of customer communication.

Instead of requiring a person to manually determine the category of every email, the workflow automates this step.

---

# 🧠 AI Agent

The classified email is passed to an **AI Agent**, which acts as the reasoning layer of the workflow.

The AI Agent processes the incoming request and can use retrieved information from the connected knowledge base when generating a response.

The objective is to generate a response that is relevant to the customer's request rather than producing a generic reply.

---

# 🔎 Vector Search, Embeddings & RAG

The workflow uses **OpenAI Embeddings** and a **Supabase Vector Store** to retrieve relevant information from a knowledge base.

### Retrieval Flow

```text
Knowledge Base
      ↓
OpenAI Embeddings
      ↓
Vector Representation
      ↓
Supabase Vector Store
      ↓
Semantic Retrieval
      ↓
Relevant Context
      ↓
AI Agent
      ↓
Generated Response
```

---

## 🧩 Embeddings

Embeddings represent information as numerical vector representations.

This allows the system to retrieve information based on **semantic similarity**, rather than depending only on exact keyword matches.

For example, a customer could ask:

> "How can I change my subscription?"

while the knowledge base could contain information about:

> "Instructions for modifying a subscription plan."

Although the wording is different, the meaning is similar.

Vector embeddings make this type of semantic retrieval possible.

---

## 🗄️ Supabase Vector Store

**Supabase** is used as the vector storage layer for the workflow.

The knowledge base can be represented as vector embeddings and stored in Supabase.

When an incoming email is processed, relevant information can be retrieved and provided to the AI Agent as context.

---

## 📚 Retrieval-Augmented Generation (RAG)

The workflow follows a **Retrieval-Augmented Generation (RAG)** approach.

Instead of relying only on the AI model's general knowledge, the workflow retrieves relevant information from the connected knowledge base before generating the response.

```text
Incoming Email
      ↓
AI Agent
      ↓
Search Knowledge Base
      ↓
Supabase Vector Store
      ↓
Relevant Information
      ↓
AI Agent
      ↓
Contextual Response
```

This helps the generated response use information relevant to the specific business context.

---

# ✉️ Gmail Draft Creation

After the AI Agent generates the response, the workflow creates a **draft in Gmail** instead of immediately sending the email.

![Gmail Draft Created](<draft saved in mail.png>)

Creating a draft creates an additional control point between AI generation and final communication.

The reviewer can inspect the generated response before it reaches the customer.

---

## 📄 Saved Draft

![Saved Draft](<draft saved.png>)

The response is stored as a Gmail draft and remains available for review before sending.

---

# 👤 Human-in-the-Loop Review

A key part of the workflow is the **Human-in-the-Loop** approach.

The AI generates the initial response, but a human remains responsible for reviewing and approving the final message.

```text
AI Generated Response
        ↓
     Gmail Draft
        ↓
   Human Review
        ↓
   Edit if Needed
        ↓
      Approve
        ↓
       Send
```

This provides an additional level of control over AI-generated communication.

---

# 📝 Edit or Review Draft

![Edit or Review Draft](<edit or review draft.png>)

The reviewer can inspect the generated response and make changes when necessary.

The reviewer can:

- Check the response
- Correct mistakes
- Improve the wording
- Add missing information
- Decide whether the response is appropriate
- Approve the message for sending

---

# 📱 Telegram Notification

Telegram is used as a notification and review channel.

When a draft is ready, the workflow sends a notification through Telegram.

![Telegram Notification](<notification from telegram.png>)

This allows the reviewer to quickly identify when a response is ready for review.

---

# 🔄 Telegram Review Process

The reviewer can interact with the workflow through Telegram to access the draft and continue the approval process.

![Telegram Review](<press draft.png>)

The review step creates a simple human approval layer between AI-generated content and the final customer response.

---

# 📤 Sending the Final Response

Once the response has been reviewed and approved, the workflow sends the final email.

![Send Final Response](<press send.png>)

The complete process becomes:

```text
Incoming Email
      ↓
Email Classification
      ↓
AI Processing
      ↓
Knowledge Retrieval
      ↓
Response Generation
      ↓
Gmail Draft
      ↓
Telegram Notification
      ↓
Human Review
      ↓
Approval
      ↓
Final Email
```

---

# 📸 Workflow Demonstration

## Incoming Email

![Incoming Email](delivered.png)

A new email arrives in Gmail and triggers the automation workflow.

---

## AI-Generated Gmail Draft

![Generated Gmail Draft](<draft saved in mail.png>)

The generated response is automatically saved as a draft in Gmail.

---

## Draft Saved

![Draft Saved](<draft saved.png>)

The draft is stored before the final response is sent.

---

## Draft Review

![Draft Review](<edit or review draft.png>)

The reviewer can inspect the generated response and modify it when necessary.

---

## Telegram Notification

![Telegram Notification](<notification from telegram.png>)

Telegram is used to notify the reviewer that a generated response is ready.

---

## Telegram Review Action

![Telegram Review](<press draft.png>)

The reviewer can access and review the response through the Telegram interaction.

---

## Send Action

![Send Action](<press send.png>)

The reviewer approves the response and triggers the final sending step.

---

## Final Sent Response

![Final Response](<draft sent.png>)

The approved response is sent to the customer.

---

## Email Writing Stage

![Writing Email](<writing mail.png>)

This demonstrates the email-writing stage of the automation workflow.

---

# 💼 Business Value

The primary business problem addressed by this project is **high-volume email support**.

Consider a business that receives hundreds or thousands of customer emails every day.

A traditional process might look like:

```text
Email
  ↓
Human reads email
  ↓
Human understands request
  ↓
Human searches for information
  ↓
Human writes response
  ↓
Human sends response
```

This process requires significant manual effort.

The automated workflow changes this to:

```text
Email
  ↓
AI Classification
  ↓
Knowledge Retrieval
  ↓
AI Response Generation
  ↓
Gmail Draft
  ↓
Human Review
  ↓
Final Response
```

### Potential Business Benefits

- Reduce repetitive support work
- Improve response speed
- Handle larger volumes of email
- Improve consistency of responses
- Reduce manual processing
- Allow employees to focus on complex cases
- Maintain human oversight over customer communication

The purpose of the system is not to remove humans from the support process.

Instead, it uses AI to **automate repetitive work while keeping humans responsible for the final response**.

---

# 🛠️ Technology Stack

| Technology | Purpose |
|---|---|
| **n8n** | Workflow automation and orchestration |
| **Gmail** | Incoming emails, drafts, and final responses |
| **OpenAI** | AI reasoning and response generation |
| **OpenAI Embeddings** | Create vector representations |
| **Supabase** | Vector storage and semantic retrieval |
| **Telegram** | Notifications and human review |

---

# ⭐ Key Features

- Gmail-based email triggering
- Automated email classification
- AI-powered response generation
- Knowledge-base retrieval
- Vector embeddings
- Supabase Vector Store
- Retrieval-Augmented Generation (RAG)
- Automatic Gmail draft creation
- Telegram notifications
- Human-in-the-loop review
- Human approval before sending
- Multi-service workflow orchestration

---

# 🧠 Key Concepts Demonstrated

## Workflow Automation

Using **n8n** to connect multiple applications and automate an end-to-end business process.

## AI Agents

Using an AI Agent to process incoming requests and generate appropriate responses.

## Vector Embeddings

Converting information into numerical vector representations to support semantic search.

## Vector Databases

Using **Supabase Vector Store** to store and retrieve vectorized information.

## Retrieval-Augmented Generation

Retrieving relevant knowledge before generating an AI response.

## Human-in-the-Loop AI

Keeping a human involved before the final response is delivered.

## API & Service Integration

Connecting multiple external services into a single automated workflow.

---

# 📁 Repository Structure

```text
n8n-ai-email-automation/
│
├── README.md
│
├── Automation Setup.png
├── delivered.png
├── draft saved in mail.png
├── draft saved.png
├── draft sent.png
├── edit or review draft.png
├── notification from telegram.png
├── press draft.png
├── press send.png
└── writing mail.png
```

---

# 🔐 Security & Production Considerations

This project demonstrates an AI-assisted automation workflow.

A production implementation would require additional considerations such as:

- Secure credential management
- OAuth and access control
- Email data privacy
- User authentication
- Role-based access
- Monitoring and logging
- Error handling
- Rate limiting
- AI response validation
- Prompt-injection protection
- Sensitive information handling

API keys, credentials, tokens, and other private configuration values should never be committed to a public repository.

---

# 🚀 Future Improvements

Potential improvements for a production-ready implementation include:

- Automatic email priority detection
- Sentiment analysis
- Automatic escalation of urgent requests
- Customer conversation history
- Confidence scoring for AI responses
- Multiple support categories
- Automatic routing to different support teams
- Response analytics dashboard
- Response-time monitoring
- AI response quality checks
- Advanced error handling
- Human approval tracking

---

# 📊 Example Business Scenario

Consider a SaaS company receiving a large number of customer emails every day.

A customer might send:

> "I was charged twice for my subscription. Can you help?"

The workflow could process the request as follows:

```text
1. Receive the email
        ↓
2. Classify the request
        ↓
3. Send it to the AI Agent
        ↓
4. Retrieve relevant information
        ↓
5. Generate a contextual response
        ↓
6. Create a Gmail draft
        ↓
7. Notify the support employee through Telegram
        ↓
8. Human reviews the response
        ↓
9. Human edits if necessary
        ↓
10. Human approves the response
        ↓
11. Send the final email
```

This approach can reduce repetitive manual work while maintaining human control over customer communication.

---

# 📚 What I Learned

Through this project, I explored and practiced:

- n8n workflow automation
- AI Agents
- OpenAI integrations
- Prompt-based AI processing
- Embeddings
- Vector databases
- Supabase Vector Store
- Retrieval-Augmented Generation
- Gmail automation
- Telegram automation
- Human-in-the-loop AI systems
- API integrations
- Business process automation

---

# 🎯 Project Outcome

This project demonstrates how AI can be integrated into a **real-world business workflow** instead of being used only as a standalone chatbot.

The workflow combines:

```text
Workflow Automation
        +
AI Reasoning
        +
Knowledge Retrieval
        +
Human Review
        =
AI-Assisted Email Support
```

The final system automates repetitive email-processing tasks while maintaining human control over the final response.

---

# 👨‍💻 Author

**Nikhil Singh Mahar**

GitHub: [@maharnikhil](https://github.com/maharnikhil)

---

⭐ **If you found this project interesting, feel free to explore the workflow and implementation.**
