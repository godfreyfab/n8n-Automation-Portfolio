# Telegram AI Logger

An AI-powered Telegram message logger built with n8n, OpenAI, and Google Sheets.

## Features

- Receives Telegram messages
- Preserves sender and timestamp information
- Uses OpenAI GPT-4o-mini to analyze messages
- Classifies messages
- Detects user intent
- Performs sentiment analysis
- Stores structured results in Google Sheets

## Workflow

```text
Telegram Trigger
↓
Edit Fields
↓
HTTP Request (OpenAI)
↓
Code Node
↓
Google Sheets
```

## AI Output

Example:

```json
{
  "classification": "Question",
  "intent": "Inquiry",
  "sentiment": "Neutral"
}
```

## Tech Stack

- n8n
- Telegram Bot API
- OpenAI API
- JavaScript
- Google Sheets

## Versions

### v1
- Basic message classification

### v2
- Added intent analysis
- Added sentiment analysis
- Implemented JSON parsing
- Improved Google Sheets mapping

## Screenshots

(Add screenshots here)

## Lessons Learned

- Prompt engineering
- Structured JSON outputs
- JSON.parse()
- Data mapping in n8n
- Using Code nodes
- Integrating OpenAI with external services

## Future Improvements

- Switch node routing
- Separate sheets for Questions, Feedback, and Spam
- Airtable integration
- Dashboard and analytics
