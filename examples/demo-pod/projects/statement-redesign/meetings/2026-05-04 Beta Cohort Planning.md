---
type: meeting-note
date: 2026-05-04
attendees:
  - Riley Chen
  - Maya Chen
duration: ~40 min
source: notes
project: statement-redesign
tags:
  - beta
  - cohort
  - rollout
  - coordination
---

## Summary

Riley and Maya planned the V1 beta cohort. They settled on three pilot practices with deliberately diverse profiles (volume + portal usage), a two-week feedback window, and explicit selection criteria over volunteer signup. Maya pushed back on Riley's original "first 3 who say yes" framing and won — Maya's experience is that volunteer practices skew engaged and bias feedback positive. The cohort got named on the spot.

## Key Decisions

1. **3 pilot practices, 2-week window.** Decision recorded as [[2026-05-12 Beta cohort — 3 pilot practices for 2 weeks]] after the cohort was finalized 5/12.
2. **Selection criteria over volunteer signup.** Three axes: statement volume (low/mid/high), portal adoption (low/mid/high), and "billing staff who give substantive feedback per Maya's read."
3. **Three named practices.** Pacific Care Chiropractic (high volume, high portal), Foothill Family Chiropractic (mid/mid), Bay Ridge Wellness (low/low).
4. **Maya owns cohort coordination.** Signed agreements, scheduling, channel intros. Riley owns onboarding session content; Sam attends each session.
5. **`#statement-beta` Slack channel.** Maya sets up, monitors. Riley and Sam respond.

## Key Discussion Points

### Why Not "First 3 Volunteers"

Riley's original framing: send a Slack to the practice-success team asking for 3 volunteers. Maya pushed back:

> "Volunteer practices are the ones who already love the platform. They'll tell you the redesign is great even if it isn't. Pick the practices you actually want feedback from."

Riley conceded immediately. Maya is the customer-side expert; the framing shifted to deliberate selection.

### The Three Axes

| Axis | Why It Matters |
|------|----------------|
| Statement volume | Exposes performance + workflow edge cases. A practice sending 30/month vs 400+/month surfaces very different feedback. |
| Portal adoption | The portal preview is only validated by practices whose patients actually use the portal. Need at least one high-adoption practice. |
| Feedback quality | Maya's team has a read on which practices give substantive vs cursory feedback. Picking engaged practices keeps the signal-to-noise high. |

### Cohort Selected

Maya pulled the practice list during the meeting and named three candidates that fit:

- **Pacific Care Chiropractic** — 400+ statements/month, 70%+ patient-portal active users. Maya's team has a strong relationship; billing manager is detail-oriented.
- **Foothill Family Chiropractic** — ~150 statements/month, mid portal adoption. Billing staff are not engineers but give clear feedback when something feels off.
- **Bay Ridge Wellness** — ~30 statements/month, low portal adoption. The "small practice" persona. Their feedback will catch the edge cases that big-practice testing misses (e.g., what happens when a patient is the only one on a statement, no portal use, etc.).

### Two Weeks

Riley initially proposed one week. Maya: too short. Most practices statement on a weekly or biweekly cycle. Two weeks gives every practice in the cohort at least one full cycle of statement-send + patient response/feedback.

Sam was looped in via Slack post-meeting and confirmed the feature-flag pattern supports a 2-week beta window with no rework — the `statementRedesign` flag can stay on for the cohort indefinitely.

### Onboarding Session Format

Riley + Sam will run a 15-min Zoom with each practice:

1. (3 min) Walk Statement Settings → logo, address, default note text
2. (5 min) First send-from-profile — preview, send, confirm patient receipt
3. (3 min) Walk the portal preview (only if patient-portal use is meaningful for that practice)
4. (4 min) Q&A + Slack channel intro

Maya joins each session — known face for the practice, continuity.

### Friday Triage

Every Friday during the beta window, Riley + Sam meet for 30 min to review feedback. Decisions:

- What's a hot-fix (ship next deploy)
- What's a documentation issue (update beta kit)
- What's a "logged for V2" item

## Action Items

| Owner | Action | Due | Status |
|-------|--------|-----|--------|
| Maya | Reach out to Pacific Care, Foothill Family, Bay Ridge — confirm participation + send beta agreements | May 8 | Open |
| Maya | Stand up `#statement-beta` Slack channel | May 18 | Open |
| Riley | Draft beta kickoff email | May 13 | Open |
| Riley | Draft Statement Settings configuration walkthrough 1-pager | May 13 | Open |
| Riley | Schedule the three 15-min onboarding sessions for week of May 18 | May 11 | Open |
| Sam | Confirm feature-flag pattern supports 2-week cohort | May 5 | Open |
| Riley | Record beta-cohort decision after cohort is finalized | May 12 | Open |

## Cross-Refs

- Decision recorded: [[2026-05-12 Beta cohort — 3 pilot practices for 2 weeks]]
- Wiki: [[Beta Cohort Selection]]
- Next meeting: [[2026-05-11 Pre-Beta Validation Walkthrough]]
