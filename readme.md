  # AI Lead Qualification & Routing Automation

## 📌 Overview

This project is an AI-powered lead qualification workflow built with **n8n** and **OpenAI**.

The workflow receives customer inquiries via a webhook, uses AI to extract important lead information, classifies the lead quality, and automatically routes the lead to the appropriate follow-up action.

This project demonstrates how AI can automate lead intake, reduce manual work, and help businesses respond faster to potential customers.

---

## 🚀 Features

- Receive leads through a Webhook
- Extract customer information using OpenAI
- Automatically identify:
  - Name
  - Email
  - Phone Number
  - Requested Service
  - Budget
  - Urgency
- AI Lead Scoring
  - 🔥 Hot
  - 🟡 Warm
  - ❄️ Cold
- Store qualified leads in Google Sheets
- Automatically notify the sales team for Hot leads via Telegram
- Automatically send follow-up emails for Warm leads using Gmail
- Store Cold leads for future nurturing

---

## 🏗 Workflow Architecture

```text
Webhook
    ↓
OpenAI
    ↓
Code (Parse AI JSON)
    ↓
Switch

├── Hot
│     ↓
│   Google Sheets
│     ↓
│   Telegram Notification
│
├── Warm
│     ↓
│   Google Sheets
│     ↓
│   Gmail Follow-up
│
└── Cold
      ↓
   Google Sheets
```

---

## 🛠 Tech Stack

- n8n
- OpenAI API
- Google Sheets API
- Gmail API
- Telegram Bot API
- JavaScript (Code Node)

---

## 📊 AI Output Example

Input:

```
Hi, I'm Sarah.

Email: sarah@gmail.com

Phone: 09171234567

I need an AI chatbot for my dental clinic.

Budget is $5,000.

Need it next week.
```

AI Output:

```json
{
  "name": "Sarah",
  "email": "sarah@gmail.com",
  "phone": "09171234567",
  "service": "AI chatbot for dental clinic",
  "budget": "$5,000",
  "urgency": "Next Week",
  "lead_quality": "Hot",
  "reason": "Customer provided contact information, budget, urgency, and clear buying intent."
}
```

---

## 🔄 Workflow Process

### 1. Receive Lead

A customer inquiry is received through an n8n Webhook.

---

### 2. AI Lead Qualification

OpenAI analyzes the message and extracts structured lead information.

---

### 3. Parse Response

A JavaScript Code node converts the AI response into structured JSON.

---

### 4. Lead Routing

The Switch node routes leads according to their qualification.

### 🔥 Hot Lead

- Store in Google Sheets
- Notify sales team via Telegram

### 🟡 Warm Lead

- Store in Google Sheets
- Send automated follow-up email

### ❄️ Cold Lead

- Store in Google Sheets for future nurturing

---

## 💼 Business Value

This automation helps businesses:

- Reduce manual lead qualification
- Respond to leads faster
- Improve sales response times
- Keep lead data organized
- Ensure no inquiry is missed

---

## 📚 Skills Demonstrated

- AI Workflow Automation
- Prompt Engineering
- OpenAI API Integration
- Google Sheets Integration
- Gmail Automation
- Telegram Automation
- Conditional Routing
- JSON Parsing
- JavaScript
- Webhooks
- API Authentication
- Business Process Automation

---

## 🔮 Possible Future Improvements

- CRM Integration (HubSpot, Salesforce)
- Lead Scoring (0–100)
- Slack Notifications
- Automatic Calendar Booking
- Follow-up Email Sequences
- Database Storage (PostgreSQL)
- Dashboard Analytics
- Multi-language Support

---

## 👨‍💻 Author

Godfrey Fabon

AI Automation Portfolio

Built using n8n and OpenAI.
