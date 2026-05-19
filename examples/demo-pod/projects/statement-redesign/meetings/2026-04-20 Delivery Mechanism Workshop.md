---
type: meeting-note
date: 2026-04-20
attendees:
  - Riley Chen
  - Sam Patel
  - Cristina Esposito
duration: ~65 min
source: fathom-transcript
project: statement-redesign
tags:
  - delivery
  - email
  - sms
  - scope
  - pdf
---

## Summary

Workshop session to settle two related questions: (1) email vs SMS delivery for the send-from-profile workflow, and (2) server-side vs client-side PDF generation. Riley framed both. Sam walked through the implementation surface for each. Dr. C joined the back end of the meeting and aligned with the pod on email-first / SMS-deferred. The PDF question was resolved separately on 4/21 after Sam ran a one-day spike — recorded in a follow-up decision the next day.

## Key Decisions

1. **Email-first delivery for V1. SMS deferred to V2.** See [[2026-04-14 Email vs SMS delivery — email-first for V1]] (decision recorded post-meeting after Dr. C's "don't conflate" framing).
2. **PDF generation method deferred to Sam's 4/21 spike.** Sam to evaluate three options (existing report engine vs client-side vs new server-side library) and decide. (Resolved 4/21 — existing report engine. See [[2026-04-21 Statement PDF generation — server-side via existing report engine]].)

## Key Discussion Points

### Email Implementation Surface

Sam walked the existing outbound email service. Patient-engagement reminders already use it; we have:

- Rendered PDF as attachment — already supported
- Per-practice sender identity (configured via Office Configuration) — already supported
- Audit trail of sent emails on the patient timeline — pattern reusable for `StatementSent`
- Plain-text fallback for HTML-poor clients — already built

Net: email implementation for V1 is mostly composition of existing pieces. No new external dependency. No new failure mode.

### SMS Implementation Surface

ChiroHD has SMS infrastructure for appointment reminders. But:

- No payload type for "PDF attachment" — SMS has no notion of file attachments
- The realistic delivery shape is **SMS-with-secure-link** — patient receives a text with a tokenized link to view the statement
- That requires: a new patient-facing landing page, signed URL handling with expiration, opt-in compliance tracking, separate audit trail

Sam's estimate: 3 weeks of net-new work just for the link infrastructure. Doesn't include the statement-side UX.

### Dr. C Joining

Dr. C joined the call ~20 minutes in. Riley summarized where the pod had landed. Dr. C immediately:

> "Ship email first. SMS is a real ask but it's not the same project. Don't conflate."

This matched Riley's lean. Sam asked whether to put SMS as a visible "coming soon" option in the modal or hide it entirely. Dr. C: visible, greyed out, with a tooltip. Sets expectations.

### Server-Side vs Client-Side PDF

Sam previewed the three options without resolving:

- A: Existing report engine (already used for DX reports, Vital Signs CSV, etc.)
- B: New client-side renderer (jsPDF or similar)
- C: New server-side renderer with a different library (Puppeteer or wkhtmltopdf)

Riley pushed for a one-day spike to decide. Sam agreed; would land an answer by 4/21.

Dr. C: "If we already own a PDF pipeline, use it. Don't introduce a second stack. Ryan Caskey would say the same."

This pre-aligned the pod with Ryan Caskey's expected architecture position, which Sam confirmed via Slack DM the next day during the spike.

### Open Questions Going Forward

- Email template design (branded vs plain) — Riley to bring options to the 4/27 Dr. C sync
- Portal preview format (HTML vs PDF) — same 4/27 sync
- PDF library choice — Sam's 4/21 spike

## Action Items

| Owner | Action | Due | Status |
|-------|--------|-----|--------|
| Riley | Write up email-first / SMS-deferred decision record | Apr 21 | Open |
| Sam | Run PDF library spike — single day | Apr 21 | Open |
| Sam | DM Ryan Caskey to confirm "reuse pipeline" stance | Apr 21 | Open |
| Riley | Prep email-template options (branded vs plain) for 4/27 Dr. C sync | Apr 27 | Open |
| Riley | Prep portal-preview format options for 4/27 Dr. C sync | Apr 27 | Open |
| Sam | Add SMS to backlog as STMT-201 | Apr 21 | Open |

## Cross-Refs

- Decisions recorded: [[2026-04-14 Email vs SMS delivery — email-first for V1]], [[2026-04-21 Statement PDF generation — server-side via existing report engine]]
- Next meeting: [[2026-04-27 Dr. C Sync — Statement UX Direction]]
