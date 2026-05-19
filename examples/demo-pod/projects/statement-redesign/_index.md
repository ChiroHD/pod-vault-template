---
type: project-index
project: statement-redesign
updated: 2026-05-15
---

# Patient Statements Redesign — Content Index

## Key Files

| File | Purpose |
|------|---------|
| [project-brief.md](project-brief.md) | Problem statement, scope, technical context |
| [plan.md](plan.md) | Build plan — phases, timeline, releases |
| [engineering-stories.md](engineering-stories.md) | Engineering stories cross-ref (JIRA-backed) + Post-V1 backlog |
| [tasks/tasks.md](tasks/tasks.md) | Active pod task board |
| [notes/Statement Glossary.md](notes/Statement Glossary.md) | Standing reference — terms specific to the statement domain |
| [notes/Validation Plan.md](notes/Validation Plan.md) | Pre-beta validation checklist + beta kit drafts |

## Meetings (6)

| Meeting | Date | Key Outcome |
|---------|------|-------------|
| [Statement Redesign Kickoff](meetings/2026-04-06 Statement Redesign Kickoff.md) | 2026-04-06 | Three problems framed; three-phase build; pod working rhythm |
| [Statement Balance Bug Deep Dive](meetings/2026-04-13 Statement Balance Bug Deep Dive.md) | 2026-04-13 | Root cause traced to `cached_balance` drift; recalc-on-render direction chosen |
| [Delivery Mechanism Workshop](meetings/2026-04-20 Delivery Mechanism Workshop.md) | 2026-04-20 | Email-first for V1; SMS deferred. PDF library spike kicked off. |
| [Dr. C Sync — Statement UX Direction](meetings/2026-04-27 Dr. C Sync — Statement UX Direction.md) | 2026-04-27 | Layout V2 approved; portal preview = embedded PDF; email template direction pending |
| [Beta Cohort Planning](meetings/2026-05-04 Beta Cohort Planning.md) | 2026-05-04 | 3 pilot practices, 2-week window, deliberate selection over volunteer signup |
| [Pre-Beta Validation Walkthrough](meetings/2026-05-11 Pre-Beta Validation Walkthrough.md) | 2026-05-11 | Two small UX nits found + fixed; beta kickoff confirmed for 5/18 |

## Decisions (6)

| Decision | Date | Status |
|----------|------|--------|
| [Statement balance source — recalc on render vs cached](decisions/2026-04-08 Statement balance source — recalc on render vs cached.md) | 2026-04-08 | Accepted |
| [Email vs SMS delivery — email-first for V1](decisions/2026-04-14 Email vs SMS delivery — email-first for V1.md) | 2026-04-14 | Accepted |
| [Statement PDF generation — server-side via existing report engine](decisions/2026-04-21 Statement PDF generation — server-side via existing report engine.md) | 2026-04-21 | Accepted |
| [Patient portal preview — read-only embedded PDF](decisions/2026-04-28 Patient portal preview — read-only embedded PDF.md) | 2026-04-28 | Accepted |
| [Email template — branded vs plain](decisions/2026-05-05 Email template — branded vs plain.md) | 2026-05-05 | Accepted |
| [Beta cohort — 3 pilot practices for 2 weeks](decisions/2026-05-12 Beta cohort — 3 pilot practices for 2 weeks.md) | 2026-05-12 | Accepted |

## Raw Sources (1)

| Source | Date | Contents |
|--------|------|----------|
| [Statement Balance Bug Deep Dive (redacted)](raw/2026-04-13 Statement Balance Bug Deep Dive (redacted).md) | 2026-04-13 | Redacted transcript of the root-cause investigation |

## Wiki Pages (relevant)

The following compiled-knowledge pages are most relevant to this project. Full wiki index at [shared/wiki/index.md](../../shared/wiki/index.md).

| Page | Topic |
|------|-------|
| [Statement Balance Recalc Pattern](../../shared/wiki/Statement Balance Recalc Pattern.md) | The architectural pattern + why |
| [Email Delivery Architecture](../../shared/wiki/Email Delivery Architecture.md) | How outbound email integrates |
| [PDF Generation Pipeline](../../shared/wiki/PDF Generation Pipeline.md) | Reusing the existing engine |
| [Patient Portal Preview UX](../../shared/wiki/Patient Portal Preview UX.md) | The UX pattern |
| [Beta Cohort Selection](../../shared/wiki/Beta Cohort Selection.md) | How the cohort was chosen + criteria |
| [Stakeholder Communication — Dr. C](../../shared/wiki/Stakeholder Communication — Dr. C.md) | Patterns observed working with Dr. C |
