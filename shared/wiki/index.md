---
type: wiki-index
description: Master catalog of compiled knowledge pages. Claude uses this to navigate the wiki.
updated: {{CREATED_DATE}}
---

# Wiki Index

This wiki follows the Karpathy LLM Wiki pattern. Pages are compiled from raw sources by Claude, not written by hand. Each page cites its sources and cross-links related concepts. One topic per page, ~2-minute reads, refined over time as understanding deepens.

## How to Use

- **Ingest:** Drop a raw source into `projects/<project>/raw/` and ask Claude to `/ingest`. Step 4 of the ingest skill checks whether wiki updates are needed (mandatory, not optional — see [feedback-wiki-synthesis memory](file:///.claude/projects/.../memory/feedback_wiki_synthesis.md)).
- **Query:** Ask Claude questions — it navigates via this index to find relevant pages.
- **Lint:** Periodically run `/vault-overhaul` to check for contradictions, orphans, stale claims, and missing-page references.

## Pages

*No wiki pages yet — they'll appear here as content is ingested and concepts accumulate.*

The categories below are placeholders for the typical structure a mature vault develops. Move topics into the right category as they're created.

### Operational concepts
*e.g., how this pod runs, beta strategy, working rhythm — populated as the pod establishes patterns*

### Product / domain concepts
*e.g., key product metrics, feature areas, technical patterns — populated as project knowledge accumulates*

### Infrastructure
*e.g., test environments, deploy patterns, integration points*

### Pattern observations
*e.g., recurring stakeholder decision patterns, recurring engineering patterns*

### Project status / history
*e.g., resolved blockers, queued work, historical context*

---

## Candidate Topics (Backlog)

⚠️ This list captures topics that have surfaced in recent ingests but aren't yet substantial enough for a dedicated page. Promote to a real page once a topic accumulates enough material (typically 2+ ingests + a recurring pattern).

*Currently empty.*

When a new ingest surfaces a topic that fits the promotion criteria, either (a) move it from this list to a real wiki page, or (b) add a new candidate row.
