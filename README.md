# Pod Vault — `{{POD_NAME}}` Pod, `{{PRODUCT_LINE}}`

Shared knowledge vault for the **{{POD_NAME}} pod** at Adio, working on **{{PROJECT_NAME}}**.

This is a [Karpathy-style LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — a GitHub-synced markdown vault maintained by Claude Code that compounds project knowledge over time. Not a notes app; it's the pod's external memory.

---

## What this vault is

Three layers of content, distinct purposes:

| Layer | What lives here | Examples |
|-------|-----------------|----------|
| **Operational** (current state) | Reconciled on every ingest | `projects/<name>/tasks/tasks.md`, `plan.md`, `project-brief.md`, people notes |
| **Compiled knowledge** (synthesis) | Refined over time as understanding deepens | `shared/wiki/*.md` — one topic per page |
| **Raw** (history) | Append-only sources | `raw/` transcripts, `meetings/`, `decisions/`, `log.md` |

Claude maintains all three layers. Humans rarely edit directly — they ingest content and ask questions.

## Getting started

### If you're the PM setting this vault up for the first time

If you just cloned this from `pod-vault-template`, run the setup skill before anything else:

```
/pod-vault-setup
```

It'll walk you through configuring this vault for your specific pod and project. Substitutes placeholders, picks the right reference layer for your product line, creates the GitHub repo, registers the pod in the Notion registry. ~5 minutes.

After setup, the next step is dropping your first content (meeting transcript, Slack thread, problem brief) into `inbox/` or `projects/<project>/raw/` and saying:

```
ingest this
```

### If you're joining an already-set-up pod

Open this folder in Claude Code. Claude reads `CLAUDE.md` automatically and understands the conventions.

**Daily flow:**
- Start of day: `git pull`, check `log.md` for partner's updates
- Capture content as it happens — drop transcripts in `raw/`, ask Claude to ingest
- End of day: `git add -A && git commit -m "..." && git push`

**Weekly:**
- Run `/vault-overhaul` to keep indexes + wiki current

Full quick reference in [CHEATSHEET.md](CHEATSHEET.md). New-engineer onboarding in [onboarding-engineer.md](onboarding-engineer.md).

## Key concepts

- **Ingest** (`/ingest`) — per-content workflow. Drop raw material → Claude classifies, files, extracts decisions, reconciles tasks + plan + people + wiki, updates log.
- **Vault overhaul** (`/vault-overhaul`) — periodic audit. Catches drift (stale counts, frozen wiki, broken cross-links). Run weekly during active work.
- **Living artifacts** — files that represent *current state* and get reconciled on every ingest. Listed in `.claude/skills/ingest/living-artifacts.md`.
- **Decisions** — append-only records. Each gets its own file in `decisions/` with rationale + consequences.
- **Wiki** — one topic per page, compiled from multiple sources, cross-linked, refined over time. The long-term-memory layer.

## What's in here right now

- **Skills:** `ingest`, `vault-overhaul`, `pod-vault-setup` (deletes itself after first run)
- **Reference content:** {{PRODUCT_LINE}} product guide, glossary, stakeholder map, operating principles
- **People notes:** base profiles for recurring Adio stakeholders (Dr. C, Gabe Doty, Luke Doty, Ryan Caskey, Ryan Oakes)
- **Project skeleton:** `projects/_template/` shows the standard project folder structure
- **Memory rules:** Three feedback memories codify enforcement of transcript capture, wiki updates, and ask-don't-assume

## What's NOT in here

- Project content — you'll ingest it
- Code — lives in {{ENGINEER_NAME}}'s code repo
- Personal journals — those stay in your personal vault
- Cross-pod content — that lives in the parent product's reference layer

## Where to go for more

- [CLAUDE.md](CLAUDE.md) — full instructions Claude reads when working in this vault
- [CHEATSHEET.md](CHEATSHEET.md) — quick-reference for daily flow
- [SETUP.md](SETUP.md) — setup instructions if you're configuring a new machine
- [examples/demo-pod/](examples/demo-pod/) — fully-populated reference implementation showing what a mature vault looks like
- [Pod Vault 101 on Notion](https://www.notion.so/Product-Pods-352a343adff88125bc55e0f930421f08) — conceptual overview + pod registry

---

*This vault was created from `pod-vault-template` on `{{CREATED_DATE}}`. See `TEMPLATE_VERSION.md` for the template commit hash + sync history.*
