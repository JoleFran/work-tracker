# Work Tracker

A personal productivity tool that turns natural language notes into structured tasks — no forms, no manual data entry.

**Live demo:** [jolefran.github.io/work-tracker/demo.html](https://jolefran.github.io/work-tracker/demo.html)

> **Note:** The demo uses fictional names and projects. Mobile capture is not available in the demo — it is a feature of the full tool only.

---

## What it does

After a meeting or conversation, you type your notes in plain English:

> *"Ping Jeffrey on the priority score story. Waiting on Bhavana to send updated API doc by Thursday. Write NSS/CDS meeting summary."*

The tool parses that into structured tasks, infers owners, due dates, projects, and statuses — then lets you review and edit before writing anything to the database.

It also sends automated email digests so nothing falls through the cracks:
- **8am daily** — what to act on today, who you're waiting on, what's coming up
- **3pm Friday** — next week's tasks, blocked items, and a nudge for anything that's been sitting undated for 3+ days

On mobile, a lightweight capture page lets you save quick notes to an inbox. When you're back at your desk, inbox items appear in the desktop UI with a one-click interpret button that pulls them into the normal review flow.

---

## Tech stack

| Layer | Tool |
|---|---|
| Desktop frontend | Plain HTML/CSS/JS, hosted on GitHub Pages |
| Mobile capture | Separate lightweight HTML page, same host |
| Backend | Google Apps Script (Web App) |
| Database | Google Sheets |
| AI | Anthropic Claude API (`claude-sonnet-4-6`) |
| Email | Gmail → Outlook via Google MailApp |

No frameworks, no build step, no server costs.

---

## Key features

- **Natural language parsing** — Claude interprets freeform notes into structured task rows
- **Editable preview** — review and correct all AI-proposed tasks before they hit the database
- **Stakeholder inference** — automatically tags tasks to the right project based on who's mentioned
- **Mobile inbox** — capture quick notes on your phone; process them from the desktop when ready
- **Bulk status updates** — select multiple tasks and update them in one click
- **Column sorting** — sort by owner, due date, project, or priority with toggle direction
- **Smart filtering** — completed tasks auto-hide the day after completion; filter by status
- **Automated email digests** — time-triggered via Apps Script, delivered to Outlook
- **Dark mode** — toggle persists via localStorage across all pages

---

## How it works

```
Desktop flow:
You type a note
      ↓
Frontend sends note + today's date to Apps Script
      ↓
Apps Script calls Claude API with your stakeholder map and schema rules
      ↓
Claude returns structured JSON (task, owner, due date, project, status)
      ↓
You review and edit in a preview table
      ↓
Apps Script writes to Google Sheet, stamps timestamps
      ↓
Time-triggered scripts send email digests on schedule

Mobile flow:
You type a quick note on your phone
      ↓
Note saves directly to an Inbox tab in Google Sheet (no AI processing)
      ↓
Desktop UI shows inbox items when you next open it
      ↓
One click loads the note into the interpret flow
```

---

## Design decisions

**Why Google Sheets instead of a database?**
Zero cost, easy to inspect and edit manually, and Apps Script integrates natively — no ORM, no connection strings, no hosting.

**Why Apps Script instead of a hosted backend?**
It lives inside Google's ecosystem for free, has native access to Sheets and Gmail, and eliminates infrastructure to maintain.

**Why call the Claude API server-side?**
Browsers block direct calls to `api.anthropic.com` from third-party origins. Apps Script handles it cleanly without exposing the API key to the client.

**Why plain HTML?**
The goal was a working tool, not a framework exercise. Keeping it dependency-free means it's easy to read, easy to fork, and deploys instantly to GitHub Pages.

**Why zero interpretation on mobile?**
Mobile capture is for fleeting thoughts, not processed meeting notes. Keeping it a raw save removes latency and the need for a review UI on a small screen. The desktop handles interpretation when you're ready.

---

## Project structure

```
work-tracker/
├── index.html       # Desktop UI — full task management interface
├── capture.html     # Mobile UI — lightweight quick-capture page
├── demo.html        # Public demo — fictional data, no mobile support
├── build-log.md     # Design decisions, architecture notes, session log
└── README.md        # This file
```

The Apps Script backend lives separately inside Google Drive, bound to the Google Sheet.
