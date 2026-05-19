# Template Version

This vault was created from [`ChiroHD/pod-vault-template`](https://github.com/ChiroHD/pod-vault-template).

| Field | Value |
|-------|-------|
| Template commit | `{{TEMPLATE_COMMIT_HASH}}` |
| Created date | `{{CREATED_DATE}}` |
| Created by | `{{PM_NAME}}` |
| Pod | `{{POD_NAME}}` |
| First project | `{{PROJECT_NAME}}` ({{PROJECT_SLUG}}) |
| Product line | `{{PRODUCT_LINE}}` |

## Sync history

| Date | Template commit synced to | Notes |
|------|--------------------------|-------|
| {{CREATED_DATE}} | `{{TEMPLATE_COMMIT_HASH}}` | Initial creation |

When the template gets updated and you sync those updates into this vault (via `vault-overhaul --apply-template-updates` or manual cherry-pick), append a row here with the new template commit hash + date.

## What's tracked from the template

This vault's structure was bootstrapped from the template, but content has diverged. The following files are still "template-tracked" — when the template updates these, you may want to sync:

- `.claude/skills/ingest/skill.md` (universal sections only)
- `.claude/skills/ingest/living-artifacts.md` (universal entries — project-specific ones are local)
- `.claude/skills/vault-overhaul/skill.md`
- `CHEATSHEET.md`, `CLAUDE.md`, `SETUP.md`, `AGENTS.md` (structure)
- Memory files (`feedback_*.md` in `~/.claude/projects/.../memory/`)

The following are explicitly NOT template-tracked — they're this pod's local content:

- Anything in `projects/<your-project>/`
- Anything in `shared/wiki/` (you compile your own)
- People note Recent Updates blocks
- `log.md`, `INDEX.md`

Run `vault-overhaul --cross-vault-compare` to see structural drift between this vault and the template.
