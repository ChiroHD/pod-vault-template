---
type: meeting-note
date: 2026-04-06
attendees:
  - Riley Chen
  - Sam Patel
duration: ~50 min
source: fathom-transcript
project: statement-redesign
tags:
  - kickoff
  - scope
  - phases
---

## Summary

First working session between Riley and Sam on the Statement Redesign project. Aligned on the three problems the project addresses (stale-balance bug, layout cleanup, send-from-profile workflow), agreed on a three-phase build with phase-aligned releases, and set the pod's working rhythm. Sam pulled the open billing-staff support tickets to confirm Riley's framing matched what staff were actually complaining about — it did, with one addition (the patient-portal preview ambiguity that Riley folded into scope as a Phase 2 sub-goal).

## Key Decisions

1. **Three-phase build, three releases.** Phase 1 (recalc service) → Phase 2 (layout + PDF + portal preview) → Phase 3 (send-from-profile + branded email). Each phase ships independently. Decision: ride the standard biweekly release cadence — May 11 release for Phase 1+2, May 25 for Phase 3.
2. **Stale-balance fix lands first.** It's the highest-volume support pain point. Shipping it on May 11 buys goodwill from billing staff before the bigger layout + workflow changes land.
3. **Pod working rhythm: Mon/Wed/Fri AM joint sessions.** 9 AM – 12 PM ET. Tuesday and Thursday are Sam's solo build days.
4. **Patient-portal preview folded into scope.** Riley originally had this as "consider for V1." Sam's investigation showed the existing portal PDF viewer iframe makes it ~0 marginal effort to embed. Pulled in.

## Key Discussion Points

### The Three Problems

Riley framed the project around three discrete problems she'd documented from Q1 support tickets:

1. **Stale balance:** Statement total doesn't match the live ledger. Tracked to `cached_balance` drift but root cause unconfirmed. Sam to investigate week 2.
2. **Layout noise:** Current statement has charges/payments/credits/adjustments/write-offs all interleaved in one column with inconsistent ordering. Patients (and patients' family members) complain they can't tell what they owe. The bolded total at the bottom is the only clear signal.
3. **Send workflow gap:** No way to email a statement from the patient profile. Staff download the PDF, switch to their email client, attach, type a message, send. ~6-8 clicks of context-switch per send.

Sam confirmed all three appear in JIRA support backlog. Added one observation: the patient portal has a "Statements" section that just lists statement dates — clicking opens the same PDF the staff can download. Patients have been asking if they can preview the statement in the portal rather than download it. Riley pulled this into Phase 2 scope.

### Why Three Releases

Sam asked whether to ship everything together. Riley's reasoning:

- Phase 1 (stale-balance fix) is the most acute pain point. Earliest delivery wins goodwill.
- Phase 2 (layout + PDF + portal preview) is a coherent visual-design change. Ships together so the patient experience changes in one moment, not two.
- Phase 3 (send-from-profile + branded email) is the workflow change. Different audience (billing staff vs patients). Ships later so beta feedback on Phase 2 can land before Phase 3.

Sam agreed. Three releases gives three signal points to learn from.

### Working Rhythm

Riley: Mon/Wed/Fri AM joint pod sessions, 9 AM – 12 PM ET. Sam codes during sessions when blocked-free; Riley validates flows and handles stakeholder context. Tuesday and Thursday are Sam's solo build days. Friday afternoon is flex.

Sam: agreed. Asked Riley to keep the Wed session light on stakeholder syncs so it stays a build day; Riley parked Dr. C syncs to Mondays.

### Open Questions Going Into Week 2

- Root cause of `cached_balance` drift (Sam — deep dive 4/13)
- Email vs SMS for V1 send workflow (Riley — workshop 4/20)
- Server-side vs client-side PDF (Sam — spike during Phase 2)

## Action Items

| Owner | Action | Due | Status |
|-------|--------|-----|--------|
| Sam | Schedule Statement Balance Bug Deep Dive — block 2 hrs to trace root cause | Apr 13 | Open |
| Riley | Schedule Delivery Mechanism Workshop with Dr. C | Apr 20 | Open |
| Riley | Pull all billing-staff Q1 statement tickets into a single Notion list for reference | Apr 10 | Open |
| Sam | Confirm patient portal's existing PDF viewer iframe is reusable for Phase 2 preview | Apr 13 | Open |
| Both | Create JIRA epic STMT-100 + Phase 1 stories | Apr 9 | Open |

## Cross-Refs

- Decision context: this kickoff predates all decision records. The three problems Riley framed here became the scope foundation for all subsequent decisions.
- Next meeting: [[2026-04-13 Statement Balance Bug Deep Dive]]
