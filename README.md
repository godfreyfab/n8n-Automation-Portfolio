Telegram AI Logger

An AI-powered Telegram message processing workflow built with n8n, OpenAI, and Google Sheets. The workflow automatically receives Telegram messages, analyzes them using AI, stores all messages in a master log, and routes them into category-specific sheets.

⸻

Project Overview

This project demonstrates how AI can be integrated into an automation workflow to classify incoming Telegram messages, determine user intent, analyze sentiment, and organize the data automatically.

⸻

Features

* Receives Telegram messages automatically
* Preserves original user information
* AI-powered message classification
* Intent detection
* Sentiment analysis
* Stores every message in a Master Log
* Routes messages to category-specific Google Sheets using a Switch node
* Uses structured JSON responses from OpenAI
* Parses AI responses with JavaScript

⸻

Workflow

Telegram Trigger
        ↓
Edit Fields
        ↓
HTTP Request (OpenAI)
        ↓
Code Node
        ↓
Google Sheets (Master Log)
        ↓
Switch
├── Questions Sheet
├── Feedback Sheet
├── Spam Sheet
└── Other Sheet

⸻

AI Response Format

The OpenAI API returns structured JSON: (This is one of my challenges since im not familiar with java script at this time and i just yet to discover json.parse() and used it on my code node

{
  "classification": "Question",
  "intent": "Inquiry",
  "sentiment": "Neutral"
}

This JSON is parsed inside the Code node and mapped to Google Sheets.

⸻

Google Sheets Structure

Master Log

Stores every incoming Telegram message.

Columns:

* Timestamp
* User ID
* Username
* Message
* Chat ID
* Classification
* Intent
* Sentiment

⸻

Category Sheets

Separate sheets for:

* Question
* Feedback
* Spam
* Other

The Switch node routes each message based on its AI classification.

⸻

Technologies Used

* n8n
* Telegram Bot API
* OpenAI API (GPT-4o-mini)
* Google Sheets API
* JavaScript

⸻

Version History

v1

* Telegram message logging
* Google Sheets integration
* Basic message classification

v2

* Added Intent Detection
* Added Sentiment Analysis
* Structured JSON responses
* JSON.parse() implementation
* Improved prompt engineering

v3

* Added Master Log
* Added Switch node routing
* Automatically routes messages to:
    * Question
    * Feedback
    * Spam
    * Other
* Improved Google Sheets mapping
* Learned the difference between automatic and manual field mapping

⸻

Key Concepts Learned

* Prompt Engineering
* OpenAI Chat Completions API
* Structured JSON Output
* response_format
* JSON.parse()
* JavaScript Objects vs Strings
* Code Nodes in n8n
* Switch Node Routing
* Google Sheets Integration
* Manual Field Mapping
* Workflow Debugging

⸻

Future Improvements

* AI Lead Qualification
* CRM Integration
* Slack/Discord Notifications
* Email Notifications
* Dashboard & Analytics
* Airtable Support
* Error Handling Workflow

⸻

Repository Structure

README.md
telegram-ai-logger-v1.json
telegram-ai-logger-v2.json
telegram-ai-logger-v3-routing.json
screenshots/

⸻

Author

Built by Godfrey Fabon as part of a hands-on AI Automation learning portfolio focused on building production-ready workflows with n8n and OpenAI.
