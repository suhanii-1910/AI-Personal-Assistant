# 🤖 AI Personal Assistant

An AI-powered personal assistant built with **Google Gemini AI and Make.com** that can understand user intent, route requests to specialized workflows, store and recall personal information, interact with calendars, retrieve information from PDFs, and perform web searches.

The system uses an **intent classification + workflow routing architecture**, allowing different types of requests to be handled by dedicated automation pipelines.

---

## 🚀 Overview

Instead of building one large AI workflow, this project uses a modular architecture:

```text
                    ┌──────────────────┐
                    │    User Query    │
                    └────────┬─────────┘
                             ↓
                    ┌──────────────────┐
                    │ Webhook Trigger  │
                    └────────┬─────────┘
                             ↓
                    ┌──────────────────┐
                    │ Gemini AI        │
                    │ Intent Classifier│
                    └────────┬─────────┘
                             ↓
                    ┌──────────────────┐
                    │     Router       │
                    └────────┬─────────┘
                             ↓
       ┌────────────┬────────┼─────────┬────────────┐
       ↓            ↓        ↓         ↓            ↓
   Memory       Calendar    PDF     Web Search   Retrieval
    Workflow     Workflow   Q&A       Workflow     Workflow
       ↓            ↓        ↓         ↓
   Response      Response  Response  Response
```

The assistant classifies each incoming request and sends it to the appropriate automation workflow.

---

## ✨ Features

### 🧠 Memory Management

The assistant can remember information provided by the user and retrieve it later.

**Examples:**

```text
"Remember that my favorite language is Python."

"What is my favorite language?"
```

Memory is converted into structured data and stored in **Google Sheets**.

Stored information follows a simple structure:

| User | Key               | Value    |
| ---- | ----------------- | -------- |
| User | favorite language | Python   |
| User | college           | SIT Pune |

## The Make.com workflow extracts the memory using Gemini, parses the JSON response, and stores it in Google Sheets.

### 📅 Calendar Automation

The assistant can understand calendar-related requests and route them to dedicated calendar workflows.

**Supported operations:**

* Create calendar events
* Schedule meetings
* Create reminders
* Retrieve upcoming events
* Check schedules

**Example:**

```text
"Schedule DSA practice tomorrow at 7 PM."

"What events do I have tomorrow?"
```

---

### 📄 PDF Knowledge Retrieval

Users can ask questions based on information available in their PDF knowledge base.

**Example:**

```text
"Explain polymorphism from my notes."

"What is temporal locality?"

"Summarize the PDF."
```

This allows the assistant to act as a personal knowledge assistant instead of relying only on general AI knowledge.

---

### 🌐 Web Search

Requests requiring external or current information can be routed to the web-search workflow.

**Examples:**

```text
"What is the latest AI news?"

"What is the weather in Pune today?"

"Who is the Prime Minister of India?"
```

---

### 🧩 Intent Classification

Gemini classifies every incoming request into a specific intent before routing it.

Current intent categories include:

```text
memory_save
memory_recall
pdf_question
web_search
calendar_create
calendar_read
```

This makes the workflow modular and easier to extend as new capabilities are added.

---

### 🔀 Intelligent Workflow Routing

After classification, Make.com's router sends the request to the correct workflow.

```text
User Input
    ↓
Gemini Intent Classification
    ↓
Router
    ├── Memory Save
    ├── Memory Recall
    ├── PDF Question
    ├── Web Search
    ├── Calendar Create
    └── Calendar Read
```

For example, when the classifier returns `memory_save`, the request is sent to the memory-storage workflow.

---

## 🛠️ Tech Stack

| Technology           | Purpose                                                         |
| -------------------- | --------------------------------------------------------------- |
| **Google Gemini AI** | Intent classification, information extraction and AI processing |
| **Make.com**         | Workflow automation and orchestration                           |
| **Google Sheets**    | Persistent memory storage                                       |
| **Google Calendar**  | Calendar management                                             |
| **Google Drive**     | PDF/knowledge-base storage                                      |
| **Webhooks**         | Receiving user requests and returning responses                 |
| **JSON Parsing**     | Converting AI responses into structured data                    |

---

## ⚙️ How It Works

### Step 1 — Receive User Request

A webhook receives the incoming message.

```text
User → Webhook → Make.com
```

The webhook acts as the entry point for the assistant.

### Step 2 — Classify Intent

Gemini analyzes the user's message and returns exactly one intent.

```text
User:
"Remember that I study at SIT Pune."

Gemini:
memory_save
```

### Step 3 — Route Request

The Make.com router checks the classified intent and sends the request to the corresponding workflow.

### Step 4 — Execute Specialized Workflow

The selected workflow performs the required operation.

For example:

```text
memory_save
     ↓
Extract key + value
     ↓
Parse JSON
     ↓
Store in Google Sheets
     ↓
Return success response
```

### Step 5 — Return Response

The workflow sends a structured response back to the user through the webhook.

---

## 🧠 Memory Architecture

The memory system follows a simple **Key-Value architecture**.

```text
User Message
     ↓
Gemini
     ↓
Memory Extraction
     ↓
JSON Parser
     ↓
Google Sheets
```

Example:

```json
{
  "key": "favorite language",
  "value": "Python"
}
```

The memory workflow then stores the extracted information together with the user identifier.
For recall, the workflow searches the stored Google Sheets data using the user's identifier and retrieves the relevant memory.

---

## 📌 Current Capabilities

| Capability                | Status |
| ------------------------- | ------ |
| 🧠 Save Memory            | ✅      |
| 🔎 Recall Memory          | ✅      |
| 📅 Create Calendar Events | ✅      |
| 📆 Read Calendar Events   | ✅      |
| 📄 PDF Question Answering | ✅      |
| 🌐 Web Search             | ✅      |
| 🔀 Intent Classification  | ✅      |
| 🔗 Webhook Communication  | ✅      |
| 🧩 Workflow Routing       | ✅      |

---

## 💡 Example Interactions

### Memory

**User**

```text
Remember that my favorite programming language is Python.
```

**Assistant**

```text
Memory stored successfully.
```

Later:

**User**

```text
What is my favorite programming language?
```

**Assistant**

```text
Your favorite programming language is Python.
```

---

### Calendar

**User**

```text
Schedule DSA practice tomorrow at 7 PM.
```

The request is classified as:

```text
calendar_create
```

and routed to the calendar workflow.

---

### PDF Knowledge

**User**

```text
Explain temporal locality from my notes.
```

The request is classified as:

```text
pdf_question
```

and routed to the PDF knowledge workflow.

---

### Web Search

**User**

```text
What is the latest AI news?
```

The request is classified as:

```text
web_search
```

and routed to the web-search workflow.

---

## 📂 Project Structure

```text
AI-Personal-Assistant/
│
├── Personal AI Agent.blueprint.json
├── README.md
└── ...
```

The Make.com blueprint contains the automation workflow, including the webhook entry point, Gemini processing, router, memory workflow, JSON parsing and Google Sheets integration.

---

## 🎯 Why This Project?

Traditional chatbots usually focus only on generating responses.

This project focuses on **AI + automation + external services**.

The assistant can:

* Understand what the user wants
* Classify the request
* Select the correct workflow
* Interact with external applications
* Store persistent information
* Retrieve information when needed
* Return an automated response

This makes it closer to an **AI agent workflow** rather than a simple chatbot.

---

## 🔮 Future Enhancements

Planned improvements include:

* 💬 WhatsApp integration
* 📧 Gmail integration
* ⚡ Trigger.dev version
* 👥 Multi-user support
* 🗄️ Database-based memory
* 🧠 Improved long-term memory
* 🔐 User authentication
* 📊 Usage and interaction analytics
* 🎙️ Voice input
* 🖥️ Dedicated web interface

---

## 📈 Future Architecture

```text
                    AI PERSONAL ASSISTANT
                            │
             ┌──────────────┴──────────────┐
             ↓                             ↓
        User Interface                Messaging
             │                       WhatsApp / Gmail
             ↓
       Intent Classifier
             │
             ↓
           Router
             │
    ┌────────┼────────┬─────────┐
    ↓        ↓        ↓         ↓
 Memory   Calendar   PDF      Web
    │        │        │         │
    └────────┴────────┴─────────┘
             ↓
        AI Response
```

---

## 👩‍💻 Author

**Suhani Garg**

Computer Science Engineering Student

### Connect

* GitHub: [@suhanii-1910](https://github.com/suhanii-1910)

---

## ⭐ Project Highlights

> **AI-powered personal assistant combining Gemini AI, workflow automation, persistent memory, and external APIs.**

Built to explore how **LLMs can be combined with automation workflows to create practical AI agents.**

---

⭐ If you find this project interesting, consider giving the repository a star!
