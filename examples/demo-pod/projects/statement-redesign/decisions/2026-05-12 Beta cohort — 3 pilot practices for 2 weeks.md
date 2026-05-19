---
type: decision
date: 2026-05-12
status: accepted
project: statement-redesign
decided_by:
  - Riley Chen
  - Maya Chen
source: 2026-05-04 Beta Cohort Planning
tags:
  - beta
  - rollout
  - coordination
---

## Decision

The Statement Redesign V1 beta cohort is **3 pilot practices** running for **2 weeks** (May 18 — May 29). Selection criteria:

1. **Different statement volumes** — one low-volume (~30 statements/month), one mid (~150), one high-volume (~400+) practice
2. **Mix of patient-portal usage** — at least one practice with high portal adoption (to validate the portal preview) and at least one with low portal adoption (to validate the email-only path)
3. **Strong billing-staff engagement** — practices Maya's team trusts to give substantive feedback, not just thumbs-up
4. **Not currently in any other ChiroHD beta** — avoid stacking beta load on a single practice

Selected cohort:

- **Pacific Care Chiropractic** (high volume, high portal adoption)
- **Foothill Family Chiropractic** (mid volume, mid portal adoption)
- **Bay Ridge Wellness** (low volume, low portal adoption)

Broad release decision happens June 1, after Riley and Dr. C review the 2-week feedback summary in 1:1.

## Context

The Statement Redesign touches a daily-use feature for billing staff and is patient-facing on the portal side. Rolling broad on May 25 with no beta would be the fastest path but risks shipping an issue (rendering edge case, deliverability problem, statement-format complaint) to all practices at once.

Riley and Maya met 5/4 to plan the beta. The discussion landed on a small cohort with explicit selection criteria rather than "first 5 practices who say yes" — the goal is feedback diversity, not signup volume.

Three is the size that emerged from the trade-off: large enough to surface real variance (low/mid/high volume + portal adoption), small enough that Riley + Sam can give each practice a 15-min onboarding session and Maya can field feedback without it becoming a separate support project. Two would be too narrow; five+ exceeds the pod's bandwidth for active support during the window.

Two weeks is the standard ChiroHD beta window for billing-area features. One week is too short to see a full statement cycle; three weeks delays broad release without typically surfacing new info.

## Rationale

- **Feedback diversity over feedback volume.** Three practices with different profiles will surface more variance than five similar practices. Volume + portal-adoption are the two axes most likely to expose edge cases.
- **Beta load is manageable.** 15-min onboarding × 3 + weekly Friday triage fits within pod cadence without displacing other work.
- **2 weeks is a full statement cycle.** Most practices send statements weekly or biweekly. Two weeks gives every practice in the cohort at least one full cycle plus a follow-up.
- **Selection criteria > "first to volunteer."** Maya pushed back on the original "ask any 3 practices" framing. Her experience: volunteer practices tend to be the most engaged with the platform, which biases feedback positive. Picking deliberately gets honest data.

## Consequences

- Three named beta practices on the cohort list. Maya owns coordination with each — signed agreements, scheduling, channel intros.
- The May 25 release deploys the Phase 3 bundle as feature-flagged behind `statementRedesign` for the broader user base, but flag-on for the beta practices only. (Feature flag pattern follows ChiroHD convention for beta scoping.)
- Each beta practice gets a 15-minute Zoom onboarding session — Statement Settings walkthrough + first send-from-profile + Q&A.
- A dedicated Slack channel `#statement-beta` hosts feedback. Maya monitors; Riley and Sam respond.
- Friday triage each week of the beta — 30 min, Riley + Sam review feedback, log decisions, queue any hot-fixes.
- Broad release decision Mon 6/1 in Dr. C 1:1. If beta clean: deploy Jun 1. If feedback to absorb: deploy Jun 8.

## Alternatives Considered

| Option | Why Rejected |
|--------|--------------|
| No beta — ship broad May 25 | Risk of shipping an edge-case bug or design issue to all practices at once. Cheap to do better. |
| 5+ practices | Beyond pod's ability to give each meaningful onboarding + support during the window. |
| 1 practice ("alpha") | Too narrow to expose variance across volume + portal usage. Single-data-point feedback. |
| 4 weeks | Delays broad release with no typical info gain after week 2. |

## History

- **2026-05-04:** Beta cohort planning meeting. See [[2026-05-04 Beta Cohort Planning]].
- **2026-05-12:** Cohort finalized. Two practices signed agreements that week.
- **2026-05-15:** Third practice (Bay Ridge) confirmation pending — Maya following up on an email-template customization question.
- **2026-05-18 (planned):** Beta kickoff.
