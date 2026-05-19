---
type: reference
project: statement-redesign
status: living
created: 2026-04-07
last_updated: 2026-05-15
ticket_system: jira
purpose: Brief mapping of pod work → engineering tickets. The ticket system is the source of truth for status; this is a navigation aid.
---

# Patient Statements Redesign — Engineering Stories

> Brief reference. JIRA epic [STMT-100](https://example.atlassian.net/browse/STMT-100) is the source of truth for current status, acceptance criteria, and implementation details. This file is a fast cross-reference between pod-vault context and the JIRA stories.
>
> *(File framing is tool-neutral via the `ticket_system:` frontmatter field — set to `jira` here. SKED pods set it to `gitlab` or `jira+gitlab`; the ingest skill applies the appropriate lifecycle rule.)*

## Epic

[STMT-100 — Patient Statements Redesign V1](https://example.atlassian.net/browse/STMT-100) — owns all stories below

## Stories (5)

### STMT-101 — StatementBalanceCalculator service

**Status:** Closed (Apr 17)
**Phase:** 1 (Recalc Service)
**Decision context:** [[2026-04-08 Statement balance source — recalc on render vs cached]]
**Meeting context:** [[2026-04-13 Statement Balance Bug Deep Dive]]

Service that computes current statement balance live from `ledger_entries` instead of reading `patient_account.cached_balance`. Bounded query — single patient, indexed on `(patient_id, posted_at)`. Used by the statement render endpoint; `cached_balance` is no longer read for statements but is retained for backward compat.

### STMT-103 — Statement layout V2 (PDF template)

**Status:** Closed (Apr 24)
**Phase:** 2 (Layout + PDF Pipeline)
**Decision context:** [[2026-04-21 Statement PDF generation — server-side via existing report engine]]
**Meeting context:** [[2026-04-20 Delivery Mechanism Workshop]]

New PDF template registered with the existing report engine. Three-column body (charges / payments / credits), single "amount due" footer, practice header from Statement Settings. Reuses the engine's font + page-break + branding subsystems — no client-side PDF generation.

### STMT-105 — Patient portal preview (embedded PDF)

**Status:** Closed (May 4)
**Phase:** 2 (Layout + PDF Pipeline)
**Decision context:** [[2026-04-28 Patient portal preview — read-only embedded PDF]]
**Meeting context:** [[2026-04-27 Dr. C Sync — Statement UX Direction]]

Portal patients see the same rendered PDF the staff sees. Read-only embed in the existing portal viewer iframe. No separate HTML render — single source of truth. Cuts the design surface in half.

### STMT-106 — Send-from-profile modal + workflow

**Status:** Closed (May 8)
**Phase:** 3 (Send-from-Profile + Branded Email)
**Decision context:** [[2026-04-14 Email vs SMS delivery — email-first for V1]]
**Meeting context:** [[2026-04-20 Delivery Mechanism Workshop]]

"Send Statement" action on Patient Profile → Billing tab. Modal: date range, delivery method (email only V1), render preview, send. Permission gated by existing billing-staff role — no new permission. Sent statements land on the patient timeline via `StatementSent` ([STMT-108](https://example.atlassian.net/browse/STMT-108)).

### STMT-107 — Branded email template + practice-logo config

**Status:** Closed (May 11)
**Phase:** 3 (Send-from-Profile + Branded Email)
**Decision context:** [[2026-05-05 Email template — branded vs plain]]

Email body with practice logo header (from Office Configuration → Statement Settings), practice address + phone footer, configurable one-line note from the practice, PDF statement attached. Delivered via the existing outbound email service. Statement Settings panel ([STMT-109](https://example.atlassian.net/browse/STMT-109)) lets each practice configure the note text + logo.

## Supporting Tasks (not user-facing stories)

| Ticket | Title | Status |
|--------|-------|--------|
| [STMT-102](https://example.atlassian.net/browse/STMT-102) | Wire `StatementBalanceCalculator` into render endpoint | Closed Apr 22 |
| [STMT-104](https://example.atlassian.net/browse/STMT-104) | Register statement template with report engine | Closed Apr 28 |
| [STMT-108](https://example.atlassian.net/browse/STMT-108) | `StatementSent` timeline event | Closed May 13 |
| [STMT-109](https://example.atlassian.net/browse/STMT-109) | Statement Settings panel (Office Config) | Closed May 14 |

## Backlog (Post-V1)

| Ticket | Title | Notes |
|--------|-------|-------|
| STMT-201 | SMS delivery for statements | Deferred from V1 per [[2026-04-14 Email vs SMS delivery — email-first for V1]] |
| STMT-202 | Patient-self-serve statement download from portal | Deferred from V1 — scope decision |
| STMT-203 | Family/multi-patient statement | Q3 candidate |
| STMT-204 | Drop `patient_account.cached_balance` column | After 2 full release cycles with no rollback need |
