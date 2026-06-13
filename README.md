## Telegram to Google Sheets Logger

An n8n automation workflow that captures messages from a Telegram bot and writes them directly to Google Sheets.

## Quick Demo

**What happens when someone messages your bot:**

1. User sends "Hello, I need help with my order"
2. n8n receives the message via webhook
3. Google Sheets gets a new row with:
   - Timestamp
   - User ID  
   - Username (or User ID if none exists)
   - The actual message
   - Chat ID

## What This Does

- Listens for incoming messages via Telegram bot webhook
- Extracts message text, user ID, username, chat ID, and timestamp
- Writes each message as a new row in Google Sheets
- **Handles missing usernames gracefully** using fallback logic (shows user ID instead)

## Tech Stack

- **n8n** (self-hosted on Railway)
- **Telegram Bot API**
- **Google Sheets API**

## Workflow Structure

<img width="1508" height="930" alt="Untitled" src="https://github.com/user-attachments/assets/9a5417b4-8b0b-4871-88cb-5731697a104d" />



No IF nodes — just a clean, straight pipe with fallback expressions.

## Key Expression (The "Smart" Part)

```javascript
{{ $json.body.message.from.username || $json.body.message.from.id }}
```
the "||" is a fallback to he display the user id if the user has no username





