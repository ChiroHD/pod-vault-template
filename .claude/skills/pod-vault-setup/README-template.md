# {{POD_NAME}} — Pod Vault

![Status: Active](https://img.shields.io/badge/status-active-{{POD_ACCENT_COLOR_NOHASH}}) ![Product line: {{PRODUCT_LINE}}](https://img.shields.io/badge/product%20line-{{PRODUCT_LINE}}-{{POD_ACCENT_COLOR_NOHASH}}) ![Theme: {{POD_THEME_NAME}}](https://img.shields.io/badge/theme-{{POD_THEME_NAME}}-{{POD_ACCENT_COLOR_NOHASH}})

Shared knowledge vault for the **{{POD_NAME}}** pod at Adio, working on **{{PROJECT_NAME}}** ({{PRODUCT_LINE}} product line).

> [!TIP]
> If you're looking for context on **what pod vaults are**, see [Pod Vault 101 on Notion](https://www.notion.so/365a343adff8815a9306e79f201cdc61) — concept overview, getting-started, and the full pod registry. This README focuses on this specific pod's context.

## Pod members

| Person | Role |
|--------|------|
| {{PM_NAME}} | {{PM_ROLE}} |
| {{ENGINEER_NAME}} | {{ENGINEER_ROLE}} |

## Project

**{{PROJECT_NAME}}** — {{PROJECT_ONE_LINE_DESCRIPTION}}

- **Engineering tickets ({{TICKET_SYSTEM}}):** [{{EPIC_REF}}]({{EPIC_URL}})
- **Code repo:** {{ENGINEER_CODE_REPO}}
- **Status:** Active

## What this vault is

A [Karpathy-style LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — a GitHub-synced markdown vault maintained by Claude Code that compounds project knowledge over time. Three layers:

| Layer | What lives here |
|-------|-----------------|
| **Operational** (current state) | `projects/{{PROJECT_SLUG}}/tasks/tasks.md`, `plan.md`, `project-brief.md`, people notes |
| **Compiled knowledge** (synthesis) | `shared/wiki/*.md` — one topic per page |
| **Raw** (history) | `meetings/`, `decisions/`, `raw/` transcripts, `log.md` |

Not a notes app — the pod's external memory. Humans rarely edit directly; they ingest content and ask questions.

## Working in this vault

If you're joining the pod, open this folder in Claude Code. `CLAUDE.md` tells Claude how the vault is organized.

**Daily flow:**
- `git pull`, scan `log.md` for the latest entries
- Drop content (meeting transcripts, Slack threads, decisions) into `inbox/` or `projects/{{PROJECT_SLUG}}/raw/`
- Ask Claude `/ingest` to file it, extract decisions, reconcile tasks/wiki, append to log
- `git commit && git push` end of session

**Weekly:** Run `/vault-overhaul` to keep indexes + wiki current. See [CHEATSHEET.md](CHEATSHEET.md) for the full quick reference.

## Sibling pods

This pod is one of several active Adio product pods. The [Pod Registry on Notion](https://www.notion.so/365a343adff8815a9306e79f201cdc61) is the source of truth for what pods exist.

{{SIBLING_PODS_LIST}}

## Reference

- [Pod Vault 101 on Notion](https://www.notion.so/365a343adff8815a9306e79f201cdc61) — concept + getting-started + registry
- [pod-vault-template](https://github.com/ChiroHD/pod-vault-template) — the GitHub template repo new pods are spun up from
- [Demo pod (worked example)](https://github.com/ChiroHD/pod-vault-template/tree/main/examples/demo-pod) — fully-populated reference vault
