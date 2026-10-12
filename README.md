# Zoho CRM Automations & Integrations

Production automations I built as the CRM & automation person at **Way2Go**, a study-abroad consultancy running three service lines on Zoho CRM. They replaced manual spreadsheet work with real-time data flows and gave the sales team reports they could act on.

> Shared with my employer's permission. Internal IDs, URLs and personal data have been removed or replaced with placeholders. Code comments and log messages are in Turkish (the team's working language); each folder's README explains the logic in English.

## Projects

| # | Project | What it does | Stack |
|---|---------|--------------|-------|
| 01 | [Real-time exam form → CRM](01-realtime-exam-form-to-crm) | Website mock-exam submissions land in the CRM instantly: deduplicated, assigned to a rep, rep notified by email | Webhook · Zoho Flow · Deluge · CRM REST API |
| 02 | [Zoom Webinar registrations → CRM](02-zoom-webinar-to-crm) | Serverless gateway verifies Zoom's webhook signature and forwards registrations; Deluge creates or enriches the lead | Zoho Catalyst (Node.js) · Zoho Flow · Deluge |
| 03 | [Contact ↔ Lead matching](03-contact-lead-matching) | Scans up to 100k records and flags leads whose phone number already belongs to an enrolled student or parent | Deluge · CRM REST API v8 · pagination |
| 04 | [Call & lead analytics (SQL)](04-call-and-lead-analytics-sql) | Joins PBX call logs with CRM data: missed calls, callbacks, lead sources per rep | SQL · Zoho Analytics |

Instagram / Meta lead forms are also routed into the CRM in real time through Zoho Flow (no custom code, so not included here).

## Impact

- **Manual → real-time:** leads used to arrive through a ~20-minute CSV → Excel → import routine. They now reach the right rep the moment a form is submitted — **30–100 new leads per day** depending on ad budget and enrollment season.
- **Fewer duplicates:** every integration searches the CRM by email and by several phone formats before creating a record.
- **Visibility:** sales management can see unanswered calls that were never returned, and which channel brings leads to which rep.

## Recurring design decisions

- **Normalize before matching.** Turkish phone numbers arrive as `0532…`, `90532…`, `+90 532…`, `0090532…`. All projects reduce them to a 10-digit form before comparing.
- **Update, don't overwrite.** When a lead already exists, only empty fields are filled; owner, source and status are preserved.
- **Secrets out of code.** Tokens and webhook URLs live in environment variables / Zoho connections.
- **Fail loudly in logs.** Each API call has its own `try/catch` and an `info` / `console.log` trail so problems can be traced in Flow history or Catalyst logs.

## Author

**Nehir Gürsoy**: Information Systems student at VILNIUS TECH, CRM & automation.
