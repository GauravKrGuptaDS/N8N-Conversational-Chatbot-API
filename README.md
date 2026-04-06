# AI Chatbot API using n8n + PostgreSQL

---

## 🚀 Project Overview

This project implements a **multi-session AI Chatbot API** using **n8n**, **LLM Agent**, and **PostgreSQL conversation memory**.

The system allows:

* ✅ API-based chatbot interaction
* ✅ Multi-user conversations
* ✅ Persistent session memory
* ✅ Conversation history storage
* ✅ Scalable AI workflow automation

The chatbot can be integrated with:

* Web Applications
* Mobile Apps
* WhatsApp Bots
* Slack/Teams Bots
* Custom AI Agents

---

## 🏗️ Architecture

```
Client Application
        │
        ▼
   Webhook API (n8n)
        │
        ▼
 Session Manager
        │
        ▼
     AI Agent
        │
        ▼
PostgreSQL Memory Store
        │
        ▼
 Respond to Webhook (API Response)
```

---

## ⚙️ Tech Stack

| Component           | Technology               |
| ------------------- | ------------------------ |
| Workflow Automation | n8n                      |
| AI Model            | OpenAI / LLM             |
| Backend API         | n8n Webhook              |
| Database            | PostgreSQL               |
| Language            | JavaScript               |
| API Testing         | Thunder Client / Postman |
| Containerization    | Docker (Optional)        |

---

## 📂 Database Schema

### 1️⃣ chat_sessions Table

```sql
CREATE TABLE chat_sessions (
    session_id TEXT PRIMARY KEY,
    conversation JSONB,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
```

---

### Conversation Structure

```json
[
  {
    "turn": 1,
    "user": "Hello",
    "assistant": "Hi! How can I help you?",
    "timestamp": "2026-04-06T10:00:00"
  }
]
```

---

## 🔗 API Endpoint

### POST — Chat API

```
POST /webhook/chat-api
```

---

### Request Body

```json
{
  "sessionId": "session_12345",
  "chatInput": "Where is Ranchi located?"
}
```

---

### Response

```json
{
  "sessionId": "session_12345",
  "response": "Ranchi is the capital city of Jharkhand, India."
}
```

---

## 🧠 Session Management Logic

The system automatically:

* Creates a new session if none exists
* Continues existing conversation
* Supports manual reset using:

```
__new_session__
```

---

## 🧩 n8n Workflow Nodes

| Node                        | Purpose             |
| --------------------------- | ------------------- |
| Webhook                     | API Entry Point     |
| Session Manager (Code Node) | Manage session IDs  |
| AI Agent                    | Generate responses  |
| Postgres Upsert             | Store conversation  |
| Respond to Webhook          | Return API response |

---

## 💻 Session Manager Code

```javascript
const body = $input.first().json.body;

const incoming = body.sessionId || '';
const message = (body.chatInput || '').trim();

function generateSessionId() {
  const random = Math.random().toString(36).slice(2, 8);
  return `session_${Date.now()}_${random}`;
}

const sessionId =
  message === '__new_session__' || !incoming
    ? generateSessionId()
    : incoming;

return [{
  json: { sessionId, chatInput: message }
}];
```

---

## 🗄️ Conversation Storage (Upsert Query)

```sql
INSERT INTO chat_sessions (session_id, conversation)
VALUES (...)
ON CONFLICT (session_id)
DO UPDATE SET ...
```

Stores complete chat history using PostgreSQL JSONB.

---

## ▶️ How to Run

### 1️⃣ Start n8n

```bash
docker run -it --rm \
-p 5678:5678 \
n8nio/n8n
```

---

### 2️⃣ Configure

* Import workflow
* Configure AI credentials
* Connect PostgreSQL database
* Activate workflow

---

### 3️⃣ Test API

```bash
curl -X POST http://localhost:5678/webhook/chat-api \
-H "Content-Type: application/json" \
-d '{"chatInput":"Hello"}'
```

---

## 🔐 Recommended Production Improvements

* API Key Authentication
* Rate Limiting
* Session Expiry
* User Identification
* Streaming Responses
* Logging & Monitoring

---

## 🌟 Features

* Multi-session chatbot
* Persistent memory
* REST API integration
* Database-backed conversations
* Scalable architecture
* Low-code AI automation

---

## 📈 Future Enhancements

* OpenAI Compatible API (`/v1/chat/completions`)
* Vector Database Memory
* RAG Integration
* Multi-Agent System
* Cost Tracking per User

---

## 👨‍💻 Author

**Gaurav Kumar Gupta**

AI Consultant | AI Architect | Automation Specialist

---


