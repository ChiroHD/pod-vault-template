---
project: Patient Statements Redesign
jira_epic: STMT-100
status: active — Phase 3 polish + beta
pm: Riley Chen
eng_lead: Sam Patel
advisor: Dr. Cristina Esposito
priority: Q2 Accepted
start_date: 2026-04-06
target_date: 2026-06-05
last_updated: 2026-05-15
---

# Patient Statements Redesign

## What We're Building

A focused cleanup of the patient-facing statement experience in CHD Admin. Three things ship together:

1. **Fix the stale-balance bug** — patient statements have shown values that diverge from the live ledger because they read a `cached_balance` column that drifts when ledger writes fail silently. Recalculate on render so a statement always reflects the current state of the patient's ledger.
2. **Redesign the statement layout** — patient-facing PDF and portal-embedded preview share one design. Less noise, clearer charge/payment/credit columns, single bolded "amount due" line at the bottom.
3. **Send-from-profile** — staff can email a statement to the patient directly from the Patient Profile, instead of downloading PDF + composing email outside ChiroHD.

**Out of scope for V1:** SMS delivery (deferred to V2), patient-self-serve statement download from the portal (deferred), insurance claim itemization on the statement, multi-patient family statements.

## Why Now

Statements have been the #3 source of billing-staff support tickets across Q1 2026 (after Greensheets and Fortis split-payment sync). Two recurring complaints:
- "The statement total doesn't match what I see on the ledger" — the stale-balance bug
- "I have to download the PDF, then go to my email, then attach it" — the missing send-from-profile workflow

Dr. C scoped this as a single Q2 pod project to clean up both in one release rather than fighting them as separate tickets.

## Key Links

| Resource | Link |
|----------|------|
| JIRA Epic | [STMT-100](https://example.atlassian.net/browse/STMT-100) |
| Notion PRD (v0.3) | `Product / Q2 2026 / Patient Statements Redesign — PRD` |
| Figma — Statement Layout V2 | `Figma: Statements / V2 Layout` |
| Stakeholder Feedback Page | `Notion: Statement Redesign — Stakeholder Feedback` |

## Stakeholders

| Person | Role | Involvement |
|--------|------|-------------|
| Dr. Cristina Esposito | Director of Product | Approver — UX direction, beta sign-off |
| Riley Chen | PM (Atlas pod) | PM — requirements, beta coordination, stakeholder comms |
| Sam Patel | Lead Engineer (Atlas pod) | Engineer — sole builder, owns architecture + impl |
| Ryan Caskey | Head Developer | Architecture review — recalc pattern, PDF pipeline reuse |
| Maya Chen (Support Lead) | Support Team | Beta clinic liaison; will field beta feedback alongside the pod |
| Beta cohort (3 practices) | External | Pilot users for the 2-week feedback window |

## Scope — V1

**Delivers:**

- Recalc-on-render: statement endpoint computes balance live from `ledger_entries` instead of reading `patient_account.cached_balance`. The cached column stays for backward compat but is no longer the source of truth for statements.
- Redesigned statement PDF + portal preview — single layout, three-column body (charges / payments / credits), clear "amount due" footer.
- Send-from-profile: a "Send Statement" action on the Patient Profile billing tab. Opens a modal with date range, delivery method (email only in V1), and a preview of the rendered statement before send.
- Email template — branded with practice logo, practice address/phone, and a one-line note from the practice (configurable per-practice default in CHD Admin → Office Configuration → Statement Settings).

**Out of scope:** SMS delivery, patient-self-serve statement download from portal, family/multi-patient statements, insurance claim itemization, Fortis-side refund visibility on the statement.

## Architecture

**Balance recalc:** A new `StatementBalanceCalculator` service runs at statement-render time. It does what `cached_balance` was supposed to do — sums ledger entries for the patient (charges + payments + write-offs + credits) — but on demand. The performance impact is bounded because statement renders are user-initiated (not background batch), and the existing ledger index covers the query. See [[Statement Balance Recalc Pattern]].

**PDF generation:** Reuses the existing report engine (the same pipeline DX reports and Vital Signs CSV use). No client-side PDF generation. The statement renderer is a new template registered with the engine; the engine handles fonts, layout, page breaks, branding. See [[PDF Generation Pipeline]].

**Email delivery:** Statements queue through the existing outbound email service (the same one patient-engagement reminders use). A new `StatementSent` event lands on the patient timeline so staff can see when statements went out and to which address. See [[Email Delivery Architecture]].

**Portal preview:** Read-only embedded PDF in the patient portal — the same rendered file the staff sees. No separate render path. See [[Patient Portal Preview UX]].

**Key architectural decisions:**

- Recalc on render, not cached column ([[2026-04-08 Statement balance source — recalc on render vs cached]])
- Email-first delivery; SMS deferred to V2 ([[2026-04-14 Email vs SMS delivery — email-first for V1]])
- Server-side PDF via existing report engine ([[2026-04-21 Statement PDF generation — server-side via existing report engine]])
- Read-only embedded PDF in the portal ([[2026-04-28 Patient portal preview — read-only embedded PDF]])
- Branded email template with practice logo ([[2026-05-05 Email template — branded vs plain]])
- 3 pilot practices for a 2-week beta window ([[2026-05-12 Beta cohort — 3 pilot practices for 2 weeks]])

## Success Criteria

- Zero new stale-balance support tickets in the 4 weeks following broad release (baseline: ~14 tickets/month in Q1)
- Send-from-profile used by ≥60% of beta practices' billing staff at least once per week during the beta window
- Statement layout receives no "missing information" feedback from any beta clinic
- Dr. C signs off on a representative clinic's statement output as "shippable to TRP coaches" (her standard quality bar)

## Timeline

| Week | Focus | Status |
|------|-------|--------|
| Apr 6 | Kickoff, scope alignment, balance bug root-cause analysis | Done |
| Apr 13–17 | Recalc service built; statement layout draft 1 | Done |
| Apr 20–24 | PDF pipeline integration; portal preview spike | Done |
| Apr 27–May 1 | Send-from-profile UX; email template draft | Done |
| May 4–8 | Email template branded; staff preview UI; internal validation | Done |
| **May 11–15** | **Polish + beta cohort selection; pre-beta validation** | **Current (Phase 3)** |
| May 18–29 | Beta cohort (3 practices, 2-week feedback window) | Planned (Phase 4) |
| Jun 1–5 | Broad release to all practices | Planned |

## Known Risks

| Item | Severity | Owner | Notes |
|------|----------|-------|-------|
| Recalc performance under high-volume practices | Medium | Sam | Worst-observed render time on a 4-year-history patient: 380ms. Acceptable but flagged for monitoring. |
| Email deliverability — branded template with logo attachments | Medium | Sam + Maya | Need SPF/DKIM check for each beta practice's domain alias. |
| Beta practice email-template defaults | Low | Riley | Each beta practice configures their own; documenting in beta kit. |
| Send-from-profile permission scope | Low | Sam | Tied to existing billing-staff role — no new permission needed. |

## Open Questions

**Currently awaiting:**
- Beta practice 3 has not yet returned signed beta agreement (Maya following up)
- Sam validating render performance for the largest beta practice's history-heaviest patients

**Resolved:**
- Recalc on render is acceptable performance-wise — confirmed during 4/14 deep dive
- Email-first; SMS to V2 — confirmed 4/14
- Existing report engine handles statement layout — confirmed 4/21 spike
- Portal preview = embedded PDF, not separate HTML — confirmed 4/28
- Branded email template (not plain) — confirmed 5/5
- 3 beta practices, 2-week window — confirmed 5/12
