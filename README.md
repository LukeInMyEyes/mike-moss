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

## Setup

1. Create Telegram group and add brokers
2. Create bot via @BotFather, get token
3. Import `n8n-workflow.json` into n8n cloud
4. Set credentials: Telegram token, Google Sheets OAuth
5. Set webhook URL in n8n Telegram trigger node
6. Create Google Sheet using column schema above
7. Share sheet with the n8n Google service account
