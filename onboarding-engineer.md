# Welcome to the {{POD_NAME}} Pod Vault — {{ENGINEER_NAME}}

Hey {{ENGINEER_NAME}}. This is the shared knowledge vault for our pod — Jeff has set it up so we have one place for everything that's NOT code. Read this note first.

## What this is

A Git repo that doubles as an Obsidian vault and a Claude Code workspace. Source code lives in your code repo. Product context, decisions, meeting notes, metric specs, the project plan, task tracking, and compiled knowledge live here.

**Three things at once:**
1. **A GitHub repo** — we stay in sync with git pull / git push
2. **An Obsidian vault** — you can browse, search, edit with wikilinks (optional but nice)
3. **A Claude Code workspace** — Claude reads CLAUDE.md and understands our entire project context when you open this folder

## Why this exists

Because context was fragmented before. Product decisions lived in someone's head. Meeting outcomes lived in transcripts nobody re-read. Specs lived in Google Docs. Task status lived in Slack threads that disappeared after a few days.

Now: one shared folder, both of us and Claude can read everything, Git tracks every change.

The other reason is the **Karpathy LLM Wiki pattern** — drop raw source material into the vault, Claude processes it into structured cross-linked wiki pages, knowledge compounds over time instead of being re-derived from scratch every conversation.

## What's already in here

- **Skills** — ingest workflow, vault audit, the setup skill (already run)
- **Reference content** — {{PRODUCT_LINE}} product guide, glossary, stakeholder map
- **People notes** — base profiles for Dr. C, Gabe, Luke, Ryan Oakes, Ryan Caskey
- **Project skeleton** — `projects/{{PROJECT_SLUG}}/` with empty structure ready for content
- **Memory rules** — three feedback memories codifying transcript capture, wiki sync, and ask-don't-assume

**Start with these files:**
1. `projects/{{PROJECT_SLUG}}/project-brief.md` — what we're building + why
2. `projects/{{PROJECT_SLUG}}/plan.md` — phased plan with timeline
3. `projects/{{PROJECT_SLUG}}/tasks/tasks.md` — what each of us is doing this week
4. `CHEATSHEET.md` — quick reference for daily flow

The decision records in `projects/{{PROJECT_SLUG}}/decisions/` capture every architecture decision we've made with rationale and history.

## How to set up your local

See [SETUP.md](SETUP.md) for the full setup. Short version:

```bash
cd ~/dev/chirohd
git clone git@github.com:ChiroHD/pod-vault-{{POD_SLUG}}.git
cd pod-vault-{{POD_SLUG}}
claude
```

## How we track work — three layers

| Layer | Tool | What Lives Here | Rule |
|-------|------|-----------------|------|
| Roadmap | Monday | Milestone status for Dr. C | Jeff updates weekly |
| Engineering backlog | JIRA ({{JIRA_EPIC}}) | Stories you pick up, build, test, close | If you write code for it, it's a JIRA story |
| Pod coordination | This vault | Project plan, decisions, PM tasks, "who's doing what" | If it's not code, it lives here |

**JIRA stories should link back to this vault.** Each story description should include a "Pod Vault Context" section with paths to relevant decision records and meeting notes. Then you can tell Claude Code in your code repo: "Read CHD-XXXX and the linked decision records, then implement."

## Daily rhythm

**Daily:** Pull at start of day. Capture decisions/notes/tasks as they happen. Commit + push end of day.

**Weekly:** Mon/Wed (or whatever the pod cadence is) we have 4-hour working sessions. Pull at the start, work together, commit + push at the end.

**End of day log entry:**

```markdown
## YYYY-MM-DD
- {{ENGINEER_NAME}}: Built X, investigated Y, found Z
```

Or just tell Claude Code: "log that I did X today."

## How Claude Code works in this vault

When you open Claude Code in this folder, it reads `CLAUDE.md` and knows the folder structure, your role, the project context, how to ingest content, where things live.

**Things you can ask Claude:**

- "What decisions have we made about X?"
- "What's my task list?"
- "Ingest this" (with pasted content)
- "Write a decision record for [topic]"
- "What does the project brief say about Y?"
- "Find the meeting note from [date]"

For the full set, see [CHEATSHEET.md](CHEATSHEET.md).

## What I need from you in your first week

1. **Set up local clone + Claude Code** — see SETUP.md
2. **Read `projects/{{PROJECT_SLUG}}/project-brief.md`** — orient on the project
3. **Read `projects/{{PROJECT_SLUG}}/plan.md`** — orient on the timeline
4. **Review JIRA epic {{JIRA_EPIC}}** — see what's already created
5. **Tell me your code repo URL** — I need it for CLAUDE.md so Claude can cross-reference

## What you should capture going forward

| When | What to capture | Where |
|------|----------------|-------|
| You make an architecture decision | Why you chose approach X over Y | `decisions/` — name it `YYYY-MM-DD <summary>.md` |
| You hit a blocker | What happened, what it affects | Note in `tasks/tasks.md` or new file in `research/` |
| You finish a task or story | Mark it done | Update `tasks/tasks.md` |
| You discover something about the codebase | A constraint, gotcha, or pattern others should know | Drop a note in `inbox/` and tell Claude "ingest this" |
| You finish a spike | Findings, recommendation, effort estimate | Decision record in `decisions/` |

**Shortcut:** If you don't want to think about where something goes or what to name it, drop it in `inbox/` and tell Claude "ingest this." Claude handles naming, frontmatter, filing, and cross-linking.

## Questions?

Slack me or drop a question in `inbox/` and I'll see it on my next pull. If something looks wrong — a meeting note that misrepresents what we discussed, a decision record with incorrect rationale, an estimate that's off — edit it directly or flag it. This is our shared source of truth.

— {{PM_NAME}}
