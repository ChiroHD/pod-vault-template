---
type: wiki
topic: Beta Cohort Selection
created: 2026-05-15
updated: 2026-05-15
sources:
  - "projects/statement-redesign/decisions/2026-05-12 Beta cohort — 3 pilot practices for 2 weeks.md"
  - "projects/statement-redesign/meetings/2026-05-04 Beta Cohort Planning.md"
  - "projects/statement-redesign/notes/Validation Plan.md"
---

# Beta Cohort Selection

How the Atlas pod chose the V1 Statement Redesign beta cohort. The pattern is generalizable to other pod projects: pick practices deliberately along feedback-relevant axes; don't take the first volunteers.

## The Cohort

Three pilot practices for a 2-week beta window (May 18 – May 29):

| Practice | Statement Volume | Portal Adoption | Why Picked |
|----------|------------------|-----------------|------------|
| Pacific Care Chiropractic | High (~400+/month) | High (~70% active) | Detail-oriented billing manager; exposes performance + portal preview edge cases |
| Foothill Family Chiropractic | Mid (~150/month) | Mid | Substantive feedback from billing staff; mid-market shape |
| Bay Ridge Wellness | Low (~30/month) | Low | Small-practice persona; catches edge cases big-practice testing misses |

## The Selection Pattern

Three axes Riley + Maya used to pick:

1. **Statement volume** — exposes performance + workflow edge cases. A practice sending 30/month vs 400+/month surfaces very different feedback.
2. **Portal adoption** — the portal preview is only validated by practices whose patients actually use the portal.
3. **Feedback quality** — Maya's team's read on which practices give substantive vs cursory feedback.

The first two are project-specific. The third is generalizable: Maya as the customer-side expert had a strong opinion about which practices were worth picking, and that opinion was load-bearing.

## What Was Rejected

| Option | Why Rejected |
|--------|--------------|
| "First 3 volunteers" | Volunteer practices skew positive. They love the platform; their feedback biases too kind. Picking deliberately gets honest data. |
| 5+ practices | Beyond Atlas pod's bandwidth for 15-min onboarding × 5 + weekly Friday triage. Diminishing feedback-diversity return past 3. |
| 1 practice ("alpha") | Too narrow to expose variance across the two feedback-relevant axes. Single-data-point feedback. |
| 4 weeks instead of 2 | Delays broad release with no typical info gain past week 2. Most practices statement on weekly/biweekly cycles — 2 weeks covers a full cycle plus follow-up. |

## Why 2 Weeks

Two weeks is ChiroHD's standard beta window for billing-area features. Most practices statement on a weekly or biweekly cycle. Two weeks gives every practice at least one full cycle of statement-send + patient response/feedback. Anything less is too short. Three weeks delays broad release without typically surfacing new info.

## Why 3 Practices

Three is the size where:

- Diversity across volume + portal-adoption axes is meaningful
- The pod can give each practice a 15-min onboarding session
- Maya can field feedback without it becoming a separate support project
- Friday triage stays under 30 minutes

Two would be too narrow. Five+ exceeds the pod's bandwidth for active support during the window.

## Cohort Operations

- **Maya** owns cohort coordination: signed agreements, scheduling, channel intros, weekly check-ins with each practice
- **Riley + Sam** run the three 15-min onboarding sessions in the same week (Tue/Wed/Thu of beta week 1)
- **`#statement-beta` Slack channel** — Maya stands up Mon AM, monitors throughout
- **Friday triage** — Riley + Sam meet 30 min each Friday during the window to review feedback, categorize, queue any hot-fixes

## Selection Anti-Patterns (Things to Avoid)

From Maya's experience across past pod betas:

- **Stacking betas on the same practice.** A practice already in another beta is split-focus; their feedback degrades. Selection criterion: not currently in any other ChiroHD beta.
- **Picking the loudest customer.** Loud doesn't equal substantive. Maya's read is the filter.
- **Picking only customers with strong relationships.** Strong relationships bias positive. Mix in at least one practice you don't know as well.
- **Skipping the small practice.** Big practices have more data so their feedback feels more "valid." But small practices catch the empty-state and lone-patient bugs.

## The Generalization

For any pod project's beta cohort:

1. Identify the 2-3 axes most likely to expose project-specific edge cases (volume, persona, feature usage, etc.)
2. Have the customer-side expert (Support Lead, Partnerships Lead, etc.) name candidates that span the axes
3. Pick a size that fits onboarding + weekly triage cadence within the pod's bandwidth
4. Use the standard 2-week window unless there's a specific reason to deviate

## Sources

- [[2026-05-12 Beta cohort — 3 pilot practices for 2 weeks]]
- [[2026-05-04 Beta Cohort Planning]]
- [[Validation Plan]] — beta kit drafts + during-beta tracking

## See Also

- [[Stakeholder Communication — Dr. C]] — broad-release sign-off pattern
