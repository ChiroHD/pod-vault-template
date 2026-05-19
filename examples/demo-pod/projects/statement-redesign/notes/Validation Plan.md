---
type: reference
date: 2026-05-11
project: statement-redesign
purpose: Testing approach for the V1 beta + the artifacts (kickoff email, walkthrough 1-pager) the beta cohort needs
tags:
  - type/reference
  - project/statement-redesign
  - beta
---

# Validation Plan

How the pod validates Statement Redesign V1 before, during, and after beta. Also hosts the first drafts of the beta-kit artifacts (kickoff email + Statement Settings walkthrough). Refined during the 5/11 pre-beta validation walkthrough.

## Pre-Beta Validation (May 11 – May 17)

### Scope

For each beta-practice profile (low/mid/high volume), do the following end-to-end on the dev environment:

1. **Render** a statement for at least one representative test patient
2. **Send-from-profile** to a test inbox
3. **Verify** the rendered PDF matches the staff preview pixel-for-pixel
4. **Verify** the timeline `StatementSent` event appears with correct sent-to address + timestamp
5. **Open** the same statement in the portal preview path and verify it matches

### Checklist (run once per beta-practice profile)

- [ ] Statement Settings configured (logo upload, default note text)
- [ ] Send-from-profile modal opens from Patient Profile → Billing tab
- [ ] Date range default works; custom date range works
- [ ] Render preview matches the rendered PDF
- [ ] Email arrives in test inbox with branded template
- [ ] PDF attachment opens cleanly; layout matches preview
- [ ] Plain-text fallback renders sensibly in the email client raw view
- [ ] `StatementSent` event lands on patient timeline
- [ ] Portal preview embed loads; PDF actions (download/print) work
- [ ] Portal preview matches staff preview pixel-for-pixel

### Edge cases to walk

- Patient with very long history (4+ years, several thousand ledger entries) — performance + page break
- Patient with no recent activity in the date range — empty-state message
- Patient with credits exceeding charges in the range — negative "amount due" handled correctly
- Patient with insurance write-offs visible in the credits column
- Practice with no logo configured — falls back to text-only header
- Practice with multi-line default note — clamped to single line in the rendered email
- Send-from-profile where staff doesn't have a billing-staff role — permission denial works

### Performance Targets

- Statement render-to-PDF: < 1s (production budget)
- Email send queue-to-delivery: < 30s (existing outbound email SLA)
- Portal preview load: < 2s (existing portal viewer iframe)

## During-Beta Validation (May 18 – May 29)

### What the beta practice does

Each beta practice's billing staff use send-from-profile in normal daily workflow for the 2-week window. No special validation steps for them — that's the point. The pod monitors usage and feedback.

### What the pod does

- **Daily:** Maya monitors `#statement-beta` Slack channel for questions/feedback
- **Daily:** Sam monitors APM for any render error spikes or email-send failures
- **Weekly Friday:** Riley + Sam triage feedback for 30 minutes
  - Hot-fix: ship next deploy (May 25 release if pre-deploy, or off-cycle if critical)
  - Documentation fix: update beta kit, send 1-line Slack note to cohort
  - Logged for V2: file in backlog, no action this cycle

### Tracking

A simple table maintained in this file during beta:

| Practice | Date | Feedback | Category | Resolution |
|----------|------|----------|----------|------------|
| (entries added during beta) | | | | |

## Post-Beta Validation (May 29 – Jun 1)

### Wrap summary for Dr. C 1:1

Riley prepares a one-page summary for the 6/1 Dr. C 1:1:

- Themes from the 2-week feedback
- Hot-fixes shipped during the window (if any)
- Outstanding issues (if any) and their disposition
- Recommendation: broad release on Jun 1 or hold for Jun 8

### Broad-release readiness

- [ ] Zero critical bugs open from beta feedback
- [ ] All beta practices report no blockers
- [ ] Render-performance metrics from beta look healthy
- [ ] Email deliverability metrics across all three practices look healthy
- [ ] Dr. C signs off in 6/1 1:1

## Beta Kit — Drafts

### Beta Kickoff Email (Draft)

> Subject: Statement Redesign Beta — kickoff & walkthrough
>
> Hi [billing manager name],
>
> Thanks for joining the Statement Redesign V1 beta. Over the next two weeks (May 18 – May 29), your practice has access to the redesigned patient statement experience — including the new send-from-profile workflow and the cleaned-up statement layout.
>
> **Your onboarding session:** [date/time] (15 min, Zoom link). I'll be joined by Sam (engineer) and Maya (support).
>
> **What to expect during the beta:**
> - Use send-from-profile in your normal workflow — that's the whole point.
> - Feedback any time in the `#statement-beta` Slack channel (Maya will add you).
> - Friday email summary from us with the week's status + any heads-up.
>
> **Before the session, please:**
> - Confirm your practice's logo is uploaded in CHD Admin → Office Configuration → Statement Settings (we can do this together if not).
> - Decide if you want a default note text on your statement emails (also configurable in Statement Settings).
>
> Looking forward to it.
>
> — Riley

### Statement Settings Walkthrough (1-pager Outline)

Headings:

1. **What is Statement Settings?** — One sentence; where to find it (CHD Admin → Office Configuration → Statement Settings).
2. **Fields you can configure**
   - Practice logo (PNG or JPG, recommended 300×100px)
   - Default note text on statement emails (single line, optional)
   - Default delivery method (Email — only option in V1)
   - Default statement date range (Last 30 / 60 / 90 days)
3. **How send-from-profile uses these defaults** — Modal pre-fills date range from Statement Settings; email body uses logo + note. Staff can override per send.
4. **What to do if something looks wrong** — Try the "Preview your branded email" button. If still off, Slack `#statement-beta`.

Riley to finalize the 1-pager with screenshots before Mon 5/18.

## Findings Log (running)

Updated during pre-beta + during-beta. Entries above the line are pre-beta findings (already addressed); entries below the line will be filled in during the beta window.

### Pre-Beta (May 11 walkthrough)

| Finding | Severity | Resolution |
|---------|----------|------------|
| No way to preview the branded email from Statement Settings | Low | Sam added "Preview your branded email" button — shipped 5/14 |
| Amount Due footer crashes into address footer on short statements | Low | Sam adjusted template padding — shipped 5/13 |

### During-Beta (May 18 – May 29)

*(To be populated during the beta window.)*
