---
name: ingest
description: Ingest raw content into the pod vault — auto-files, adds frontmatter, extracts decisions, reconciles all living artifacts from dynamic registry (tasks, people, decisions, plan, project-brief, engineering-stories + auto-discovered types), updates wiki and log. Published external tickets (JIRA or GitLab) create update tasks instead of editing the vault copy. Self-improves by auto-registering new living artifacts. Also supports cross-vault ingestion.
---

# Ingest Skill

Process raw content into the pod vault. Handles filing, naming, frontmatter, decision extraction, living artifact reconciliation, wiki updates, and logging — so the user never has to do any of that manually.

## Trigger

User says "ingest this", "ingest new content", "ingest from my personal vault", or runs `/ingest`.

## Arguments

- No argument or pasted content: process whatever is in `inbox/` or content pasted in the chat
- `--from-vault` or "from my personal vault": scan the PM's personal Adio vault for new project content and ingest it
- `--project <name>`: target a specific project (defaults to `{{PROJECT_SLUG}}`)
- `--lint`: instead of ingesting, check the wiki for contradictions, orphans, and stale claims

## Confidence Protocol (applies across all steps)

**Default to asking, not assuming.** If the source material is ambiguous, contradicts other sources, or requires inference to file correctly — stop and ask the user (or the person triggering the ingest) before proceeding. Don't capture assumptions as truth. See [[feedback-ingest-assumptions]] in per-project memory for the full rule.

### When to ASK (interactive, blocking)

If the ambiguity materially changes what gets filed:

- **Is this a real decision, or just a discussion item?** Affects whether a decision record gets created. Trigger: source doesn't say "decided / agreed / let's do X" but you're considering filing it as a decision.
- **Who is `decided_by`?** A single person, the pod, or a stakeholder? Trigger: the source uses "we" without naming people, OR you're attributing to a single name when the source is collective.
- **Does this statement supersede an earlier decision?** Trigger: a new claim contradicts an existing decision record in the vault.
- **Two sources contradict on a fact** — which is canonical? Trigger: a transcript says one thing, a Slack thread says another, or a meeting note disagrees with a decision record.
- **An action item has no owner.** Trigger: "we should X" with no name attached.
- **A status update is ambiguous** ("done" — shipped to prod? to dev? code-complete? validated?). Trigger: vague status word without context.
- **A new entity is mentioned but unclear if it's already in the vault under another name.** Trigger: "Northern Lakes Chiropractic" — is this Network 812 or a different network? Check before filing.
- **A file in the destination directory already exists for the same date and project.** Trigger: scanning `projects/<project>/meetings/` (or decisions/, etc.) before filing reveals a same-date file that might be a pre-meeting prep, an earlier session, or a different meeting on the same day. Don't silently create a parallel file. See Step 1c for the merge/create/replace prompt. Origin: 2026-05-20 incident in pod-vault-pj where a prep note and a transcript ingest produced two notes for the same working session.
- **A wiki page promotion is borderline.** Trigger: a topic surfaced once with minimal context — leave in Candidate Topics, don't create a half-baked page.

**How to phrase the question:** be specific about what's ambiguous (quote the source text), offer the candidate interpretations you considered, ask one focused question at a time.

### When to FLAG (non-blocking, file with caveat)

If the ambiguity is small and the ingest can proceed:

- File the content with the best available interpretation
- Add `confidence: low` or `needs_review: true` to the file frontmatter
- Include the item in the final ingest report under a "**Needs Review**" section
- Don't block the rest of the ingest

### When the user isn't available (async ingest)

- Default to FLAG behavior (file with `needs_review: true`)
- Make the "Needs Review" section of the ingest report prominent
- For material ambiguities (would-be decisions, contradictions between sources, conflicting attributions), add a task to `tasks.md` so the user catches it on next session

### Anti-patterns to avoid

These have all happened in past ingests:

1. **Inferring decisions from discussion** — "{{ENGINEER_NAME}} said X" ≠ "{{ENGINEER_NAME}} decided X"
2. **Filling in unstated dates** — "next week" stays as "next week," don't materialize a specific date
3. **Resolving conflicting sources silently** — file both, flag the conflict
4. **Attributing collective decisions to a single person** — "the pod decided" ≠ "{{ENGINEER_NAME}} decided"
5. **Promoting wiki Candidate Topics prematurely** — single surface ≠ wiki page
6. **Cross-linking to similar-but-different concepts** — verify the linked file matches the intended concept
7. **Adding inferred causation** — "X then Y" ≠ "X caused Y"

## Ingestion Pipeline

### Step 0: Source Priority (MANDATORY)

When ingesting a meeting, ALWAYS use these sources in this order:

1. **Primary: Raw transcript** — Find and read the full transcript from the PM's personal vault (`Meetings/Transcripts/`). Generate all pod vault content directly from what was actually said.
2. **Secondary: PM's manual notes** — Check the meeting note for anything the PM personally wrote. NOT the AI-generated sections between `<!-- AI_SUMMARY_START -->` / `<!-- AI_SUMMARY_END -->` markers, and NOT `<!-- PERSON_UPDATES_START -->` / `<!-- PREP_BRIEFING_START -->` sections. Manual notes in Discussion Notes, Decisions, Action Items, or other sections are valid input.
3. **Never use: AI-generated summaries** — The Fathom Summary, auto-generated meeting note content, and any AI-processed sections are derivatives. They may be inaccurate. The transcript is ground truth.

If the transcript cannot be found, ask the user where it is. Do not proceed with AI-generated content as a substitute.

### Step 1: Identify the content type

Read the raw content and classify it:

| Type | Signals | Destination |
|------|---------|-------------|
| Meeting transcript/notes | Date + attendees, discussion format, action items | `projects/<project>/meetings/` |
| Decision | "we decided", "the decision is", architecture choice language | `projects/<project>/decisions/` |
| Research/analysis | Comparative analysis, data, recommendations | `projects/<project>/research/` |
| Raw reference material | Articles, docs, screenshots, spreadsheet exports | `projects/<project>/raw/` |
| General/unclear | Anything else | `inbox/` (ask user to clarify) |

### Step 1b: Redact transcripts

If the content is a meeting transcript or raw audio recording transcript, redact all non-project content before filing. These transcripts often capture long-running in-person or Zoom sessions that include casual conversation, personal topics, and off-topic discussion.

**What to redact:**
- Personal conversation (family, health, weekend plans, food/drink, pets)
- Casual banter not related to the project
- Gossip or candid opinions about colleagues that could be embarrassing if shared
- Complaints about company leadership that aren't constructive project feedback
- Sensitive personal information (addresses, phone numbers, private messages)
- Inside jokes or references that could be misunderstood out of context
- Personal phone calls captured in the recording
- Social media references, personal text messages read aloud

**What to keep:**
- All project-relevant discussion, even if the language is blunt or informal
- Candid assessments of technical constraints, product decisions, or architecture — even when critical
- Constructive observations about organizational dynamics that affect the project
- Any content where a stakeholder provides requirements, decisions, or domain knowledge

**How to redact:**
- Replace each off-topic section with: `[redacted — off-topic]`
- Group adjacent off-topic lines into a single marker
- Preserve transitions so the remaining transcript reads coherently
- When in doubt, keep it — only redact content that is clearly personal or could be embarrassing

**Important:** The unredacted original stays in the PM's personal vault. Only the redacted version goes into `raw/` in the pod vault.

### Step 1c: Check for an existing file before creating a new one (MANDATORY)

**Why this exists:** before creating a new file in the destination directory, scan for an existing file that represents the same event. Origin: a 2026-05-20 incident in pod-vault-pj where a pre-meeting prep note already existed and a transcript ingest filed a parallel post-meeting note for the same working session — producing two notes for one meeting. The skill should detect and surface that overlap before filing.

**Procedure:**

1. **Compute the candidate destination path** — based on classified type + project. Example: a meeting on YYYY-MM-DD in the `{{PROJECT_SLUG}}` project → `projects/{{PROJECT_SLUG}}/meetings/`.
2. **Glob the destination directory for same-date files:** `YYYY-MM-DD *.md` matching the content's date.
3. **For each match, assess overlap:**
   - **Strong signal — file matches:** same date AND same project AND any of (a) attendees overlap in frontmatter, (b) substantive title-word overlap excluding stop words and date, (c) explicit reference to the same calendar event / Fathom call / Zoom ID, (d) the same source transcript / external link.
   - **Weak signal — possibly different:** same date but different meeting (e.g., a pod working session + a separate stakeholder 1-on-1 on the same day). Don't merge.
4. **If a strong-signal match exists, STOP and ASK before filing:**
   ```
   Found existing file: <path>
   It looks like the same event as what you're asking me to ingest because <evidence>.
   
   Options:
     1. Merge — preserve the existing file's content (likely prep / agenda) and add today's recap + decisions + action items into it
     2. Create a new file alongside — they're actually different events
     3. Replace — the existing file is obsolete; overwrite with the new content
   
   Which?
   ```
5. **Special case — pre-meeting prep vs. post-meeting recap.** The most common form of overlap. The existing file usually contains agenda / topics / pre-meeting task lists, while the new content contains decisions / discussion / outcomes. Default merge structure: post-meeting Summary + Decisions + Action Items + Key Discussion Points lead the file; the existing prep + agenda gets preserved as a "Pre-Meeting Prep / Agenda" section at the bottom, with `~~strikethrough~~` markup on any task-list items that the meeting itself superseded.
6. **If no match exists, proceed to Step 2.**

**What this DOES NOT do:**
- Doesn't catch every form of overlap (e.g., a meeting filed on Day N about Day N+1 events). The same-date heuristic is the strongest signal; weaker dates are out of scope.
- Doesn't auto-merge — always asks first. Merge is a content decision; only the user knows whether two notes represent the same event or distinct ones.

### Step 2: Create the filed version

1. **Name the file:** `YYYY-MM-DD <descriptive title>.md` using today's date or the date found in the content. If Step 1c surfaced an existing file with a different title (e.g., old pod name vs. new pod name), keep the existing filename when merging unless the user opts otherwise — preserving stable inbound references.
2. **Add frontmatter:**
```yaml
---
type: <meeting|decision|research|raw>
date: YYYY-MM-DD
project: <project-name>
source: <where this came from — "inbox", "personal-vault", "pasted", filename>
tags:
  - type/<type>
  - project/<project>
---
```
3. **File it** in the correct project subfolder
4. If the content is a meeting transcript, also extract a clean summary at the top with: attendees, key topics, decisions made, action items

### Step 3: Extract decisions

Scan the content for decision language. For each decision found:
1. Create a separate file in `projects/<project>/decisions/`
2. Name: `YYYY-MM-DD <decision summary>.md`
3. Include: context, the decision, rationale, who decided, consequences
4. Cross-link back to the source: `Source: [[YYYY-MM-DD Meeting Title]]`

### Step 3b: Reconcile living artifacts

This is the critical cross-artifact sweep. Before updating the wiki, read every living artifact in the vault and update it based on what happened in the ingested content. "Living artifacts" are files that represent current state and must stay accurate over time — they are not append-only like meeting notes or decision records.

**How this step works:**

1. **Read the registry:** Load `.claude/skills/ingest/living-artifacts.md` — this is the single source of truth for what gets reconciled. Do NOT use a hardcoded list.
2. **For each registered artifact type:** read the file(s) matching the path pattern, then apply the reconciliation rules defined in the registry entry.
3. **Ticket-system lifecycle check:** For `engineering-stories.md` specifically (legacy: `jira-stories.md`), the registry defines a lifecycle rule. Read the file's `ticket_system:` frontmatter (`jira`, `gitlab`, or `jira+gitlab`), then check whether each story has a ticket reference in that system before editing. If it does, the external system is the source of truth: create a task in tasks.md describing what needs updating rather than editing the vault copy. Pre-creation drafts (no external ticket) can be edited directly.

**General reconciliation guidance:**
- Read each living artifact file before modifying it
- For tasks.md: scan every row in Active tables and compare against meeting content — could this task have been completed or advanced? Then scan meeting content for new commitments not yet in the table.
- For people notes: only update if there's substantive new information, not just "they attended a meeting"
- For decisions: grep the ingested content for references to existing decision topics

**This step is mandatory, not optional.** The whole point of ingestion is that the vault reflects current reality after every ingest. A meeting note sitting in `meetings/` while `tasks.md` still shows stale status is a process failure.

### Step 4: Update the wiki

For each significant topic, concept, or entity mentioned in the content:

1. Check if a wiki page exists in `shared/wiki/` for that topic
2. **If yes:** Update the page with new information, add the source citation
3. **If no:** Create a new page with:
```yaml
---
type: wiki
topic: <topic name>
created: YYYY-MM-DD
sources: [<list of source files>]
---
```
4. Add cross-links: `See also: [[Related Page]]`
5. Update `shared/wiki/index.md` with any new pages

**Wiki page guidelines:**
- One page per concept/entity/topic — not per source document
- Pages compile information across multiple sources
- Every claim cites its source file
- Pages link to related pages via wikilinks
- Keep pages concise — aim for a page someone can read in 2 minutes

### Step 4b: Update indexes

Update the vault's two-tier index system to reflect any new content created during this ingest.

1. **Project `_index.md`:** Read `projects/<project>/_index.md`. Add rows for any new meetings, decisions, or raw sources created in Steps 2-3. Each row needs: file link, date, and a one-line key outcome or description. Keep tables sorted by date (oldest first).
2. **Root `INDEX.md`:** Read `INDEX.md`. Update the "Recent Activity" table — replace the oldest entry with a new row describing this ingest (date, what was ingested, key outcome). Keep the table at 5 entries max. Also update the Projects table's Sub-Index column with current counts (e.g., "3 meetings, 11 decisions, 3 raw sources"). If a new wiki page was created in Step 4, add it to the Wiki table.
3. **Update the `updated` field** in the frontmatter of both files to today's date.

### Step 5: Update log.md

Append an entry to the TOP of `log.md`:
```markdown
## YYYY-MM-DD
- <user>: Ingested <description> — filed as <filename>, extracted N decisions, updated N wiki pages
```

### Step 6: Report back

Tell the user what was ingested:
- Files created (with paths)
- Decisions extracted (new and updated)
- Living artifacts reconciled — specifically list: tasks marked complete, tasks added, people notes updated, decisions updated/superseded, plan changes
- Wiki pages created or updated
- Any items that need user clarification

### Step 7: Self-improvement scan

After the ingest is complete, scan the vault for files that look like living artifacts but aren't in the registry. This keeps the registry growing organically as the vault evolves.

**How to scan:**
1. Read `.claude/skills/ingest/living-artifacts.md` to get the current registered path patterns
2. Walk the `projects/<project>/` and `shared/` directories for markdown files that:
   - Are NOT in append-only directories (meetings/, raw/, research/ are excluded)
   - Represent current state rather than point-in-time records (e.g., a `roadmap.md`, `risks.md`, `dependencies.md`, `architecture.md`)
   - Are not already matched by an existing registry path pattern
3. For each candidate found:
   - Auto-register it: append a new numbered section to `living-artifacts.md` with the path pattern and reasonable reconciliation rules inferred from the file's content and structure
   - Update `last_updated` in the registry frontmatter
4. Report any new registrations in the Step 6 summary: "Registered new living artifact: [path] — [what it tracks]"

**What NOT to register:**
- Index files (`index.md`, `wiki/index.md`) — these are navigation, not state
- Log files (`log.md`) — append-only
- The registry file itself
- Files under 5 lines (stubs, placeholders)
- Files in `raw/`, `meetings/`, `research/` directories

## Cross-Vault Ingestion (--from-vault)

When the user asks to ingest from their personal vault:

### Source locations to scan

Scan the PM's personal Adio vault. Claude resolves the vault location from the operating environment — do not hardcode.

Scan these subpaths for project-related content (replace `{{PROJECT_KEYWORD}}` with the actual keyword for your project, e.g., "TRP" or "PayFac"):

- `Meetings/` — files with `{{PROJECT_KEYWORD}}` in the name
- `Strategy/Decisions/` — files with `{{PROJECT_KEYWORD}}` in the name
- `Projects/Q2 Roadmap/` — files with `{{PROJECT_KEYWORD}}` in the name
- Any file tagged with `project/Q2Roadmap/{{PROJECT_TAG}}`

### Deduplication

Before copying, check if the file already exists in the pod vault:
1. Compare filenames — if exact match exists, check modification dates
2. If the personal vault version is newer, replace the pod vault version
3. If the pod vault already has it and it hasn't changed, skip it
4. Report: "Found N new files, M updated files, K already up to date"

### Mapping
| Personal vault location | Pod vault location |
|------------------------|-------------------|
| `Meetings/*{{PROJECT_KEYWORD}}*` | `projects/{{PROJECT_SLUG}}/meetings/` |
| `Strategy/Decisions/*{{PROJECT_KEYWORD}}*` | `projects/{{PROJECT_SLUG}}/decisions/` |
| `Projects/Q2 Roadmap/{{PROJECT_KEYWORD}}*` | `projects/{{PROJECT_SLUG}}/` |
| `Reference/ChiroHD Product Guide/` | `shared/reference/product-guide/` |
| `Reference/ChiroHD Academy/ChiroHD Glossary.md` | `shared/reference/ChiroHD Glossary.md` |
| `Reference/Knowledge/` | `shared/knowledge/` |

After copying new/updated files, run them through the full ingestion pipeline (Steps 1-6) to update the wiki.

## Lint Mode (--lint)

When the user asks to lint the wiki:

1. Read every page in `shared/wiki/`
2. Check for:
   - **Contradictions:** Two pages making conflicting claims about the same topic
   - **Orphans:** Pages not linked from any other page or the index
   - **Stale claims:** Information citing sources older than 30 days without recent confirmation
   - **Missing pages:** Topics referenced via wikilink that don't have a page yet
   - **Index gaps:** Pages that exist but aren't listed in `shared/wiki/index.md`
3. Report findings and suggest fixes
4. Offer to auto-fix: update index, create stub pages for missing topics, flag contradictions for user review

## Important Rules

- Never modify files in the personal vault — only read from it
- Always preserve existing content when updating wiki pages — append, don't overwrite
- Always cite sources in wiki pages
- **Default to asking, not assuming.** If the source is ambiguous, contradicts another source, or requires inference, follow the Confidence Protocol above — ASK for blocking ambiguities, FLAG with `needs_review: true` for non-blocking ones. Never silently capture an assumption as truth.
- Keep wiki pages focused — one topic per page, not mega-pages
- Update `shared/wiki/index.md` every time you create or rename a wiki page
