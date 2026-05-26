# Work Tracker

A personal productivity tool that turns natural language notes into structured tasks — no forms, no manual data entry.

**Live demo:** [jolefran.github.io/work-tracker](https://jolefran.github.io/work-tracker)

---

## What it does

After a meeting or conversation, you type your notes in plain English:

> *"Ping Jeffrey on the priority score story. Waiting on Bhavana to send updated API doc by Thursday. Write NSS/CDS meeting summary."*

The tool parses that into structured tasks, infers owners, due dates, projects, and statuses — then lets you review and edit before writing anything to the database.

It also sends automated email digests so nothing falls through the cracks:
- **8am daily** — what to act on today, who you're waiting on, what's coming up
- **3pm Friday** — next week's tasks, blocked items, and a nudge for anything that's been sitting undated for 3+ days

---

## Tech stack

| Layer | Tool |
|---|---|
| Frontend | Plain HTML/CSS/JS, hosted on GitHub Pages |
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
- **Bulk status updates** — select multiple tasks and update them in one click
- **Automated email digests** — time-triggered via Apps Script, delivered to Outlook
- **Dark mode** — toggle persists via localStorage

---

## How it works

```
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

---

## Project structure

```
work-tracker/
├── index.html      # Full frontend — UI, styles, and JS in one file
├── build-log.md    # Design decisions, architecture notes, session log
└── README.md       # This file
```

The Apps Script backend lives separately inside Google Drive, bound to the Google Sheet.
