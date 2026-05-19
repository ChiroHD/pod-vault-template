---
type: decision
date: 2026-05-05
status: accepted
project: statement-redesign
decided_by:
  - Riley Chen
  - Cristina Esposito
source: 2026-04-27 Dr. C Sync — Statement UX Direction
tags:
  - email
  - branding
  - ux
  - configuration
---

## Decision

The statement email template is **branded per practice**, not plain. The email body includes:

- Practice logo (uploaded in Office Configuration → Statement Settings)
- Practice address and phone in the footer
- A one-line configurable note from the practice (e.g., "Call us with any questions — thanks for letting us serve you.")
- PDF statement as attachment

Plain-text email is supported as a fallback for clients that don't render HTML, but the rendered HTML version is the default. No "ChiroHD" or "Powered by" branding appears anywhere in the email body — the practice is the only brand the patient sees.

## Context

After the layout direction was approved 4/27, Riley raised the question of email-body design. Two options were on the table:

- **Plain:** Minimal HTML, no logo, body says "Your statement from [Practice Name] is attached." Same template for every practice.
- **Branded:** Practice logo header, practice contact info, configurable note, PDF attached.

Riley initially leaned plain — faster to ship, no per-practice setup, lower email-deliverability risk (HTML emails with attached PDFs hit more spam-filter heuristics). Dr. C, when shown both options on 5/5:

> "The patient relationship is with the practice, not us. If it doesn't look like the practice sent it, half the patients will think it's a phishing email and delete it. Branded."

This is consistent with Dr. C's broader pattern (see [[Stakeholder Communication — Dr. C]]): she treats the practice as the customer; ChiroHD's job is to make the practice look good to the patient, not to be visible itself.

## Rationale

- **Patient trust signal.** Practices know their patients. The practice's logo + contact info in the email body is the trust signal that says "this is from your chiropractor's office, not random spam."
- **Consistent with the rest of ChiroHD's patient-facing comms.** Appointment reminder emails are already branded per practice. Statement email looking different would be jarring.
- **Per-practice config already exists.** Practice logos and address fields already live in Office Configuration for other features (appointment reminders, the practice header on clinical notes). The same data sources back the statement email — no new data plumbing.
- **Configurable note covers practice voice.** Some practices want a warm/casual note ("Thanks for being a part of our family!"), some want a formal note ("Please contact our billing department with any questions"). One configurable field handles both.

## Consequences

- [STMT-107](https://example.atlassian.net/browse/STMT-107) ships as branded HTML email with PDF attachment, plain-text fallback.
- Statement Settings panel ([STMT-109](https://example.atlassian.net/browse/STMT-109)) gets a "Default Note" text field — practice configures their preferred one-liner. Default value: empty (no note); the email looks fine without it.
- Deliverability risk addressed via SPF/DKIM check per beta practice domain (see [[Email Delivery Architecture]]).
- A future "Email Customization" feature could expand the configurable surface (custom subject line, multiple template options). Out of scope for V1.

## Alternatives Considered

| Option | Why Rejected |
|--------|--------------|
| Plain (minimal HTML, no branding) | Looks like spam from the patient's POV. Misses Dr. C's "practice is the customer" framing. |
| Co-branded (practice logo + small "Powered by ChiroHD") | Dr. C's stance is firm — patient relationship is with the practice, not the platform. No co-branding. |
| Fully configurable HTML template per practice | Too much surface for V1. Reserve for a later feature once patterns emerge from beta. |

## History

- **2026-04-27:** Initial Dr. C sync flagged email design as still-open. See [[2026-04-27 Dr. C Sync — Statement UX Direction]].
- **2026-05-05:** Decision locked when Riley showed both options in 1:1 with Dr. C.
- **2026-05-11:** [STMT-107](https://example.atlassian.net/browse/STMT-107) shipped.
- **2026-05-14:** [STMT-109](https://example.atlassian.net/browse/STMT-109) (Statement Settings panel) shipped with the configurable note field.
