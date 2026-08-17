# 🤖 AI Customer Support Router — n8n

An AI-powered customer support automation built with **n8n, OpenAI, Gmail, Google Sheets, and Telegram**.

The workflow automatically analyzes incoming customer emails, determines the appropriate department, assigns priority and sentiment, generates a summary and suggested action, logs the ticket, and alerts the team when an important issue requires attention.

---

## 🎯 Project Goal

Customer support inboxes can contain a mixture of:

- Sales inquiries
- Technical support requests
- Billing concerns
- Customer complaints
- General or unrelated emails

Manually reviewing and routing every message can become time-consuming and may cause important tickets to be missed.

This workflow automates the initial ticket triage process.

---

## ⚙️ Workflow Architecture

```text
Gmail Trigger
      ↓
OpenAI — Analyze Email
      ↓
Code Node — Parse & Validate Data
      ↓
Switch — Qualify Department
      ↓
 ┌───────────┬───────────┬───────────┬─────────────┬───────────┐
 ↓           ↓           ↓           ↓             ↓
Sales      Support     Billing    Complaint       Other
 ↓           ↓           ↓           ↓             ↓
Google     Google      Google      Google         Google
Sheets     Sheets      Sheets      Sheets         Sheets
 ↓           ↓           ↓           ↓             ↓
Priority    Priority    Priority    Priority       Priority
Check       Check       Check       Check          Check
 ↓           ↓           ↓           ↓             ↓
High/Urgent → Telegram Notification

----------------------
AI Analysis

Every incoming email is analyzed by OpenAI and converted into structured data.
Example:
{
  "department": "Billing",
  "priority": "High",
  "sentiment": "Negative",
  "summary": "Customer reports a duplicate subscription charge.",
  "suggested_action": "Review the transaction and resolve the duplicate charge."
}

The workflow classifies each email using three main dimensions.

Department

Possible departments:

* Sales
* Support
* Billing
* Complaint
* Other

The classifier considers the customer’s primary problem and requested resolution, rather than relying only on keywords.
For Example:
"I was charged twice."
→ Billing

"My subscription renewed successfully,
but I can't access the paid features."
→ Support

Although the second message mentions payment, the actual problem requiring resolution is product access.

Promotional emails, advertisements, newsletters, and unsolicited vendor offers are classified as Other rather than Sales.
----------------------
🚨 Priority Classification

Tickets are assigned one of four priority levels:

1.Urgent

Immediate risk, fraud/security, major outage, repeated unresolved issue, or cancellation threat

2.High

Payment/refund issue, account access problem, or serious service issue

3.Medium

Standard support or sales inquiry

4.Low

Informational, promotional, or non-urgent message

Only: 
High OR Urgent tickets generate a Telegram notification

Medium and Low tickets are still logged but do not create unnecessary alerts.
-----------------
Sentiment Analysis

The workflow also determines customer sentiment:

* Positive
* Neutral
* Negative

A customer does not need to use angry language for a message to be classified as Negative.

For example:

"I was charged twice. Could you please help me get this corrected?"

→ Negative

The message is polite, but the customer is still reporting a negative experience.
----------------
🧹 Data Parsing & Validation

A JavaScript Code node processes the AI response before routing.

It handles:

* JSON parsing
* Markdown code-fence removal
* Department validation
* Priority validation
* Sentiment validation
* Fallback values
* Gmail metadata extraction
* Standardized output generation

Example standardized output:
{
  "timestamp": "2026-08-16T10:00:00.000Z",
  "sender": "customer@example.com",
  "subject": "Duplicate Charge",
  "message": "I noticed I was charged twice...",
  "department": "Billing",
  "priority": "High",
  "sentiment": "Negative",
  "summary": "Customer reports a duplicate charge.",
  "suggested_action": "Review the transaction and resolve the duplicate payment.",
  "status": "New Ticket"
}

The Code node also protects the workflow against malformed AI output by validating values before they reach the routing logic.
---------------
Ticket Logging

After classification, tickets are routed to their corresponding Google Sheets department:
Sales      → Sales Sheet
Support    → Support Sheet
Billing    → Billing Sheet
Complaint  → Complaint Sheet
Other      → Other Sheet

Logged information includes:

* Timestamp
* Sender
* Subject
* Original message
* Department
* Priority
* Sentiment
* Summary
* Suggested action
* Ticket status
-----------
🔔 Priority Escalation

After the ticket is logged, an IF node evaluates its priority.
Priority = High
OR
Priority = Urgent

TRUE

A Telegram alert is sent containing:

* Department
* Priority
* Sentiment
* Sender
* Subject
* Summary
* Suggested action

FALSE

The ticket remains logged without generating an alert.

This prevents the team from being notified about every incoming email while ensuring important issues receive attention.
-----
🧪 Testing

The workflow was tested with both straightforward and ambiguous customer messages.

Basic Classification Tests

Successfully tested:

* Sales
* Support
* Billing
* Complaint
* Other

Edge Cases

Additional tests included:

* Support vs Complaint
* Billing vs Complaint
* Billing vs Support
* Sales vs Other
* Promotional Email vs Sales

Regression testing was also performed after modifying classification rules to ensure previously working cases continued functioning correctly.
---------
🐛 Problems Solved During Development

Markdown-Wrapped JSON

The AI occasionally returned:
```json
{
  "department": "Support"
}
This caused `JSON.parse()` to fail.

The Code node was updated to strip Markdown code fences before parsing the response.

### Billing vs Support

Messages mentioning subscriptions or payments were sometimes classified as Billing even when the actual problem was technical access.

The classifier was improved to prioritize:

```text
Primary Problem + Requested Resolution

instead of simply detecting payment-related keywords
-----
Support vs Complaint

Support requests and complaints can overlap.

The classifier distinguishes between:
Support
→ Customer primarily wants help fixing a problem.

Complaint
→ Customer primarily expresses dissatisfaction with how
   the company handled the problem.
----
Promotional Emails Classified as Sales

Promotional emails were initially interpreted as Sales because they contained sales language.

The classifier was updated so that:
Inbound prospective customer → Sales

Advertisement / Newsletter / Vendor Promotion → Other
------

🛠️ Technologies Used

* n8n — Workflow automation
* OpenAI — AI classification and analysis
* Gmail — Incoming email trigger
* Google Sheets — Ticket logging
* Telegram — Priority notifications
* JavaScript — AI response parsing and validation
------
💡 Skills Demonstrated

This project demonstrates:

* AI workflow design
* n8n workflow development
* Prompt engineering
* OpenAI integration
* Structured AI output
* JavaScript Code nodes
* JSON parsing
* Error handling
* Data validation
* Conditional logic
* Switch-based routing
* Gmail automation
* Google Sheets integration
* Telegram notifications
* AI classification testing
* Edge-case testing
* Regression testing
* Workflow debugging
----

🔐 Security

The workflow included in this repository is a sanitized portfolio version.

Sensitive configuration such as:

* API credentials
* OAuth credentials
* Telegram Chat IDs
* Google Sheet IDs
* Webhook identifiers
* n8n instance identifiers

has been removed or replaced with placeholders.

Users importing the workflow must configure their own credentials and service IDs.
------

🚀 Possible Future Improvements

Potential V2 improvements include:

* Automatic draft responses
* Human approval before sending responses
* SLA tracking
* Ticket IDs
* CRM integration
* Confidence scoring
* Manual review for uncertain classifications
* Automatic follow-up reminders
* Support platform integration
* Analytics dashboards

These features were intentionally excluded from V1 to keep the workflow focused on reliable classification, routing, logging, and escalation.
