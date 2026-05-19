# Pod Vault `{{POD_NAME}}` — Setup Guide

For setting up a new local clone of this vault, or for the first-time setup if you just created this vault from `pod-vault-template`.

---

## If you just cloned from `pod-vault-template` (first-time pod setup)

Open this folder in Claude Code and say:

```
/pod-vault-setup
```

The setup skill will walk you through configuring this vault for your pod + project. ~5 minutes. After that, the rest of this doc applies.

---

## Step 1: Prerequisites

You need:

- Git (with SSH access to the ChiroHD GitHub org)
- Claude Code (`claude` CLI installed)
- Obsidian (optional, for visual browsing of the vault)
- `gh` CLI (for vault-overhaul + skill operations)
- Notion MCP configured (for pod registry updates)

## Step 2: Clone the vault

```bash
cd ~/dev/chirohd
git clone git@github.com:ChiroHD/pod-vault-{{POD_SLUG}}.git
```

## Step 3: Open in Claude Code

```bash
cd pod-vault-{{POD_SLUG}}
claude
```

Claude Code reads `CLAUDE.md` automatically and understands the vault structure, conventions, and your role.

## Step 4: Daily Workflow

**Start of day:**
```bash
cd ~/dev/chirohd/pod-vault-{{POD_SLUG}} && git pull
```

**During work:**
- Capture decisions, notes, and task updates as you go
- Drop raw sources (transcripts, screenshots, docs) into `projects/{{PROJECT_SLUG}}/raw/`
- Ask Claude to ingest raw sources into the wiki

**End of day:**
```bash
cd ~/dev/chirohd/pod-vault-{{POD_SLUG}}
git add -A && git commit -m "your summary" && git push
```

**Check partner's updates:**
- Read `log.md` (most recent at top)
- Check `projects/{{PROJECT_SLUG}}/tasks/tasks.md` for task status changes

**Weekly vault hygiene:**
- Once per week (or after 5+ ingests), run `/vault-overhaul` to refresh indexes, lint the wiki, and surface drift. See [CHEATSHEET.md](CHEATSHEET.md) for details.

## Folder Structure

```
pod-vault-{{POD_SLUG}}/
├── CLAUDE.md              # Claude Code instructions (read this first)
├── AGENTS.md              # Cross-tool agent instructions
├── README.md              # PM-facing overview + getting started
├── CHEATSHEET.md          # Quick reference for daily flow
├── SETUP.md               # This file
├── log.md                 # Append-only pod timeline
├── INDEX.md               # Master catalog
├── TEMPLATE_VERSION.md    # Records template commit hash + creation date
│
├── shared/                # Cross-project knowledge
│   ├── reference/         # Product guide, glossary, deep dives
│   ├── people/            # Stakeholder dossiers
│   ├── knowledge/         # Learnings system (hypotheses > rules)
│   └── wiki/              # Compiled knowledge pages
│
├── projects/
│   ├── _template/         # Skeleton for new projects in this pod
│   └── {{PROJECT_SLUG}}/  # First project
│       ├── raw/           # Source material (transcripts, docs)
│       ├── decisions/     # Architecture Decision Records
│       ├── meetings/      # Meeting notes
│       ├── tasks/         # Task board
│       ├── notes/         # Working notes
│       └── research/      # Analysis, spikes, deep dives
│
├── meetings/              # Pod-level (cross-project) meetings
├── inbox/                 # Dump stuff here when unsure
└── .claude/
    └── skills/            # Pod skills (ingest, vault-overhaul, pod-vault-setup)
```

## Per-Project Memory

Memory files for this vault live at:

```
~/.claude/projects/-Users-<you>-dev-chirohd-pod-vault-{{POD_SLUG}}/memory/
├── MEMORY.md
├── feedback_ingest_transcripts.md
├── feedback_wiki_synthesis.md
└── feedback_ingest_assumptions.md
```

These get auto-loaded when Claude starts a session in this vault. The setup skill bootstraps them on first run.

## Troubleshooting

- **Claude doesn't see the skills** — make sure you're in the vault directory when you start Claude
- **Ingest isn't picking up transcripts** — confirm your personal Adio vault path; the ingest skill resolves it from environment
- **`/vault-overhaul` reports drift** — that's the skill working. Mechanical fixes apply automatically; review the surfaced judgment calls.

For deeper guidance, ask Claude — it has access to all the skill docs and feedback memories.
