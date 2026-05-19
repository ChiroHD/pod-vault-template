---
type: log
description: Append-only pod timeline for the Atlas pod. Most recent entries at top.
---

# Atlas Pod Log

## 2026-05-15

- Riley: **Pre-beta validation findings closed out.** Sam shipped the two UX nits from Monday's walkthrough — "Preview your branded email" button in Statement Settings (5/14), and statement footer padding adjustment (5/13). Re-walked the checklist; both fixes look clean.
- Sam: **Render-performance test on Pacific Care's largest patient — done.** 4-year-history patient with ~6,200 ledger entries rendered in 410ms total. Well under the 1s budget. Logged the number in [[Validation Plan]] as a baseline for production monitoring.
- Sam: **SPF/DKIM checks in progress.** Pacific Care + Foothill Family domains pass; Bay Ridge pending Maya's confirmation that they're committed.
- Riley: **Confirmed Dr. C broad-release sign-off process.** Same as past pod projects — wrap summary in the 6/1 1:1, recommendation included, one-sentence approval expected. Updated [[Stakeholder Communication — Dr. C]] with the explicit confirmation.

## 2026-05-14

- Sam: **Dev-complete announced** in `#statement-redesign`. All Phase 3 work shipped and tested. STMT-106 (send-from-profile), STMT-107 (branded email template), STMT-108 (StatementSent event), STMT-109 (Statement Settings panel) all closed. Reactions: thumbs-up from Riley, party-popper from Maya.
- Sam: **STMT-109 Statement Settings panel shipped.** Per-practice logo upload, default note text, default delivery method, default date range. Office Configuration UI primitives reused — no new patterns. Verified in dev.
- Riley: **Beta kickoff email + 1-pager walkthrough — drafts done.** Both filed in [[Validation Plan]]. Will personalize per-practice once Maya confirms Bay Ridge.

## 2026-05-13

- Sam: **STMT-108 StatementSent timeline event shipped.** Lands on patient profile timeline with sent-to address + timestamp. Pattern mirrors the existing ReminderSent event — no new primitive.
- Sam: **Footer padding fix shipped** (from Monday's walkthrough finding). Bay Ridge-style short-statement layout no longer has the "Amount Due" line crashing into the practice address footer.
- Riley: **Started pre-beta validation pass on dev.** Working through [[Validation Plan]] for the low-volume profile (Bay Ridge-style test patient). One follow-up: write a clearer empty-state message when there are no charges in the date range. Sam picking up tomorrow.

## 2026-05-12

- Riley + Maya: **Beta cohort finalized — 3 practices, 2 weeks.** Decision recorded as [[2026-05-12 Beta cohort — 3 pilot practices for 2 weeks]]. Cohort: Pacific Care Chiropractic, Foothill Family Chiropractic, Bay Ridge Wellness. Two signed agreements in hand; Bay Ridge pending.
- Riley: **Updated plan.md** — beta kickoff confirmed for Mon 5/18. Phase 4 schedule now reflects two-week window (5/18 → 5/29) and 6/1 broad-release decision in Dr. C 1:1.
- Maya: **Bay Ridge follow-up** — practice has a Q about email-template customization. Maya clarifying that V1 = single configurable note text, no per-patient customization. Should land midweek.

## 2026-05-11

- Riley + Sam: **Pre-beta validation walkthrough (110m pod session).** Walked [[Validation Plan]] end-to-end on dev for all three beta-practice profiles. Found two small UX nits (Statement Settings preview button, footer padding); both logged. Performance across all three profiles well under budget. See [[2026-05-11 Pre-Beta Validation Walkthrough]].
- Sam: **May 11 release shipped.** Phase 1 (recalc service) + Phase 2 (layout + PDF + portal preview) + most of Phase 3 (send-from-profile + branded email template) live. Phase 3 gated behind `statementRedesign` feature flag for now; flip-on for beta cohort scheduled for 5/18.

## 2026-05-08

- Sam: **STMT-106 send-from-profile modal shipped.** Date range + delivery method + render preview + send. Permission tied to existing billing-staff role — no new permission added. Tested in dev across all three beta-practice profiles.
- Riley: **Reviewed STMT-106 send-from-profile flow.** One small ask: surface the sender's identity (practice + email) in the modal so staff know what address the email will come from. Sam queued the change for STMT-107 polish.

## 2026-05-05

- Riley + Dr. C: **Email template direction locked — branded, not plain.** Showed Dr. C both mockups in 1:1. She picked branded immediately, citing patient phishing-perception concern. Decision: [[2026-05-05 Email template — branded vs plain]]. Updates Statement Settings panel to include logo upload + configurable note text.
- Riley: **Logged Dr. C's "practice is the customer" framing** in [[Stakeholder Communication — Dr. C]]. Same pattern as her previous decisions on this project — the practice is the brand patients see, not ChiroHD.

## 2026-05-04

- Riley + Maya: **Beta cohort planning meeting.** Decided on selection criteria (volume + portal-adoption + feedback-quality axes) and shortlisted three practices. See [[2026-05-04 Beta Cohort Planning]]. Maya pushed back hard on the "first 3 volunteers" framing — won the argument cleanly.
- Sam: **STMT-105 portal preview shipped.** Embedded PDF in the existing portal viewer iframe. Read-only. Tested on desktop, mobile-web, and iOS app shell — all three render correctly.

## 2026-04-30

- Sam: **STMT-104 report engine registration shipped.** Statement template wired into the engine's template registry. Branding subsystem applies practice logo + address automatically. End-to-end render path working from staff click → PDF buffer in ~280ms (mid-volume profile).
- Riley: **Update to project-brief.md.** Marked Phase 2 dev-complete; updated Known Risks to remove "PDF library choice" (resolved).

## 2026-04-28

- Riley: **Portal preview decision recorded** — [[2026-04-28 Patient portal preview — read-only embedded PDF]]. Captures Dr. C's "one render, three consumers" framing from yesterday's sync.
- Sam: **Started STMT-104 engine registration.** Quick scope — template + branding plumbing.

## 2026-04-27

- Riley + Dr. C: **UX direction sync (30m 1:1).** Layout V2 approved as designed. Portal preview locked as embedded PDF (no HTML). Email template direction surfaced; follow-up scheduled for 5/5. See [[2026-04-27 Dr. C Sync — Statement UX Direction]].

## 2026-04-24

- Sam: **STMT-103 statement layout V2 shipped.** PDF template: three-column body (charges / payments / credits), single "amount due" footer, practice header. Renders correctly with the engine's branding subsystem.

## 2026-04-22

- Sam: **STMT-102 recalc service wired into render endpoint.** All statement renders now compute live via `StatementBalanceCalculator`. `cached_balance` no longer read by statements. Verified output matches live ledger across the 12-patient test set.

## 2026-04-21

- Sam: **PDF library spike resolved — existing report engine.** One-day spike confirmed the right call. Ryan Caskey weighed in via Slack DM: "If we already own the pipeline, we use the pipeline." Decision: [[2026-04-21 Statement PDF generation — server-side via existing report engine]].

## 2026-04-20

- Riley + Sam + Dr. C: **Delivery mechanism workshop.** Email-first for V1; SMS deferred. Dr. C joined late and aligned with the pod's lean. PDF library question opened for Sam's 4/21 spike. See [[2026-04-20 Delivery Mechanism Workshop]].
- Riley: **Updated jira-stories.md** — added STMT-201 (SMS delivery) to the Post-V1 backlog.

## 2026-04-17

- Sam: **STMT-101 StatementBalanceCalculator service shipped.** Single-patient query, indexed on `(patient_id, posted_at)`. Verified against the 12-patient test set including 4 known-drift histories. All four now compute correctly.

## 2026-04-14

- Riley: **Decision recorded — email-first for V1, SMS deferred.** [[2026-04-14 Email vs SMS delivery — email-first for V1]]. Written up ahead of the 4/20 workshop where Dr. C aligned.

## 2026-04-13

- Riley + Sam: **Statement Balance Bug Deep Dive (95m).** Root cause traced to `cached_balance` drift caused by non-transactional trigger interaction with failed-and-retried ledger writes. Three options considered; recalc-on-render chosen. See [[2026-04-13 Statement Balance Bug Deep Dive]] and the redacted transcript [[2026-04-13 Statement Balance Bug Deep Dive (redacted)]].
- Riley: **Decision recorded** — [[2026-04-08 Statement balance source — recalc on render vs cached]]. Backdated to 4/8 (when Riley first wrote up the architecture choice in notes), but the 4/13 deep dive is what made it real.

## 2026-04-10

- Riley: **Pulled all Q1 statement-related support tickets** into a Notion list. 47 tickets, 23 distinct practices, theme "statement total doesn't match ledger." Confirms the framing from the kickoff. Filed as background for Sam's 4/13 deep dive.
- Riley: **Drafted [[Statement Glossary]]** — terms specific to the statement domain. Started small; will expand as the project surfaces more terminology.

## 2026-04-09

- Sam: **JIRA epic STMT-100 created.** Phase 1 stories: STMT-101 (`StatementBalanceCalculator` service), STMT-102 (wire into render endpoint). Phase 2 + Phase 3 stories to be created after the 4/13 deep dive confirms direction.

## 2026-04-06

- Riley + Sam: **Kickoff session (50m).** Aligned on the three problems (stale balance, layout, send-from-profile), three-phase build, pod working rhythm (Mon/Wed/Fri AM). Patient-portal preview folded into Phase 2 scope after Sam confirmed the existing portal PDF viewer iframe makes it cheap. See [[2026-04-06 Statement Redesign Kickoff]].
- Riley: **Wrote initial [[project-brief]] and [[plan.md]]** stub. Filled in scope, stakeholders, three-phase outline. Will refine after the 4/13 deep dive.
