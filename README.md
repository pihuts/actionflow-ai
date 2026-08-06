# ActionFlow AI

An n8n workflow that turns a meeting transcript into a summary, decisions, risks, and tracked action items.

![ActionFlow AI workflow](screenshots/ActionFlow%20AI.png)

## What it does

1. Receives a transcript via webhook.
2. GPT-5.6 Terra summarizes the meeting.
3. The AI extracts key decisions, risks, and action items with owners, due dates, and priorities.
4. The meeting summary is logged to Google Sheets.
5. Every action item becomes its own tracked row.
6. The team receives an email with the summary and action items.

## Current tech

- n8n webhooks + AI Agent (LangChain) nodes
- OpenAI GPT-5.6 Terra with structured JSON output
- Google Sheets relational logging (meeting row + action item rows)
- Gmail team digest

## Setup

1. Import `ActionFlow AI.json`.
2. Set environment variables:

   | Variable | Purpose |
   |---|---|
   | `MEETING_SHEET_ID` | Google Sheet ID |
   | `MEETING_EMAIL_TO` | Team inbox for the digest |

3. Connect OpenAI, Google Sheets, and Gmail credentials.
4. Create two tabs:
   - `Meetings`: `Meeting ID, Date, Title, Organizer, Attendees, Summary, Key Decisions, Risks, Suggested Follow-up, Received At`
   - `Action Items`: `Meeting ID, Meeting Title, Owner, Task, Due Date, Priority, Status`
5. Activate the workflow.

## Test it

```bash
curl -X POST https://your-n8n.com/webhook/actionflow-meeting-notes \
  -H "Content-Type: application/json" \
  -d '{"transcript":"Alice: we need to ship the dashboard by Friday. Bob: I will own the API work. Decision: launch next week.","meeting_title":"Sprint sync","attendees":["Alice","Bob"]}'
```

