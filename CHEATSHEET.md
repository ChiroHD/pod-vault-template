# Pod Vault `{{POD_NAME}}` — Cheat Sheet

Quick reference for {{PM_NAME}} + {{ENGINEER_NAME}}. For full details see `CLAUDE.md` and `SETUP.md`.

---

## Daily Git

```bash
# Start of day
cd ~/dev/chirohd/pod-vault-{{POD_SLUG}} && git pull

# End of day
git add -A
git commit -m "{{PM_NAME}}: <summary>"   # or "{{ENGINEER_NAME}}: <summary>"
git push
```

---

## Where Things Live

| What | Path |
|------|------|
| Project plan | `projects/{{PROJECT_SLUG}}/plan.md` |
| Project brief | `projects/{{PROJECT_SLUG}}/project-brief.md` |
| Task board | `projects/{{PROJECT_SLUG}}/tasks/tasks.md` |
| Decision records | `projects/{{PROJECT_SLUG}}/decisions/` |
| Meeting notes | `projects/{{PROJECT_SLUG}}/meetings/` |
| Raw transcripts | `projects/{{PROJECT_SLUG}}/raw/` |
| Research / spikes | `projects/{{PROJECT_SLUG}}/research/` |
| Product guide | `shared/reference/{{PRODUCT_LINE_SLUG}}/product-guide/` |
| Glossary | `shared/reference/{{PRODUCT_LINE_SLUG}}/<Product> Glossary.md` |
| People notes | `shared/people/` |
| Wiki (compiled knowledge) | `shared/wiki/` |
| Timeline | `log.md` |
| Unsorted stuff | `inbox/` |

---

## Three-Layer Tracking

| Layer | Tool | What Goes There |
|-------|------|-----------------|
| Roadmap | Monday | Milestone status for Dr. C |
| Engineering | JIRA ({{JIRA_EPIC}}) | Stories {{ENGINEER_NAME}} builds, tests, closes |
| Pod coordination | This vault | Plan, decisions, PM tasks, "who's doing what" |

**Rule of thumb:** If {{ENGINEER_NAME}} writes code for it, it's a JIRA story. Everything else lives here.

---

## Claude Code Prompts

### Everyday

| What you want | What to say |
|---------------|-------------|
| See current tasks | "What's on the task board?" |
| See the project plan | "Show me the project plan" |
| Check recent decisions | "What decisions have we made?" |
| Find a meeting note | "Find the meeting note from <date>" |
| Look up something product-specific | "What is <feature/term>?" |
| Look up a person | "Tell me about <Name>" |
| Search across the vault | "Search for <keyword>" |

### Ingesting Content

| What you want | What to say |
|---------------|-------------|
| Ingest pasted content | Paste it, then say "ingest this" |
| Ingest from inbox | "Ingest what's in the inbox" |
| Pull new content from personal vault | "Ingest from my personal vault" |
| Drop a raw file | Put it in `projects/{{PROJECT_SLUG}}/raw/` then "ingest this" |

The ingest pipeline auto-classifies, files, extracts decisions, reconciles all living artifacts, updates wiki, and logs. Transcripts are auto-redacted for personal/off-topic content. The pipeline asks clarifying questions when ambiguity is material (Confidence Protocol).

### Writing

| What you want | What to say |
|---------------|-------------|
| Write a decision record | "Write a decision record for <topic>" |
| Update the task board | "Mark <task> as complete" or "Add a task: <description>" |
| Add a log entry | "Log that I did X today" |
| Draft a stakeholder update | "Draft a status update for <stakeholder> on <project>" |

### Vault Maintenance

| What you want | What to say |
|---------------|-------------|
| **Run a periodic vault audit** (weekly minimum) | `/vault-overhaul` or "run a vault overhaul" |
| Find orphan wiki pages | "Are there any orphan wiki pages?" |
| Update wiki from new sources | "Update the wiki with recent meeting content" |
| Dry-run audit (report only, no changes) | "Run a vault overhaul dry-run" |
| Compare against sibling vaults | `/vault-overhaul --cross-vault-compare` |

**About `/vault-overhaul`:** The periodic safety net. Run weekly during active project work (or after every 5+ ingests). It mechanically refreshes index counts, dates, and Recent Activity tables; lints wiki cross-links; flags decisions that should be marked superseded; surfaces wiki candidates that have accumulated enough material to be promoted; compares against sibling pod vaults for cross-vault drift. Mechanical fixes auto-apply; judgment calls (new wiki pages, supersession statuses) surface for review. Full skill at `.claude/skills/vault-overhaul/skill.md`.

**About `/ingest`:** Per-content ingest (a single meeting, Slack thread, etc.). Different from overhaul — `ingest` adds new material; `overhaul` audits what's already there.

---

## Log Entry Format

Add to the top of `log.md` at end of day:

```markdown
## YYYY-MM-DD
- {{PM_NAME}}: Did X, decided Y, filed Z
- {{ENGINEER_NAME}}: Built A, investigated B
```

---

## Decision Record Template

File in `projects/{{PROJECT_SLUG}}/decisions/YYYY-MM-DD <summary>.md`:

```yaml
---
type: decision
date: YYYY-MM-DD
status: accepted
project: {{PROJECT_SLUG}}
decided_by: [names]
source: <meeting or context>
tags: [relevant-tags]
---
```

Sections: Decision, Context, Rationale, Consequences

---

## Pod Session Rhythm ({{POD_SESSION_DAYS}} {{POD_SESSION_TIME}})

1. **First 15 min** — `git pull`, check `log.md`, align on goals
2. **Middle 3.5 hours** — Build + coordinate
3. **Last 15 min** — Update `tasks.md`, commit and push

## Weekly Vault Hygiene

Run **once per week** (e.g., Friday afternoon or start of {{POD_SESSION_DAY_1}} session):

```
/vault-overhaul
```

Catches drift the per-ingest enforcement misses — stale index counts, frozen wiki pages, missing supersession statuses, broken cross-links. Mechanical fixes apply automatically; judgment calls surface for review. Skip if there's been < 5 ingests since the last overhaul.

---

## Key Links

| Resource | Location |
|----------|----------|
| JIRA Epic | {{JIRA_EPIC_URL}} |
| Monday Roadmap | {{MONDAY_ROADMAP_REFERENCE}} |
| Problem Brief (Notion) | {{NOTION_PROBLEM_BRIEF_URL}} |
| Slack Channel | {{SLACK_CHANNEL}} |

---

## Key People (Quick Ref)

| Person | Role | When to involve |
|--------|------|-----------------|
| Dr. C | Director of Product | Definitions, scope approval |
| Gabe Doty | Chief Strategist | Architecture decisions |
| Luke Doty | President | Executive stakeholder |
| Ryan Caskey | Head Dev | Code review, engineering alignment |
| Ryan Oakes | VP Payments | PayFac questions |
