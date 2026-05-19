# Pod Vault `{{POD_NAME}}` — Claude Code Instructions

**Start here:** Read `INDEX.md` first — it's the master catalog of vault content (projects, people, wiki, recent activity). For project-specific content, follow the link to the project's `_index.md`.

Shared knowledge vault for the **{{POD_NAME}} pod** at Adio, working on **{{PROJECT_NAME}}** (product line: **{{PRODUCT_LINE}}**).

## Pod Members

| Person | Role | GitHub |
|--------|------|--------|
| {{PM_NAME}} | {{PM_ROLE}} | {{PM_GITHUB}} |
| {{ENGINEER_NAME}} | {{ENGINEER_ROLE}} | {{ENGINEER_GITHUB}} |

## Primary Artifacts

| Artifact | Link |
|----------|------|
| Problem Brief (Notion) | {{NOTION_PROBLEM_BRIEF_URL}} |
| Engineering Epic ({{TICKET_SYSTEM}}) | {{EPIC_URL}} |
| Monday Roadmap | {{MONDAY_ROADMAP_REFERENCE}} |
| Slack Channel | {{SLACK_CHANNEL}} |

## What This Is

A shared Obsidian vault and Claude Code workspace for everything that is NOT code. Product context, decisions, meeting notes, specs, task coordination, and compiled knowledge live here. Source code lives in {{ENGINEER_NAME}}'s code repo.

## Sibling Repos

| Repo | Purpose | Who manages |
|------|---------|-------------|
| `ChiroHD/pod-vault-{{POD_SLUG}}` | This vault — knowledge, product, coordination | {{PM_NAME}} + {{ENGINEER_NAME}} |
| {{ENGINEER_CODE_REPO}} | {{PROJECT_NAME}} feature code | {{ENGINEER_NAME}} |

When you need code context, read from the sibling code repo. When you need product context, read from this vault.

## File Organization

```
pod-vault-{{POD_SLUG}}/
├── shared/                    # Cross-project knowledge
│   ├── reference/             # Product guide, glossary, deep dives, etc.
│   ├── people/                # Stakeholder dossiers (curated for pod)
│   ├── knowledge/             # Hypothesis -> rules system
│   └── wiki/                  # Karpathy-style compiled knowledge pages
├── projects/
│   ├── _template/             # Skeleton for new projects in this pod
│   └── {{PROJECT_SLUG}}/      # First project
│       ├── raw/               # Source docs, transcripts, screenshots
│       ├── decisions/         # ADRs for this project
│       ├── meetings/          # Project-specific meeting notes
│       ├── tasks/             # Pod task board (markdown)
│       ├── notes/             # Working notes, validation plans, etc.
│       └── research/          # Analysis, spikes, deep dives
├── meetings/                  # Pod-level meetings (not project-specific)
├── inbox/                     # Triage point for unsorted content
├── log.md                     # Append-only timeline
└── .claude/
    └── skills/                # Pod skills (ingest, vault-overhaul, pod-vault-setup)
```

## Conventions

- Wikilinks for cross-references: `[[note name]]`
- Frontmatter on every note (type, date, tags at minimum)
- Decision records named: `YYYY-MM-DD <decision summary>.md`
- Meeting notes named: `YYYY-MM-DD <Meeting Title>.md`
- Tasks tracked in `projects/<project>/tasks/tasks.md`
- Timeline tracked in `log.md` (append-only, most recent at top)

## Content Boundaries

| Content | This Vault | Code Repo | PM's Personal Vault |
|---------|-----------|-----------|----------------------|
| Source code | Never | Yes | Never |
| Architecture decisions | Yes (canonical) | Reference by link | Mirror |
| PRDs & problem briefs | Yes (canonical) | Never | Mirror |
| Meeting notes (pod) | Yes (canonical) | Never | Never |
| Meeting notes (PM's other) | Never | Never | Yes |
| Product guide / glossary | Yes (synced from personal) | Never | Canonical |
| Stakeholder notes | Curated subset | Never | Full version |
| Task coordination | Yes | Never | Never |
| Personal journal | Never | Never | Yes |

## Working With Raw Sources (Karpathy Pattern)

Drop raw source material into `projects/<project>/raw/`:
- Meeting transcripts
- Slack thread exports
- Competitor screenshots
- Support ticket summaries
- Spreadsheet exports

Then ask Claude to **ingest** it: "Ingest this" or run `/ingest`. Claude will:
1. Classify the content (meeting, decision, research, raw reference)
2. Name, add frontmatter, and file it in the correct project subfolder
3. Extract any decisions into separate decision records
4. Reconcile all living artifacts (tasks, plan, people notes, etc.)
5. Update structured wiki pages in `shared/wiki/`
6. Cross-link with existing wiki pages
7. Update `shared/wiki/index.md` + project + root indexes
8. Append to `log.md`

Users can also drop content into `inbox/` and Claude will classify and route it.

### Cross-Vault Ingestion

The PM can say "ingest new {{PROJECT_KEYWORD}} content from my personal vault" to pull new/updated project files from the personal Adio vault into the pod vault. Claude scans for new files, deduplicates, copies, and runs the full ingestion pipeline. See `.claude/skills/ingest/skill.md` for details.

### Periodic Vault Overhaul

Separate from ingest: a periodic audit pass that catches drift the per-ingest enforcement misses. Say "run a vault overhaul", "audit the vault", or `/vault-overhaul`. Claude will:

1. Verify INDEX.md + project `_index.md` file counts match actual files; refresh Recent Activity tables
2. Check each registered living artifact's freshness vs. recent activity
3. Lint the wiki — broken cross-links, stale pages, candidate topics ready for promotion
4. Audit decisions for missing supersession statuses
5. Verify all `[[wikilinks]]` resolve
6. Flag people notes that need fresh update blocks
7. Sanity-check log.md continuity
8. Scan for new living artifacts to auto-register
9. Compare against sibling pod vaults (deep mode: --cross-vault-compare)

Mechanical fixes (counts, dates, regrouping) apply automatically. Judgment calls surface for review. Recommended cadence: **weekly during active project work**.

## Task Tracking

Pod tasks live in `projects/<project>/tasks/tasks.md`. Format:

```markdown
| Task | Owner | Status | Due | Ticket / Monday |
|------|-------|--------|-----|------------------|
| Example task | {{PM_NAME}} | In Progress | YYYY-MM-DD | [{{EPIC_REF}}-XX]({{EPIC_URL}}) |
```

Engineering tickets live in `{{TICKET_SYSTEM}}` ({{EPIC_URL}}). Monday tracks company-level roadmap. This vault is for pod-level coordination — anything that isn't a discrete engineering ticket or a roadmap-level rollup.

For SKED pods using `jira+gitlab` (split tracking), it's fine to reference either system in the Ticket column — the URL itself disambiguates.

## Daily Rhythm

**Morning:** Pull latest (`git pull`), check `log.md` for partner's updates
**During work:** Capture decisions, notes, task updates as you go
**End of day:** Commit and push. Add a line to `log.md` summarizing what you did.

## Weekly

- Review the week's commits together
- PM syncs any new reference content from personal vault
- Update project tasks and close completed items
- Run `/vault-overhaul` for periodic audit

## Company Context

Adio Software is the parent company. Products under Adio include:

- **ChiroHD** — chiropractic EHR platform (PE-backed by Mainsail Partners, 5,000+ practices). Products: EHR, practice management, scheduling, billing, patient engagement.
- **SKED** — separate product line under Adio.

This pod works on **{{PRODUCT_LINE}}**. Project context lives in `projects/{{PROJECT_SLUG}}/project-brief.md`.

## Key Stakeholders

| Person | Role | Relevance |
|--------|------|-----------|
| Dr. Cristina Esposito | Director of Product | PM's manager, project approver |
| Gabe Doty | Chief Strategist / Founder | Architecture authority |
| Luke Doty | President | Executive stakeholder |
| Ryan Caskey | Head Developer | Engineering lead |
| Ryan Oakes | VP Payments | PayFac stakeholder |

Full dossiers in `shared/people/`. The pod adds dated update blocks to these as it observes the stakeholders in pod-relevant contexts.
