# Mike Moss — Property Lead Management System

Telegram-based lead capture and tracking system for a commercial property brokerage team.

## How It Works

1. Broker posts a new lead or update in the Telegram group
2. AI bot parses the message and creates/updates a row in the Master Google Sheet
3. Leads are automatically assigned to the correct broker by area (North/South/West)
4. Each broker has a filtered weekly contact sheet used at the weekly breakfast meeting
5. Brokers update progress via the group chat — bot keeps records in sync

## Team

| Broker | Area |
|--------|------|
| Nico   | North |
| Mike   | South |
| Dave   | West |

## Lead Intake

Two methods:
- **Telegram group** (primary) — post naturally, bot extracts the data
- **Google Form** (structured intake) — for detailed enquiries after a call

## Google Sheet Structure

### Master Sheet — `Leads`

| Column | Description |
|--------|-------------|
| Lead ID | Auto-generated unique ID |
| Date | Date enquiry was logged |
| Lead Originator | Who generated the lead |
| Assigned Broker | Who is working the lead |
| Area | North / South / West |
| Company Enquiring | Tenant/buyer company name |
| Contact Person | Name |
| Contact Number | Phone |
| Contact Email | Email |
| Lead Source | Prop24 / ToLet Board / Return Business / LinkedIn / Referral / Other |
| Property Type | Warehouse / Office / Retail / Land / Other |
| Size Required (m²) | Required floor area |
| Power (Amps) | Electrical requirement |
| Budget (R/m²) | Monthly rental budget |
| Location Preference | Area or suburb preference |
| Timeline | When they need to move |
| Notes | Free-text updates |
| Status | New / Active / Viewing / Negotiating / Closed / Dead |
| Commission Split | e.g. 50/50 Mike/Nico |
| Last Updated | Timestamp of last change |

### Broker Weekly Sheets

Auto-filtered views per broker showing only their active leads for the week.

## Stack

- **Telegram Bot** — group message capture
- **n8n (cloud)** — automation and AI parsing workflow
- **Claude AI** — message understanding and field extraction
- **Google Sheets** — master lead database + broker weekly sheets
- **Google Forms** — structured lead intake (linked to master sheet)

## n8n Workflow

See `n8n-workflow.json` — import into your n8n cloud instance.

### Workflow Nodes

1. **Telegram Trigger** — webhook on group messages
2. **Filter** — ignore bot's own messages
3. **Claude AI** — classify message (new lead / update / status change / irrelevant) and extract fields
4. **Google Sheets — Search** — look up existing lead by company name or lead ID
5. **Router** — branch on new vs update
6. **Google Sheets — Create / Update** — write the row
7. **Telegram — Reply** — confirm action back to group

## Telegram Setup (DONE)

| Item | Value |
|------|-------|
| Bot name | Rasmussen Leads Bot |
| Bot username | @RasmussenLeads_bot |
| Group | Rasmussen Properties — Leads |
| Group Chat ID | -5177500666 |
| Privacy mode | DISABLED (reads all messages) |
| Bot role in group | Admin |

## Remaining Setup

1. Log into n8n cloud and import `n8n-workflow.json`
2. Add Telegram credential using the bot token (in .env.example)
3. Add Google Sheets OAuth credential
4. Set `GOOGLE_SHEET_ID` once the master sheet is created
5. Activate the workflow — n8n will register the webhook automatically
6. Create Google Sheet using the column schema above
7. Invite Nico, Mike, Dave to the Telegram group
