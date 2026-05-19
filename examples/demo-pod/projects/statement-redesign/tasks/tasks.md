---
type: tasks
project: statement-redesign
updated: 2026-05-15
---

# Statement Redesign — Pod Tasks

Daily coordination tasks. Engineering work lives in JIRA (STMT-100). See `plan.md` for the full project plan and [[jira-stories]] for the JIRA cross-reference.

## This Week — Pre-Beta Validation (Mon 5/11 → Fri 5/15)

### Riley

| # | Task | Owner | Due | Status | Notes |
|---|------|-------|-----|--------|-------|
| 1 | Walk [[Validation Plan]] on dev environment — every statement type, every beta-practice profile | Riley | Fri 5/15 | In Progress | Pair with Sam Fri AM for the tricky cases (negative-balance write-offs, refunds) |
| 2 | Finalize beta kickoff email | Riley | Fri 5/15 | Drafted | Draft in [[Validation Plan]]; sends Monday once Maya confirms third practice |
| 3 | 1-pager — Statement Settings configuration walkthrough | Riley | Fri 5/15 | Drafted | Reuse the same pattern previous pod betas used |
| 4 | Confirm Dr. C broad-release sign-off process — same as past pod projects? | Riley | Fri 5/15 | Done | Confirmed: Dr. C reviews beta-week-2 feedback summary, signs off in 1:1 |
| 5 | Update Monday roadmap card — phase 3 dev complete, beta begins next week | Riley | Mon 5/18 | Queued | — |

### Sam

| # | Task | Owner | Due | Status | Notes |
|---|------|-------|-----|--------|-------|
| 6 | Render-performance test against Pacific Care Chiro's 4-year-history patients | Sam | Thu 5/14 | Done | Worst case 380ms; well under the 1s render budget |
| 7 | SPF/DKIM check for each beta practice's domain | Sam | Fri 5/15 | In Progress | Pacific Care + Foothill Family done; Bay Ridge pending Maya confirm |
| 8 | Test send rendered statement to each practice's billing-admin inbox | Sam | Fri 5/15 | Queued | Validates deliverability against the actual mailbox |
| 9 | Patch — Statement Settings panel: add a "Preview your branded email" button | Sam | Thu 5/14 | Done | Small QoL ask from Riley after 5/11 validation walk |

### Maya (Support Lead — async)

| # | Task | Owner | Due | Status | Notes |
|---|------|-------|-----|--------|-------|
| 10 | Confirm beta practice 3 (Bay Ridge Wellness) signed agreement | Maya | Fri 5/15 | In Progress | Question about email-template customization being clarified |
| 11 | Set up dedicated Slack channel `#statement-beta` for the cohort | Maya | Mon 5/18 | Queued | Pattern from past beta cohorts |

---

## Active — Week of May 18 (Beta Onboarding)

| Task | Owner | Status | Due | Notes |
|------|-------|--------|-----|-------|
| Send beta kickoff email | Riley | Not Started | Mon 5/18 AM | Personalize per practice; CC Maya |
| Beta onboarding session — Pacific Care Chiro (15 min Zoom) | Riley + Sam | Not Started | Tue 5/19 | Walk Statement Settings; first send-from-profile |
| Beta onboarding session — Foothill Family Chiropractic | Riley + Sam | Not Started | Wed 5/20 | Same agenda |
| Beta onboarding session — Bay Ridge Wellness | Riley + Sam | Not Started | Thu 5/21 | Contingent on Maya confirming signed agreement |
| Friday triage — first week of beta feedback | Riley + Sam | Not Started | Fri 5/22 | 30 min review of any feedback logged in the cohort's feedback channel |

---

## Active — Week of May 25 (Beta Mid-Window + Release)

| Task | Owner | Status | Due | Notes |
|------|-------|--------|-----|-------|
| May 25 release deploy | Sam | Not Started | Mon 5/25 PM | Phase 3 bundle (send-from-profile + branded email + Statement Settings + StatementSent) |
| Monitor beta channel daily; respond within 24h | Riley + Maya | Not Started | Through 5/29 | — |
| Friday triage — week 2 of beta | Riley + Sam | Not Started | Fri 5/29 | Last triage before broad release decision |
| Beta wrap summary for Dr. C 1:1 | Riley | Not Started | Mon 6/1 | Feedback themes, outstanding bugs (if any), broad release recommendation |

---

## Active — June (Broad Release)

| Task | Owner | Status | Due | Notes |
|------|-------|--------|-----|-------|
| Broad release decision in Dr. C 1:1 | Riley | Not Started | Mon 6/1 | Recommend Jun 1 deploy if beta clean; Jun 8 if any feedback to absorb |
| Broad release deploy | Sam | Not Started | Jun 1 or Jun 8 | Same release process |
| Update Monday roadmap to "Released" | Riley | Not Started | Day-of release | — |
| Post-release: monitor support ticket volume for stale-balance + statement-send issues | Riley + Maya | Not Started | 4 weeks post-release | Success criterion: zero new stale-balance tickets |
| Post-release cleanup: schedule `cached_balance` column drop ticket (STMT-204) | Sam | Not Started | 2 release cycles post-launch | Backward-compat safety window |

---

## Completed

| Task | Owner | Completed | Notes |
|------|-------|-----------|-------|
| Send-from-profile workflow built and tested (STMT-106) | Sam | 2026-05-08 | Modal + render preview + email send. Permission tied to existing billing-staff role. |
| Branded email template + practice-logo config (STMT-107) | Sam | 2026-05-11 | Per-practice logo + footer + configurable note text |
| StatementSent timeline event (STMT-108) | Sam | 2026-05-13 | Lands on patient profile timeline with sent-to address + timestamp |
| Statement Settings panel — Office Configuration (STMT-109) | Sam | 2026-05-14 | Per-practice default note text, logo upload, default delivery method |
| Pre-beta validation walkthrough (joint, working session) | Riley + Sam | 2026-05-11 | Found 2 small UX nits — logged + fixed by 5/13 |
| Beta cohort decided — 3 pilot practices, 2-week window | Riley + Maya | 2026-05-12 | Pacific Care, Foothill Family, Bay Ridge |
| Email template branding direction confirmed with Dr. C | Riley | 2026-05-05 | Branded with per-practice logo, not plain |
| Portal preview shipped (STMT-105) | Sam | 2026-05-04 | Embedded PDF read-only, reuses existing portal viewer |
| Dr. C sync — UX direction approved | Riley | 2026-04-27 | Layout V2 mockups + portal preview format both approved |
| Report engine registration (STMT-104) | Sam | 2026-04-28 | Statement template registered, branding subsystem reused |
| Statement layout V2 PDF template (STMT-103) | Sam | 2026-04-24 | Three-column body, "amount due" footer, practice header |
| Delivery mechanism workshop | Riley + Sam | 2026-04-20 | Email-first decided; SMS deferred to V2 |
| Recalc service wired into render endpoint (STMT-102) | Sam | 2026-04-22 | All statement renders now compute live |
| StatementBalanceCalculator built (STMT-101) | Sam | 2026-04-17 | Single-patient query, indexed; verified against 12 patient histories |
| Statement balance bug root cause identified | Riley + Sam | 2026-04-13 | `cached_balance` drifts when ledger writes fail silently |
| Project kickoff with Sam | Riley | 2026-04-06 | Scope, phases, working rhythm |

---

## Blocked

| Task | Owner | Blocker | Notes |
|------|-------|---------|-------|
| Beta onboarding session — Bay Ridge Wellness | Riley + Sam | Maya confirming signed agreement | Two practices confirmed; third pending |
| `cached_balance` column drop (STMT-204) | Sam | 2-release safety window | Backlog — not blocking V1 |
