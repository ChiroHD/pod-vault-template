---
type: template-reference
description: Catalog of all {{PLACEHOLDER}} tokens used in this template + what the setup skill substitutes them with.
audience: contributors to pod-vault-template, plus anyone debugging a half-substituted vault
---

# Template Placeholders

This template uses `{{PLACEHOLDER}}` tokens throughout its files. The `pod-vault-setup` skill substitutes them on first run after the PM clones the template into a new pod vault.

**If you're seeing this file in a live pod vault, the setup skill didn't finish.** Re-run `/pod-vault-setup` or report it.

## Token catalog

| Token | What it is | Example substitution | Used in |
|-------|------------|----------------------|---------|
| `{{POD_NAME}}` | Display name of the pod | `PJ`, `JM`, `Atlas` | README, CLAUDE, CHEATSHEET, CSS comments |
| `{{POD_SLUG}}` | Kebab-case slug for the pod (used in repo names, vault paths) | `pj`, `jm`, `atlas` | TEMPLATE_VERSION, CSS comments |
| `{{PRODUCT_LINE}}` | Which Adio product the pod works on | `ChiroHD`, `SKED` | README, CLAUDE, SETUP |
| `{{PROJECT_NAME}}` | Human-readable project name | `TRP Vital Signs Export`, `PayFac Recurring Billing` | README, CLAUDE, project-brief |
| `{{PROJECT_SLUG}}` | Kebab-case project slug for directory names | `trp-export`, `payfac-recurring-billing` | project paths, ingest skill, frontmatter |
| `{{PROJECT_KEYWORD}}` | Short keyword for scanning content | `TRP`, `PayFac` | ingest skill (cross-vault content matching) |
| `{{PM_NAME}}` | Product manager's first name | `Jeff`, `Riley` | CLAUDE, meeting note templates, memory templates |
| `{{ENGINEER_NAME}}` | Lead engineer's first name | `Patrick`, `Matt`, `Sam` | CLAUDE, meeting note templates, memory templates |
| `{{CREATED_DATE}}` | Date the vault was set up | `2026-05-14` | README footer, TEMPLATE_VERSION |
| `{{SESSION_CADENCE}}` | How often the pod meets | `Mon/Wed/Fri 09:30 ET`, `Tue/Thu mornings` | CLAUDE, project-brief |
| `{{SLACK_CHANNEL}}` | Pod's Slack channel | `#trp-vitalsigns`, `#payfac-recurring` | CHEATSHEET, ingest skill |
| `{{MONDAY_BOARD_URL}}` | Pod's Monday/JIRA board | full URL or "n/a" | CHEATSHEET |
| `{{POD_THEME_NAME}}` | Theme palette name | `Lavender`, `Forest`, `Crimson` | CSS snippet comments |
| `{{POD_ACCENT_COLOR}}` | Primary hex color | `#9b8ec4` | `.obsidian/snippets/pod-identity.css`, `appearance.json` |
| `{{POD_ACCENT_LIGHT}}` | Lighter accent variant | `#c2b8de` | pod-identity.css |
| `{{POD_ACCENT_SUBTLE}}` | Subtle background tint | `#ece6f5` | pod-identity.css |
| `{{POD_ACCENT_DARK}}` | Darker accent variant | `#6e5fa1` | pod-identity.css |
| `{{POD_TERMINAL_BG}}` | Terminal pane background | `#1a1530` | terminal-bladerunner.css |
| `{{SIBLING_POD}}` | Name of another pod for example output | `pod-vault-jm` | vault-overhaul skill example output |

## Substitution rules

- The setup skill performs a literal-string replacement across all template files
- Tokens are only substituted in **template files at setup time**. Once a vault is live, no further substitution happens — the setup skill self-archives.
- Reference content (`shared/reference/chirohd/`, `shared/people/*.md`) intentionally does **NOT** use placeholders. It's seed content for the new vault to extend.
- The `examples/demo-pod/` directory uses real fictional names (Riley Chen, Sam Patel, Atlas pod) — not placeholders. It's a worked example, not a template.

## Adding a new placeholder

If you're contributing to the template and need a new placeholder:

1. Add a row to the table above
2. Add a step to `.claude/skills/pod-vault-setup/skill.md` that asks the user for the value
3. Add the substitution to the apply phase of the setup skill
4. Use the token in the appropriate template files
5. If the token affects theming, also add a new palette entry to `color-palettes.json` if needed

Don't add placeholders for things that will rarely change — if 95% of pods use the same value, just hardcode it and let outlier pods edit after setup.
