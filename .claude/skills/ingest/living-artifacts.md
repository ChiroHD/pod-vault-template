---
name: living-artifacts-registry
description: Dynamic registry of living artifact types reconciled on every ingest. The ingest skill reads this file at Step 3b — this is the single source of truth, not a hardcoded list in the skill definition.
last_updated: 2026-05-19
---

# Living Artifacts Registry

Each entry defines a living artifact type that must be read and reconciled on every ingest. The ingest skill reads this file dynamically — adding an entry here automatically includes it in future ingests.

## Registered Artifacts

### 1. Task Board
- **Path pattern:** `projects/<project>/tasks/tasks.md`
- **What to reconcile:**
  - Mark tasks complete if the ingested content shows they were done
  - Add new tasks that emerge from the content
  - Update task status if progress is evident (Not Started → In Progress, add notes)
  - Move blocked tasks if blockers were resolved
  - Do NOT create separate per-meeting task lists — all tasks go in the main tasks.md
- **Scope rule:** Only include tasks directly related to the pod's project

### 2. People Notes
- **Path pattern:** `shared/people/*.md`
- **What to reconcile:**
  - Add dated update blocks with new observations, stated preferences, feedback, commitments, or status changes
  - Capture: communication style signals, decision patterns, concerns raised, action items committed to
- **Threshold:** Only update if there's substantive new information, not just "they attended a meeting"

### 3. Decision Records
- **Path pattern:** `projects/<project>/decisions/*.md`
- **What to reconcile:**
  - If a prior decision is superseded: add `status: superseded` and `superseded_by:` to old record, add `supersedes:` to new record, add History entry
  - If a prior decision gets new context: add a History entry with the new information

### 4. Project Plan
- **Path pattern:** `projects/<project>/plan.md`
- **What to reconcile:**
  - Timeline shifts, phase changes, scope adjustments
  - Blocker additions or resolutions
  - Working rhythm changes
  - Week-by-week view updates if specific weeks are affected

### 5. Project Brief
- **Path pattern:** `projects/<project>/project-brief.md`
- **What to reconcile:**
  - Current state tables (What's built / What's not built)
  - Known blockers — resolve or add
  - Open questions — check boxes and add answers
  - Timeline table updates
  - Stakeholder or scope changes

### 6. Engineering Stories
- **Path pattern:** `projects/<project>/engineering-stories.md` (legacy: `jira-stories.md`)
- **What to reconcile:**
  - Story descriptions, acceptance criteria, context links
  - Unblocked stories — remove or update "Blocked by" notes
  - Stale Pod Vault Context links (decision superseded)
- **Ticket-system lifecycle rule:** Read the `ticket_system:` field in the file's frontmatter (values: `jira`, `gitlab`, `jira+gitlab`). The rule is the same for either system — the external ticket is the source of truth once it exists; the vault copy is a navigation aid that must not be silently edited into divergence.
  - **Before editing any story:** check if it has a ticket reference in the external system
    - JIRA: pattern like `CHD-XXXX` / `STMT-XXX` / `<KEY>-<NUM>` in the story header or a `jira_ticket:` field
    - GitLab: pattern like `<group>/<project>#NNNN` or a `gitlab_issue:` field, or an `&NN` epic reference
    - Split (`jira+gitlab`): check both — the project decides which system holds source of truth; usually one engineer's primary system. If unclear, treat as if both exist and surface a task to clarify.
  - **If the story has an external ticket:**
    - Do NOT edit the vault copy — the external ticket is the source of truth and editing the vault creates divergence
    - Instead, create a task in tasks.md: `Update <SYSTEM> ticket <REF>: [describe what changed]` (e.g., `Update JIRA story CHD-8366: scope reduced per [[2026-05-19 Decision]]` or `Update GitLab issue sked/alina#142: ACs revised per [[meeting]]`)
    - Include enough detail in the task that the PM or engineer can apply the external update
  - **If the story has no external ticket** (pre-creation drafts): vault is source of truth — edit directly

<!--
Entries 1-6 above are the universal living artifacts present in every pod vault.

Entries 7+ are project-specific. When a pod accumulates a domain-specific artifact
that gets updated across multiple ingests (e.g., a metrics glossary, an architecture
proposal, a requirements document), add a numbered entry below. The ingest skill
self-improves by auto-registering candidates it discovers (Step 7 of the skill).

Example shape for a project-specific entry — replace with your actual artifact:

### 7. <Artifact Name> (project-specific example)
- **Path pattern:** `projects/<project>/notes/<filename>.md`
- **What to reconcile:** [domain-specific update logic]
- **Threshold:** [when this gets updated vs. skipped]
- **Auto-registered:** YYYY-MM-DD (reason: [why this needed to be a registered artifact])
-->

### 7. Wiki Pages (Compiled Knowledge)
- **Path pattern:** `shared/wiki/*.md` (also `shared/wiki/index.md` as the navigation hub + Candidate Topics tracker)
- **What to reconcile (run on EVERY ingest, even if briefly):**
  - Scan ingested content for concepts. For each concept:
    - **Does an existing wiki page cover it?** If yes, check whether the ingest extends/contradicts/refreshes the page. Update with new source citation + any necessary status changes.
    - **Is the concept new and substantial enough for a page?** Promotion criteria: surfaced 2+ times in recent ingests, OR is a structural concept (governance pattern, architecture model, recurring constraint, key stakeholder relationship), OR has been referenced as something Jeff or Patrick will need to consult later. If yes → create the page.
    - **Borderline?** Add to the `## Candidate Topics` backlog at the top of `shared/wiki/index.md` with a one-line description + date + source pointer. Revisit when it surfaces a second time.
  - **Always update existing pages whose status materially changed:** decisions that got superseded, blockers that got resolved, scope that shifted.
  - **Force an explicit call on every ingest:** at the end of the ingest summary, state whether the ingest produced a new wiki page, extended an existing one, added a Candidate Topic, or genuinely surfaced nothing wiki-worthy. Don't allow silent skips.
- **Style requirements for new pages:**
  - One topic per page
  - ~100 lines / 2-minute read — synthesis over verbosity
  - Frontmatter: `type: wiki`, `topic`, `created`, `updated`, `sources:` array
  - Every claim cites a source file via `[[wikilink]]`
  - "See Also" section at bottom cross-linking related pages
  - Update `shared/wiki/index.md` to list the new page in its category (operational concepts / product concepts / infrastructure / patterns / project status)
- **Auto-registered:** 2026-05-19 (reason: ingests were running Step 3b but skipping Step 4. Wiki went stale for 3+ weeks while ~20 ingests happened. See [[feedback-wiki-synthesis]] in per-project memory for the precipitating incident.)
- **Lint cadence:** A periodic `vault-overhaul` skill audits the wiki + indexes; recommended weekly minimum during active project work.
