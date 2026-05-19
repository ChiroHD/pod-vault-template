---
type: reference
project: {{PROJECT_SLUG}}
status: living
created: {{CREATED_DATE}}
last_updated: {{CREATED_DATE}}
ticket_system: {{TICKET_SYSTEM}}
purpose: Brief mapping of pod work → engineering tickets. The ticket system is the source of truth for status; this file is a navigation aid.
---

# {{PROJECT_NAME}} — Engineering Stories

> Brief reference. The engineering tickets in `{{TICKET_SYSTEM}}` ({{EPIC_REF}}) are the source of truth for current status, acceptance criteria, and implementation details. This file is a fast cross-reference between pod-vault context and the engineering work.
>
> **`ticket_system:` values:** `jira` (ChiroHD default), `gitlab` (SKED default), or `jira+gitlab` (split-tracked pods like Alina, where one engineer uses JIRA personally and mirrors to GitLab Issues). The ingest skill applies the right lifecycle rule based on this field.

## Epic

[{{EPIC_REF}}]({{EPIC_URL}}) — owns all stories below

## Stories

### {{EPIC_REF}}-### — Example story title

**Status:** Open / In Progress / Closed
**Phase:** 1
**Decision context:** [[YYYY-MM-DD Some Decision]]
**Meeting context:** [[YYYY-MM-DD Some Meeting]]

One paragraph describing what this story implements, the constraints it inherits from the decisions it cites, and any cross-references to other stories.

### Supporting Tasks (not user-facing stories)

| Ticket | Title | Status |
|--------|-------|--------|
| | | |

## Backlog (Post-V1)

| Ticket | Title | Notes |
|--------|-------|-------|
| | | |
