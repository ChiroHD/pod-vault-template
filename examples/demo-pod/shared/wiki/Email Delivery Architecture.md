---
type: wiki
topic: Email Delivery Architecture
created: 2026-05-15
updated: 2026-05-15
sources:
  - "projects/statement-redesign/decisions/2026-04-14 Email vs SMS delivery — email-first for V1.md"
  - "projects/statement-redesign/decisions/2026-05-05 Email template — branded vs plain.md"
  - "projects/statement-redesign/meetings/2026-04-20 Delivery Mechanism Workshop.md"
---

# Email Delivery Architecture

How statement delivery integrates with ChiroHD's outbound email infrastructure. The short version: V1 composes pieces that already exist. No new external dependency, no new failure mode.

## What Already Existed

ChiroHD has an outbound email service used by patient-engagement features (appointment reminders, intake form invitations, etc.). The service already handles:

- Per-practice sender identity (configured in Office Configuration; uses each practice's verified sender domain)
- Rendered PDF as attachment
- HTML body + plain-text fallback
- Send audit trail on the patient timeline
- Bounce + delivery-failure handling
- SPF/DKIM compliance (per practice domain)

The statement workflow ([STMT-106](https://example.atlassian.net/browse/STMT-106), [STMT-107](https://example.atlassian.net/browse/STMT-107), [STMT-108](https://example.atlassian.net/browse/STMT-108)) composes these.

## What V1 Adds

Three new pieces:

1. **Statement email template.** Branded HTML body with practice logo + practice contact info + configurable note + PDF statement attached. Lives in the same template registry as appointment reminders. See [[2026-05-05 Email template — branded vs plain]].
2. **`StatementSent` timeline event.** Lands on the patient profile timeline with sent-to address + timestamp. Pattern mirrors the existing `ReminderSent` event.
3. **Statement Settings panel** (CHD Admin → Office Configuration → Statement Settings). Per-practice configuration: logo, default note text, default delivery method, default date range. Reuses the existing Office Configuration UI primitives.

## End-to-End Flow

```
[Staff: Patient Profile → Billing → Send Statement]
         ↓
[Send-from-profile modal]
         ↓ (date range + delivery method + preview)
[Render statement PDF via report engine]
         ↓
[Compose email body from statement-email template + Statement Settings]
         ↓
[Outbound email service queue]
         ↓
[Existing send infrastructure: SPF/DKIM, bounce handling, audit]
         ↓
[Patient inbox + StatementSent event on timeline]
```

## What's Configurable Per-Practice

Statement Settings panel:

| Field | Default | Notes |
|-------|---------|-------|
| Practice logo | (none) | PNG/JPG, recommended 300×100px. Renders at top of email body. |
| Default note text | empty | Single line. Renders above the PDF attachment reference. |
| Default delivery method | Email | Only option in V1. SMS greyed out (coming in V2). |
| Default statement date range | Last 30 days | Staff can override per-send. |

## Email Body Layout

```
[Practice logo]
[Practice name]                  [Practice phone]

Hi [Patient first name],

[Configurable note text — practice default, single line]

Your statement is attached as a PDF.

— [Practice name]
[Practice address]
[Practice phone]
```

Plain-text fallback drops the logo and renders the same content in plain layout.

## Deliverability Considerations

Each beta practice's domain needs to be set up for SPF/DKIM correctly on the practice side. Sam runs a pre-flight check before beta onboarding:

1. Verify SPF record includes ChiroHD's send-from domain
2. Verify DKIM signing is configured
3. Send a test rendered statement to the practice's billing-admin inbox; confirm it lands in Inbox, not Spam

This is standard for any new email-sending feature in ChiroHD. The check pattern is reusable.

## What's NOT in V1 (Deferred to V2)

- **SMS delivery** — STMT-201. See [[2026-04-14 Email vs SMS delivery — email-first for V1]]. SMS-with-secure-link requires a new patient-facing landing page + signed URL system; that's its own project, not a half-baked add to V1.
- **Patient-self-serve statement download** from the portal — STMT-202. Different access pattern, different auth surface. Separate feature.
- **Custom email subject line per practice** — out of scope. Single subject line: `Statement from [Practice Name]`.
- **Multiple email template options** — out of scope. One branded template is enough variance for V1.

## Sources

- [[2026-04-14 Email vs SMS delivery — email-first for V1]]
- [[2026-05-05 Email template — branded vs plain]]
- [[2026-04-20 Delivery Mechanism Workshop]]

## See Also

- [[PDF Generation Pipeline]] — what gets attached
- [[Statement Glossary]] — `StatementSent` event, branded email template, Statement Settings definitions
