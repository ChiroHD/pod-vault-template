---
type: meeting-note
date: 2026-05-11
attendees:
  - Riley Chen
  - Sam Patel
duration: ~110 min
source: pod-session
project: statement-redesign
tags:
  - validation
  - pre-beta
  - pod-session
  - polish
---

## Summary

Mon AM pod session. The May 11 release shipped Phase 1 + Phase 2 + most of Phase 3 the night before. Riley and Sam walked the [[Validation Plan]] end-to-end on the dev environment, rendering and sending statements against the three beta-practice profiles (low/mid/high volume). Found two small UX nits, logged both, Sam fixed them by Wed. Cleared the path for beta kickoff next Monday (5/18).

## Key Decisions

1. **Two small UX fixes land before beta.**
   - Statement Settings preview button — Sam adds a "Preview your branded email" button so practices can see their configured email rendering without sending a test send.
   - "Amount Due" footer spacing — the bolded value was visually crashing into the address footer on short statements. Sam to adjust template padding.
2. **Beta kickoff is Mon 5/18.** No new blockers surfaced from the walkthrough. Confidence high.
3. **Sam's render-performance check on Pacific Care's largest patient is the last gate.** Sam to land Thursday 5/14.

## Key Discussion Points

### The Walkthrough

Riley + Sam took 3 representative test patients (one per beta-practice profile) and ran the full statement flow for each:

| Practice Profile | Patient | Statement Date Range | Result |
|------------------|---------|---------------------|--------|
| Low volume / low portal | Bay Ridge — single-patient, mostly cash-pay | Last 30 days | Render ~140ms. PDF clean. Send-from-profile worked. Email rendered with default-empty note. |
| Mid volume / mid portal | Foothill Family — family with insurance + write-offs | Last 60 days | Render ~210ms. PDF clean. Write-offs visible in the credits column as expected. |
| High volume / high portal | Pacific Care — long-history patient, mixed payment methods | Last 90 days | Render ~340ms. PDF clean. Portal preview embedded correctly; PDF actions (download/print) worked. |

All three patients also rendered as a portal embed and Sam confirmed the patient view matches the staff preview pixel-for-pixel.

### Findings

**Two UX nits:**

1. **Statement Settings preview button:** Riley clicked into Statement Settings to set a logo and a default note text. After configuring, there's no way to see how the configured email will look without going back to a Patient Profile and doing a test send-from-profile. Annoying. Sam: "5-minute fix" — add a "Preview your branded email" button right in the settings panel.

2. **Footer spacing:** On the low-volume Bay Ridge test patient (short statement, only 3 line items), the bolded "Amount Due" footer was visually flush against the practice address footer. Looked cramped. Sam adjusts template padding — small CSS change.

Both logged as JIRA polish tasks; Sam closed both by Wed 5/13.

**Things that worked surprisingly well:**

- The recalc-on-render performance was better than Sam's worst-case estimate. The longest-history dev patient hit 380ms; production data should be similar.
- Branded email rendered correctly in all three test inboxes (Gmail, Outlook web, Apple Mail mobile). Logo + footer all came through.
- Portal embed worked first try on desktop, mobile-web, and the iOS app shell.

**Things to monitor in beta:**

- Email deliverability per beta-practice domain — Sam running SPF/DKIM checks this week.
- Practice-side experience of configuring Statement Settings — the panel is functional but not opinionated; new feature surface. Maya's onboarding walkthrough should land it clearly.

### Beta Kickoff Logistics

Riley confirmed:

- Beta kickoff email — drafted, in [[Validation Plan]], ready to personalize per practice
- Statement Settings 1-pager — drafted, in [[Validation Plan]]
- Three onboarding sessions scheduled for Tue/Wed/Thu next week (5/19–5/21)
- `#statement-beta` Slack channel — Maya standing up Mon AM 5/18
- Bay Ridge agreement — pending Maya's follow-up on a customization Q; not blocking the channel setup

### What Could Go Wrong

Riley asked Sam to enumerate the things he'd be most worried about during beta:

1. Email deliverability for one of the practice domains (SPF/DKIM misconfiguration on the practice side)
2. A render edge case in a long-history patient with unusual ledger entries (e.g., very old write-offs without a write-off reason set)
3. A practice configures the default note text with formatting (newlines, bullets) the renderer doesn't handle gracefully

Sam mitigations:

- (1) Pre-flight check this week for each beta practice
- (2) Sam to grep dev data for unusual ledger entries and walk a few
- (3) Default note field is single-line in the UI; multi-line input is constrained. Should be fine.

## Action Items

| Owner | Action | Due | Status |
|-------|--------|-----|--------|
| Sam | "Preview your branded email" button in Statement Settings | Thu 5/14 | Open |
| Sam | Adjust statement footer padding | Wed 5/13 | Open |
| Sam | Render-performance test on Pacific Care's largest patient | Thu 5/14 | Open |
| Sam | SPF/DKIM check per beta practice domain | Fri 5/15 | Open |
| Sam | Test send rendered statement to each practice's billing-admin inbox | Fri 5/15 | Open |
| Riley | Personalize beta kickoff email per practice | Fri 5/15 | Open |
| Riley | Final review of Statement Settings 1-pager | Fri 5/15 | Open |

## Cross-Refs

- Validation reference: [[Validation Plan]]
- Wiki: [[Beta Cohort Selection]]
- Next meeting: beta kickoff sessions week of May 18
