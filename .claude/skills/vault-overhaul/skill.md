---
name: vault-overhaul
description: Periodic on-demand audit pass to keep the entire pod vault current and consistent. Complements the per-ingest enforcement (which prevents drift in real-time) by catching anything that slipped through, refreshing all index dates + counts, surfacing wiki candidates, linting cross-links, flagging stale claims, and (in deep mode) comparing against sibling pod vaults for cross-vault drift. Run weekly or whenever you suspect drift.
---

# Vault Overhaul Skill

Run a comprehensive audit pass over the entire pod vault. Find drift, fix mechanical issues automatically, surface judgment calls for the user.

This skill is the **periodic safety net** for the vault. The per-ingest enforcement (living-artifacts registry + transcript-capture rule + wiki-update rule) is the **continuous** layer — it prevents drift on each ingest. But judgment-call work can still get deferred, and silent drift (file counts wrong, stale dates, cross-links broken) can accumulate. This skill catches all of that.

## Trigger

User says "run a vault overhaul", "audit the vault", "vault refresh", or runs `/vault-overhaul`.

## When to Run

- **Weekly minimum** during active project work
- **On-demand** whenever the user suspects drift
- **Before major milestones** (e.g., before sending the vault to a new collaborator, before a stakeholder review)
- **After 5+ ingests** since the last overhaul

## Arguments

- No argument: full overhaul (all sections below). Cross-vault comparison runs in brief mode (file-existence asymmetries only).
- `--dry-run`: report findings only, don't make changes
- `--section <name>`: run just one section (e.g., `--section indexes`, `--section wiki-lint`, `--section cross-vault`)
- `--cross-vault-compare`: enable deep mode for Section 9 (full content diff against sibling vaults). Otherwise Section 9 only checks file-existence asymmetries.

## The Overhaul Pipeline

Run sections in order. Each section produces a brief findings report; mechanical fixes happen automatically, judgment calls are surfaced for the user.

### Section 1: Index + Count Verification

**Goal:** Indexes accurately reflect file counts and current state.

1. Read `INDEX.md`. Check `updated:` date.
2. For each project listed in INDEX.md, count actual files via shell:
   - `ls projects/<project>/meetings/ | wc -l`
   - `ls projects/<project>/decisions/ | wc -l`
   - `ls projects/<project>/raw/ | wc -l`
3. Compare against the counts shown in the project sub-index entry. Any mismatch → fix the count.
4. Read each project's `_index.md`. Cross-check that every file in `meetings/`, `decisions/`, `raw/` appears in the index tables.
5. Update `updated:` fields on INDEX.md + each `_index.md` to today's date if any change was made.
6. **Recent Activity table** in INDEX.md: refresh to show the actual last 5 ingests from `log.md`. Drop entries older than the 5-most-recent.

**Output:** "Indexes updated: X file counts corrected, Y missing entries added, Z dates refreshed."

### Section 2: Living Artifacts Reconciliation Check

**Goal:** Every registered living artifact reflects recent activity.

1. Read `.claude/skills/ingest/living-artifacts.md` registry.
2. For each registered artifact (tasks, people notes, decisions, plan, project-brief, jira-stories, glossary, wiki — and any auto-registered types):
   - Read the file
   - Check its `updated:` (or `last_updated:`) field
   - Compare against the last ingest date in `log.md`
   - If updated date is older than the most recent ingest that mentioned the artifact's domain, flag it for review
3. For `tasks.md` specifically: scan Active tables for tasks that should be closed based on `log.md` evidence since the last task update.

**Output:** "Living artifacts: X up-to-date, Y flagged for review."

### Section 3: Wiki Coverage + Lint

**Goal:** Wiki pages are current; new topics are captured; broken links are flagged.

1. Read `shared/wiki/index.md` and check the Candidate Topics list.
2. For each wiki page in `shared/wiki/`:
   - Check `updated:` date
   - Scan its `sources:` list — are all source files still present?
   - Grep its body for any `[[wikilink]]` that points to a non-existent page → flag as missing wiki page
3. Scan recent ingests (last 2 weeks of `log.md`) for:
   - Topics mentioned 2+ times that don't have a wiki page
   - Topics with a wiki page whose `updated:` is older than the most recent mentioning ingest
4. Check the Candidate Topics list: are any topics ready for promotion to a full wiki page (2+ surfaces + substance)?
5. **Mechanical fixes:** update `updated:` dates on pages that got source citations added. **Judgment calls:** surface candidates for new pages or promotion.

**Output:** "Wiki: X pages current, Y need refresh, Z candidate topics identified."

### Section 4: Decision Status Audit

**Goal:** Decisions that have been superseded are marked superseded; resolved questions don't show as open.

1. Read all decisions in `projects/*/decisions/`. Group by topic.
2. For each topic, find the most recent decision. Earlier decisions on the same topic should be marked `status: superseded` with a `superseded_by:` link.
3. Cross-check `tasks.md` Blocked + Awaiting items: any of them resolved per a recent decision but still showing as open?
4. Cross-check `project-brief.md` Open Questions: any resolved per a recent decision but still showing as open?

**Output:** "Decisions: X correctly marked, Y need supersession status, Z task/brief items stale."

### Section 5: Cross-Link Integrity

**Goal:** Every `[[wikilink]]` resolves.

1. Grep the vault for all `[[...]]` references.
2. For each, verify the target file exists (case-insensitive match in any vault subdirectory).
3. Surface broken links by source file.

**Output:** "Cross-links: X valid, Y broken (listed below)."

### Section 6: People Notes Freshness

**Goal:** People notes reflect recent observations.

1. For each file in `shared/people/`, check the `Recent Updates` section.
2. Scan recent ingests for mentions of that person (people/PersonName tag, or substantive paragraphs about them).
3. If the person was mentioned substantively in the last 7 days but no dated update block exists, flag for refresh.

**Output:** "People notes: X current, Y need new dated update block."

### Section 7: Log Sanity Check

**Goal:** `log.md` is being maintained.

1. Check that `log.md` has at least one entry per active project day (allowing for weekends + holidays).
2. Flag any gap > 3 business days without entries.
3. Verify the most recent entry references recently-modified files in the vault (mod times match log claims).

**Output:** "Log: X entries in last 2 weeks, Y gaps flagged."

### Section 8: Self-Improvement Scan

**Goal:** New living artifacts get registered; new conventions get captured.

1. Walk the vault for files that look like living artifacts but aren't in `.claude/skills/ingest/living-artifacts.md`.
2. Same scan as the ingest skill's Step 7, applied broadly.
3. Surface candidates for new memory entries (recurring user preferences observed but not memorialized).

**Output:** "Self-improvement: X new artifacts to register, Y memory candidates."

### Section 9: Cross-Vault Comparison

**Goal:** Detect structural drift between sibling pod vaults so improvements made in one vault propagate to the others.

**Why it matters:** Skills and memories evolve within a single vault as Claude learns from incidents (e.g., the 5/19 wiki backfill incident → `feedback_wiki_synthesis.md`). If pod-vault-A learns a new enforcement rule that pod-vault-B doesn't have, both vaults benefit when that improvement propagates. Without this comparison, vaults silently diverge.

**Behavior:**

- **Default (brief mode):** check for file existence asymmetries only — does each sibling vault have the same set of skills, memory files, and how-to docs? Fast, runs on every overhaul.
- **`--cross-vault-compare` flag (deep mode):** full content-level diff analysis. Slower; only run when the user explicitly asks.

**Procedure:**

1. **Discover sibling vaults.** Look for other vaults at `~/dev/chirohd/pod-vault-*`. Skip the current vault.
2. **For each sibling, compare these structural files:**
   - `.claude/skills/ingest/skill.md`
   - `.claude/skills/ingest/living-artifacts.md`
   - `.claude/skills/vault-overhaul/skill.md`
   - `CHEATSHEET.md`
   - `CLAUDE.md`
   - `SETUP.md`
   - `AGENTS.md`
   - `shared/wiki/index.md` (the "How to Use" + Candidate Topics structure, not the project-specific page listings)
   - Per-project memory `feedback_*.md` files at `~/.claude/projects/<encoded-cwd>/memory/`
   - `MEMORY.md` index in that same per-project memory directory
3. **For each compared file, classify differences:**
   - **Cosmetic / appropriate** — project name, person name, JIRA epic, file paths, project-specific examples. Ignore these.
   - **Structural** — sections, rules, patterns, or memory entries that exist in one vault but not the other. **Flag these.**
4. **Produce a report by direction:**
   - **✅ Aligned** — files with matching structure (just cosmetic diffs)
   - **⬅️ Sibling has but we don't** — structural improvements present in the sibling vault, candidates for backport into THIS vault
   - **➡️ We have but sibling doesn't** — improvements present here, candidates for forwarding to the sibling
   - **🟡 Diverged but both valid** — files where both vaults have made independent improvements that don't conflict
5. **Surface only — don't make changes automatically.** Cross-vault changes affect skill behavior; user should approve each direction. The user will run the overhaul on each sibling separately or invoke targeted backports.

**Heuristics for "cosmetic vs structural":**

- Project names ("TRP" / "PayFac"), people ("Patrick" / "Matt"), JIRA epics ("CHD-8366" / "CHD-8772"), paths (`projects/trp-export` / `projects/payfac-recurring-billing`) → cosmetic
- Rule wording differences that mean the same thing → cosmetic
- New sections, new bullet rules, new memory entries, new registry entries, new how-to mentions → structural
- Universal living-artifact entries (tasks, people notes, decisions, plan, project-brief, jira-stories, wiki) — diffs here matter and should propagate across vaults. Project-specific entries (varies per pod — e.g., a metrics glossary, architecture proposal, requirements doc) are usually appropriately divergent.

**Output:**

```
Cross-Vault Comparison — comparing against <sibling-vault-name>

✅ Aligned (X files):
- <list>

⬅️ <Sibling> has improvements we don't (Y items):
- <file>: <section/rule>
  Recommendation: backport via "<one-line ask>"

➡️ We have improvements <sibling> doesn't (Z items):
- <file>: <section/rule>
  Recommendation: forward via the cross-vault prompt pattern

🟡 Diverged but valid (W items):
- <file>: <description of independent improvements>
```

**Brief mode output** (when `--cross-vault-compare` NOT set): just the file-existence asymmetry line. Example:
```
Cross-vault brief: 2 sibling vaults found (pod-vault-jm). All structural files present in both. Run with --cross-vault-compare for deep analysis.
```

## Final Report

After all sections run, produce a single summary:

```
Vault Overhaul — YYYY-MM-DD

✅ Mechanical fixes applied: [count]
- Index counts corrected
- Updated dates refreshed
- Recent Activity table updated
- ...

⚠️ Surfaced for review: [count]
- Wiki candidates ready for promotion: [list]
- Decisions needing supersession status: [list]
- People notes needing fresh update blocks: [list]
- Broken cross-links: [list]

🔄 Next overhaul recommended: [date — typically 1 week out, or after N ingests]
```

Append a summary entry to `log.md`.

## Important Rules

- **Mechanical fixes happen automatically** — counts, dates, regrouping. The user doesn't need to approve each one.
- **Judgment calls get surfaced, not auto-resolved** — wiki page creation, supersession statuses on borderline decisions, people-note synthesis. The user decides.
- **Don't touch decision records** — they're append-only / never edited. Only their `status:` frontmatter field gets updated.
- **Don't touch meeting notes or raw transcripts** — also append-only / historical.
- **Don't edit `log.md` retroactively** — only append.
- **Commit + push** at the end of the overhaul, with a clear commit message describing what was fixed vs. surfaced.

## What This Skill Is NOT

- Not an ingest replacement. Use `/ingest` for new content.
- Not a wiki content generator. Surfaces candidates; doesn't write the pages (use the user's explicit ask for that — they may want a specific voice).
- Not a JIRA sync. Vault `jira-stories.md` is historical; JIRA is source of truth. The skill just verifies that historical record is appropriately labeled.

## Lint Mode (--lint inherited from ingest skill)

The ingest skill's `--lint` mode is now superseded by this skill's Section 3 (Wiki Coverage + Lint) and Section 5 (Cross-Link Integrity). Running this skill includes the lint pass.

## Why This Skill Exists

Origin story: 2026-05-19 audit revealed that despite operational artifacts being kept current via the living-artifacts registry, the wiki had been frozen for 3+ weeks. The fix was per-ingest enforcement (`feedback_wiki_synthesis.md` + adding wiki to the registry) — but the user also wanted a periodic safety net to catch anything that drifted past the enforcement. This skill is that safety net.

Memory: see `feedback_wiki_synthesis.md` and `feedback_ingest_transcripts.md` for the underlying enforcement rules this skill complements.
