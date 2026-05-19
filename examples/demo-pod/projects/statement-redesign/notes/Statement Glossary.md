---
type: reference
date: 2026-04-10
project: statement-redesign
purpose: Standing reference — terms specific to the patient statement domain in ChiroHD
tags:
  - type/reference
  - project/statement-redesign
---

# Statement Glossary

Standing reference for the Statement Redesign project. Terms specific to the patient-statement domain in ChiroHD, defined once here so we don't re-litigate them in every meeting.

## Patient-Facing Terms (what the patient sees on the statement)

| Term | Definition |
|------|-----------|
| **Statement** | A patient-facing document summarizing the charges, payments, credits, and resulting balance for a date range. Delivered as PDF (via email, portal preview, or download). |
| **Amount Due** | The bolded footer value the patient is responsible for paying. Computed live from the ledger via [[Statement Balance Recalc Pattern]]. The single most important number on the statement. |
| **Statement Date Range** | The window of charges + payments + credits the statement covers. Defaults to "last 30 days" but staff can adjust per-send via the send-from-profile modal. |
| **Charge** | A billable service rendered to the patient. Includes the standard rate before any write-offs. |
| **Payment** | Money the patient has paid. Includes cash, card, check, and ACH. Insurance payments are tracked separately and don't appear here in V1. |
| **Credit** | A reduction of the patient's responsibility. Includes write-offs (e.g., prepay discount, professional courtesy) and applied prepayments from a care package. |
| **Practice Header** | The top-of-statement block with practice name, address, phone, and (if configured) logo. Sourced from Office Configuration → Statement Settings. |

## Internal / Staff-Facing Terms (used by the pod and engineering)

| Term | Definition |
|------|-----------|
| **Ledger Entry** | A single row in the `ledger_entries` table. Each charge, payment, write-off, and credit is one entry. The ledger is the source of truth for everything financial about a patient. |
| **Cached Balance** | The `patient_account.cached_balance` column. Historically the source for statement totals. **No longer used by statements** as of V1 — drifted when ledger writes failed silently. Still used by case summary cards, dashboard tiles, etc. See [[2026-04-08 Statement balance source — recalc on render vs cached]]. |
| **`StatementBalanceCalculator`** | The new service introduced in V1 that computes statement totals live from `ledger_entries` at render time. Implementation lives in the statement-render code path; not used by other features. |
| **Send-from-Profile** | The new workflow added in V1. Staff click "Send Statement" on Patient Profile → Billing tab. Modal opens with date range + delivery method + render preview + send. Replaces "download PDF → switch to email client → attach → send." |
| **Statement Settings** | New panel in CHD Admin → Office Configuration. Per-practice: logo, default note text, default delivery method, default date range. |
| **`StatementSent` event** | Timeline event recorded when staff send a statement. Lands on the patient profile timeline with sent-to address + timestamp. Audit trail for billing staff. |
| **Branded Email Template** | The HTML email body with practice logo + practice contact info + configurable note + PDF attachment. Decision context: [[2026-05-05 Email template — branded vs plain]]. |

## Architectural Terms

| Term | Definition |
|------|-----------|
| **Recalc on Render** | The architectural pattern adopted for V1: balance is computed live each time a statement is rendered, not read from a cached column. See [[Statement Balance Recalc Pattern]]. |
| **Report Engine** | ChiroHD's existing server-side PDF rendering pipeline. Used by DX reports, Well Health summaries, Vital Signs CSV, and now statements. Handles fonts, layout, page breaks, branding. See [[PDF Generation Pipeline]]. |
| **Portal Preview** | The patient-portal view of a statement. V1: read-only embedded PDF — same file the patient receives by email. See [[Patient Portal Preview UX]]. |
| **`statementRedesign` Feature Flag** | The flag gating the V1 release. On for the beta cohort; off for everyone else until broad release. Standard ChiroHD beta-scoping pattern. |

## Deferred-to-V2 Terms (mentioned in scope discussions but not in V1)

| Term | Definition |
|------|-----------|
| **SMS Delivery** | Statement delivery via SMS with a secure tokenized link. Deferred to V2. Tracked as STMT-201. See [[2026-04-14 Email vs SMS delivery — email-first for V1]]. |
| **Patient-Self-Serve Statement Download** | Patient initiates statement generation from the portal (vs staff initiating via send-from-profile). Deferred. STMT-202. |
| **Family / Multi-Patient Statement** | A single statement covering multiple patients in the same family. Deferred. STMT-203. |
| **Insurance Itemization** | Statement breaks out per-claim insurance contributions, write-offs by payer, etc. Out of scope for V1 entirely. |

## Notes

This glossary is small on purpose. It captures terms that have been a source of confusion or that map cleanly to project artifacts. If a term shows up in a meeting more than once and someone asks "what do we mean by that exactly?", it belongs here.

If you find a term being used inconsistently across notes/meetings/decisions, add it here and link from the inconsistent uses.
