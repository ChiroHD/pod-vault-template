---
type: wiki-index
description: Master catalog of compiled knowledge pages for the demo pod. Claude uses this to navigate the wiki.
updated: 2026-05-15
---

# Wiki Index

This wiki follows the Karpathy LLM Wiki pattern. Pages are compiled from raw sources by Claude, not written by hand. Each page cites its sources and cross-links related concepts. One topic per page, ~2-minute reads, refined over time as understanding deepens.

## How to Use

- **Ingest:** Drop a raw source into `projects/<project>/raw/` and ask Claude to `/ingest`. Step 4 of the ingest skill checks whether wiki updates are needed (mandatory, not optional).
- **Query:** Ask Claude questions — it navigates via this index to find relevant pages.
- **Lint:** Periodically run the vault-overhaul skill to check for contradictions, orphans, stale claims, and missing-page references.

## Pages

### Architectural patterns

- [[Statement Balance Recalc Pattern]] — Recalc on render vs cached column; the V1 fix for the stale-balance bug
- [[PDF Generation Pipeline]] — Reusing the existing report engine for statement PDF rendering
- [[Email Delivery Architecture]] — How statement email integrates with ChiroHD's outbound email service

### Product / UX concepts

- [[Patient Portal Preview UX]] — Read-only embedded PDF; one render, three consumers

### Operational concepts

- [[Beta Cohort Selection]] — How the Atlas pod chose the V1 beta cohort; generalizable pattern

### Pattern observations

- [[Stakeholder Communication — Dr. C]] — Patterns observed working with Dr. C across the Statement Redesign project

---

## Candidate Topics (Backlog)

This list captures topics that have surfaced in recent ingests but aren't yet substantial enough for a dedicated page. Promote to a real page once a topic accumulates enough material (typically 2+ ingests + a recurring pattern).

*Currently empty.*

When a new ingest surfaces a topic that fits the promotion criteria, either (a) move it from this list to a real wiki page, or (b) add a new candidate row here for tracking.
