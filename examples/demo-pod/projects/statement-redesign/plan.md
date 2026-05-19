---
type: project-plan
project: statement-redesign
status: active
created: 2026-04-07
last_updated: 2026-05-15
---

# Patient Statements Redesign — Project Plan

## How We Track Work

Three layers, same conventions as the rest of the pod:

| Layer | Tool | What Lives Here | Audience | Cadence |
|-------|------|----------------|----------|---------|
| **Roadmap** | Monday | Milestone status, RAG, target dates | Dr. C, leadership | Weekly |
| **Engineering backlog** | JIRA (STMT-100) | User stories, bugs, tech tasks Sam owns | Sam, eng, Riley | Per-story |
| **Pod coordination** | This vault (`plan.md` + `tasks.md`) | Phases, blockers, PM work, beta logistics | Riley + Sam | Daily |

**The rule:** If Sam writes code for it, it's a JIRA story. If it's PM work, stakeholder coordination, content, or a decision to be made, it lives in the vault.

### Release schedule

CHD ships every other Monday night at 10:30 PM ET:
- **May 11** — Recalc service + layout V2 (Phase 2 release) — done
- **May 25** — Send-from-profile + branded email + beta enablement (Phase 3 release) — current target
- **Jun 8** — Broad release post-beta (Phase 4)

---

## Current State (as of 2026-05-15)

### What's built

Phase 1 (Recalc Service) and Phase 2 (Statement Layout + PDF Pipeline) both shipped on the May 11 release. Phase 3 (Send-from-Profile + Branded Email) is dev-complete and on the May 25 release branch. Sam announced dev-complete in `#statement-redesign` on May 14.

| Component | JIRA | Phase | Completed |
|-----------|------|-------|-----------|
| StatementBalanceCalculator service | [STMT-101](https://example.atlassian.net/browse/STMT-101) | 1 | Apr 17 |
| Recalc-on-render endpoint integration | [STMT-102](https://example.atlassian.net/browse/STMT-102) | 1 | Apr 22 |
| Statement layout V2 (PDF template) | [STMT-103](https://example.atlassian.net/browse/STMT-103) | 2 | Apr 24 |
| Report engine registration (statement template) | [STMT-104](https://example.atlassian.net/browse/STMT-104) | 2 | Apr 28 |
| Patient portal preview (embedded PDF) | [STMT-105](https://example.atlassian.net/browse/STMT-105) | 2 | May 4 |
| Send-from-profile modal + workflow | [STMT-106](https://example.atlassian.net/browse/STMT-106) | 3 | May 8 |
| Branded email template + practice-logo config | [STMT-107](https://example.atlassian.net/browse/STMT-107) | 3 | May 11 |
| StatementSent timeline event | [STMT-108](https://example.atlassian.net/browse/STMT-108) | 3 | May 13 |
| Statement Settings panel (CHD Admin → Office Config) | [STMT-109](https://example.atlassian.net/browse/STMT-109) | 3 | May 14 |

### What's remaining

| Item | Owner | Status | Notes |
|------|-------|--------|-------|
| Pre-beta validation pass on dev environment | Riley + Sam | In progress | Working through [[Validation Plan]] — render + send each statement type at least once per beta practice profile. |
| Beta practice 3 signed agreement | Maya | Pending | Two practices signed; third has a Q on the email-template configurability. Maya following up. |
| Email deliverability check per beta practice domain | Sam | Pending | SPF/DKIM check + a test send from the rendered template to each practice's billing-admin inbox. |
| Beta kickoff email | Riley | Drafted | Draft in [[Validation Plan]]; sends Monday after Maya confirms third practice. |
| Statement Settings configuration walkthrough (beta clinics) | Riley | Drafted | Reuse pattern from earlier pod beta kits — 1-pager. |

### Key blockers

None hard-blocking. Two coordination items:

| Item | Owner | Notes |
|------|-------|-------|
| Beta practice 3 confirmation | Maya | Question about email-template customization is being clarified; not a no. |
| Sam's render-performance check on the largest beta practice | Sam | Worst-case test in progress; will land before beta kickoff. |

---

## Phased Plan

### Phase 1: Recalc Service — "Fix the Stale-Balance Bug"
**Target: Apr 20 → Done Apr 22.**
**Status: COMPLETE.**

Built `StatementBalanceCalculator`. Wired it into the statement render endpoint. Verified output matches live ledger across the test dataset (12 patient histories, 4 with prior cached-balance drift). Cached column preserved for backward compat but no longer source of truth for statements. See [[Statement Balance Recalc Pattern]].

### Phase 2: Statement Layout + PDF Pipeline — "Cleaner Statement"
**Target: May 4 → Done May 4 (portal preview).**
**Status: COMPLETE.**

Registered new statement template with the report engine — same pipeline DX reports use. Layout: three-column body (charges, payments, credits), clear "amount due" footer, practice header. Portal embeds the rendered PDF read-only. See [[PDF Generation Pipeline]] and [[Patient Portal Preview UX]].

### Phase 3: Send-from-Profile + Branded Email — "Close the Workflow Gap"
**Target: May 25 release**
**Status: Dev-complete May 14. In pre-beta validation.**

Send-from-profile modal lives on Patient Profile → Billing tab. Date range + delivery method (email only V1) + render preview + send. Branded email template renders the statement as an attachment with a practice-logo header. `StatementSent` event lands on the patient timeline. Statement Settings panel in CHD Admin lets each practice configure their default note text + logo. See [[Email Delivery Architecture]].

### Phase 4: Beta + Broad Release
**Target: Beta May 18 → May 29; broad release Jun 1–8**

| Work Item | Owner | Type | Notes |
|-----------|-------|------|-------|
| Confirm 3 beta practices signed | Maya | Vault task | Two signed; third pending |
| Email deliverability per practice domain | Sam | Vault task | SPF/DKIM + test send |
| Beta kickoff email + 1-pager walkthrough | Riley | Vault task | Drafts in [[Validation Plan]] |
| Beta onboarding session (15 min Zoom per practice) | Riley + Sam | Vault task | Walk through Statement Settings + first send-from-profile |
| 2-week feedback window | All | — | Practices use send-from-profile, log feedback weekly |
| Triage feedback during window | Riley + Sam | Vault task | Weekly Friday triage |
| Broad release decision | Riley + Dr. C | Decision | After beta wraps May 29 |
| Broad release deploy | Sam | Vault task | Jun 1 or Jun 8 release |
| Monday roadmap update to "Released" | Riley | Vault task | After deploy |

---

## Pod Working Rhythm

| Day | Morning (9:00 AM – 12:00 PM ET) | Afternoon |
|-----|----------------------------------|-----------|
| Monday | Atlas pod working session | Riley: stakeholder coordination |
| Tuesday | Sam: solo build time | Riley: PM tasks, async |
| Wednesday | Atlas pod working session | Both: parallel work |
| Thursday | Sam: solo build time | Riley: stakeholder coordination |
| Friday | Atlas pod working session — review + triage | Flex |

**Pod session structure:**
1. **First 10 min:** Pull vault, check log.md, align on the day
2. **Middle 2.5 hours:** Build + validate. Sam codes, Riley validates flows + cross-stakeholder context
3. **Last 20 min:** Update tasks.md, commit + push, note async items

---

## Week-by-Week View

### Week 1: Apr 6 – 10 — Kickoff + Root Cause
- Kickoff (4/6); scope alignment
- Balance bug deep dive (4/13 was actually Apr 13 — see below)
- Note: kickoff was a half-week; we counted the first full week starting Apr 13

### Week 2: Apr 13 – 17 — Phase 1
- Balance bug deep dive (4/13) — identified `cached_balance` drift root cause
- Decision: recalc on render (4/8 — written up after the bug dive)
- Built `StatementBalanceCalculator` service ([STMT-101](https://example.atlassian.net/browse/STMT-101)) — done 4/17

### Week 3: Apr 20 – 24 — Phase 2 (PDF Pipeline)
- Delivery mechanism workshop (4/20) — email-first decided
- Statement PDF generation decision (4/21) — reuse report engine
- Statement layout V2 + report engine integration — done 4/24

### Week 4: Apr 27 – May 1 — Phase 2 (Portal Preview)
- Dr. C sync (4/27) — UX direction approved
- Portal preview decision (4/28) — embedded PDF
- Portal preview integration — done 5/4

### Week 5: May 4 – 8 — Phase 3 Build
- Email template decision (5/5) — branded with practice logo
- Send-from-profile modal + email template — done 5/8

### Week 6: May 11 – 15 — Polish + Beta Prep ← **CURRENT**
- May 11 release shipped Phase 1 + 2 + early Phase 3
- Beta cohort decided (5/12) — 3 pilot practices
- Beta cohort planning meeting (5/4)
- Pre-beta validation walkthrough (5/11)
- Dev-complete announced 5/14 in `#statement-redesign`
- This week: validation pass + deliverability checks + beta kickoff send

### Week 7: May 18 – 22 — Beta Onboarding
- Mon 5/18: Beta kickoff email sends; first onboarding session
- Tue–Thu: Onboarding sessions for the three practices
- Fri 5/22: First weekly triage of beta feedback

### Week 8: May 25 – 29 — Beta Mid-Window
- May 25 release — final tweaks if needed
- Daily feedback monitoring; weekly Friday triage

### Week 9: Jun 1 – 5 — Broad Release Prep
- Beta wraps May 29
- Broad release decision Jun 1
- Broad release Jun 1 (early) or Jun 8 (normal cadence) depending on feedback

---

## Revised Timeline

| Milestone | Original Target | Revised | Confidence |
|-----------|----------------|---------|------------|
| Phase 1 code-complete | Apr 24 | Apr 22 | Done |
| Phase 2 code-complete | May 4 | May 4 | Done |
| Phase 3 dev-complete | May 18 | May 14 | Done (ahead) |
| Beta kickoff | May 18 | May 18 | High |
| Beta wrap | May 29 | May 29 | High |
| Broad release | Jun 8 | Jun 1 (early) or Jun 8 | Medium — depends on beta feedback |

---

## Decision Points

| Date | Decision | Who Decides | Status |
|------|----------|-------------|--------|
| ~Apr 8 | Recalc on render vs cached column | Riley + Sam | Done — recalc on render |
| ~Apr 14 | Email vs SMS for V1 | Riley + Dr. C | Done — email-first |
| ~Apr 21 | Server-side vs client-side PDF | Sam + Ryan Caskey | Done — server-side, reuse engine |
| ~Apr 28 | Portal preview format | Riley + Dr. C | Done — embedded PDF read-only |
| ~May 5 | Branded vs plain email template | Riley + Dr. C | Done — branded |
| ~May 12 | Beta cohort size + window | Riley + Maya | Done — 3 practices, 2 weeks |
| May 29 | Broad release green light | Riley + Dr. C | Upcoming — after beta wrap |

---

## Assumptions

- Sam is ~100% on this project through Phase 4
- Riley is ~70% — splits time with broader product roadmap work
- Beta cohort signed agreements + email-template configuration land before May 18
- The May 25 release is the deploy point for the Phase 3 bundle (send-from-profile + branded email + StatementSent timeline event)
- Broad release contingent on clean beta (no critical bugs surfacing in the 2-week window)
- Dr. C is the approver for broad release sign-off, consistent with her pattern across pod projects
