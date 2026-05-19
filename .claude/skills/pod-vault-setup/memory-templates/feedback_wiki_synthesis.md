---
name: feedback-wiki-synthesis
description: "Every meeting/Slack/document ingest must check Step 4 of the ingest skill (wiki update), not just Step 3b (living-artifacts reconciliation). The compiled-knowledge layer is the Karpathy LLM Wiki concept's whole point; if it drifts, the vault loses long-term-memory value."
metadata: 
  node_type: memory
  type: feedback
  originSessionId: fe3b3c3b-44fa-45f6-946e-a84d68c71ade
---

Every ingest must run the ingest skill's **Step 4: Update the wiki**, not just Step 3b (living-artifacts reconciliation). Don't consider an ingest complete until you've checked whether the content surfaced a new topic worth a wiki page OR extends an existing topic's wiki page. Wiki updates are MANDATORY, not optional.

**Why:** The vault is structured per the Karpathy LLM Wiki concept (https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — a continuously-refined personal wiki of compiled knowledge, one topic per page, cross-linked, synthesizing across sources. The compiled-knowledge layer is the vault's LONG-TERM MEMORY, distinct from activity logs (`log.md`), operational state (`tasks.md`, `plan.md`), and raw content (`raw/`, `meetings/`).

In May 2026, {{PM_NAME}} audited this vault and found that operational artifacts were being kept current via the living-artifacts registry (mechanical, gets done because it's registry-driven), but `shared/wiki/` had been **frozen for 3+ weeks** since the vault was seeded. ~20 ingests happened in that window — major decisions, recurring patterns, new entities — and none of it had been compiled. The wiki step kept getting skipped because it requires synthesis judgment ("is this concept big enough?") which is easy to defer when there are 6 other things to update mechanically.

**How to apply:**

1. At the end of every ingest, ask explicitly: "Did this surface a new topic worth a wiki page, or extend an existing page?"
2. If **yes** and substantial: create/update the wiki page now, with source citations and cross-links. Don't defer.
3. If **borderline** (concept surfaced once, not clear if it'll recur): add to the `## Candidate Topics` backlog at the top of `shared/wiki/index.md` with a one-line description, date, and source pointer. Revisit when it surfaces a second time.
4. If **no**: explicitly say so in the ingest summary. **Forcing yourself to make the call prevents silent skips** — the failure mode that caused the 5/19 backfill.
5. **Promotion criteria** for moving Candidate Topics → real wiki pages: a concept has surfaced 2+ times, OR is a structural concept (governance, architecture pattern, recurring constraint), OR has been referenced as something {{PM_NAME}} or {{ENGINEER_NAME}} will need to consult later.

**What the wiki should look like (per Karpathy):**

- One topic per page (not chronological, not per-meeting)
- Pages compile information across multiple sources with citations
- Cross-linked liberally via `[[wikilinks]]`
- Refined over time as understanding deepens — pages get rewritten, not appended
- Concise — aim for a page someone (or Claude) can read in 2 minutes
- Listed in `shared/wiki/index.md`

**Related:** [[feedback-ingest-transcripts]] — the parallel rule that every ingest must capture the source transcript. Both rules exist to prevent specific drift patterns that happened in May 2026.
