---
type: index
description: Master catalog of demo pod content. Claude reads this first to navigate.
updated: 2026-05-15
---

# Atlas Pod Vault — Demo Index

This is a **worked example** vault for the Atlas pod (PM: Riley Chen, Engineer: Sam Patel), populated with realistic content for the Patient Statements Redesign project. Use this as a reference for what a mature pod vault looks like once a project has run for a month or two.

## Pod Members

| Person | Role |
|--------|------|
| Riley Chen | Senior Product Manager |
| Sam Patel | Lead Software Engineer |

## Projects

| Project | Status | Phase | Sub-Index |
|---------|--------|-------|-----------|
| [Patient Statements Redesign](projects/statement-redesign/_index.md) | Active | Phase 3 (Polish + Beta) | 6 meetings, 6 decisions, 1 raw source |

## Wiki (Compiled Knowledge)

6 pages. See [shared/wiki/index.md](shared/wiki/index.md) for the full grouped listing.

| Page | Summary | Updated |
|------|---------|---------|
| [Statement Balance Recalc Pattern](shared/wiki/Statement Balance Recalc Pattern.md) | Recalc on render vs cached column — V1 fix for the stale-balance bug | 2026-05-15 |
| [PDF Generation Pipeline](shared/wiki/PDF Generation Pipeline.md) | Reusing the existing report engine for statement PDF rendering | 2026-05-15 |
| [Email Delivery Architecture](shared/wiki/Email Delivery Architecture.md) | How statement email integrates with ChiroHD's outbound email service | 2026-05-15 |
| [Patient Portal Preview UX](shared/wiki/Patient Portal Preview UX.md) | Read-only embedded PDF; one render, three consumers | 2026-05-15 |
| [Beta Cohort Selection](shared/wiki/Beta Cohort Selection.md) | How the Atlas pod chose the V1 beta cohort + selection criteria | 2026-05-15 |
| [Stakeholder Communication — Dr. C](shared/wiki/Stakeholder Communication — Dr. C.md) | Patterns observed working with Dr. C across the project arc | 2026-05-15 |

## Recent Activity (last 5 entries)

| Date | What | Key Outcome |
|------|------|-------------|
| 2026-05-15 | Pre-beta validation findings closed out | Both UX nits shipped; Pacific Care render-performance test passes; SPF/DKIM checks in progress |
| 2026-05-14 | Dev-complete announced in `#statement-redesign` | All Phase 3 work shipped: send-from-profile, branded email, StatementSent event, Statement Settings panel |
| 2026-05-12 | Beta cohort finalized — 3 practices, 2 weeks | Pacific Care, Foothill Family, Bay Ridge; decision recorded |
| 2026-05-11 | Pre-beta validation walkthrough (pod session) | Two UX nits found + fixed; beta kickoff confirmed for 5/18 |
| 2026-05-08 | STMT-106 send-from-profile modal shipped | Modal + preview + send; permission tied to existing billing-staff role |
