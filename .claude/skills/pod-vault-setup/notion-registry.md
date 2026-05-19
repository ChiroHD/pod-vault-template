---
type: setup-config
description: Coordinates of the Notion Pod Registry database. Used by the pod-vault-setup skill to append a row when a new pod is created.
---

# Pod Registry — Notion Coordinates

## Where it lives

- **Parent page (Pod Vault 101):** https://www.notion.so/365a343adff8815a9306e79f201cdc61
- **Database:** https://www.notion.so/8ea0da071b7345a395c9d6e40d585780
- **Data source ID** (for the Notion MCP `create_pages` tool): `44fdeca6-f7f8-4b3c-b1f7-690f24e3f371`
- **Parent of registry in the Notion tree:** Product Department → Product Pods → Pod Vault 101 → Pod Registry

## Schema (for setup-skill apply phase)

When the setup skill creates a row, use these property keys:

| Property | Type | Source |
|----------|------|--------|
| `Pod Name` | TITLE | `{{POD_NAME}}` (display form) |
| `Pod Slug` | RICH_TEXT | `{{POD_SLUG}}` (kebab-case) |
| `Product Line` | SELECT | `{{PRODUCT_LINE}}` — must be `ChiroHD` or `SKED` exactly |
| `Primary Project` | RICH_TEXT | `{{PROJECT_NAME}}` |
| `Members` | RICH_TEXT | `"{{PM_NAME}} (PM), {{ENGINEER_NAME}} (Engineer)"` |
| `GitHub Repo` | URL | `https://github.com/ChiroHD/pod-vault-{{POD_SLUG}}` |
| `JIRA Epic` | URL | `{{JIRA_EPIC_URL}}` (skip if unknown) |
| `Status` | SELECT | `Active` |
| `date:Created:start` | DATE | today, ISO format |
| `Theme` | SELECT | one of: Lavender, Forest, Crimson, Ocean, Amber, Slate, Plum, Teal, Rust, Indigo, Moss, Rose |
| `Template Commit` | RICH_TEXT | `{{TEMPLATE_COMMIT_HASH}}` (short SHA) |
| `Notes` | RICH_TEXT | optional |

## Failure mode

If Notion MCP is unavailable during setup, fall back to:

1. Surface the row data to the user
2. Provide the database URL above
3. Ask them to add the row manually
4. Continue with the rest of setup — registry registration is non-blocking
