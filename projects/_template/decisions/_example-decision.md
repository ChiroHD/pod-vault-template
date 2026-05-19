---
type: decision
date: YYYY-MM-DD
status: example
project: {{PROJECT_SLUG}}
decided_by:
  - {{PM_NAME}}
  - {{ENGINEER_NAME}}
tags:
  - type/decision
  - project/{{PROJECT_SLUG}}
  - topic/example
---

# Example Decision — Use This As A Template

This file is a reference. Delete it (or repurpose) once you have a real decision to record.

## Decision

State the decision clearly in one or two sentences. What was decided, with enough specificity that a future reader can act on it without needing to re-derive.

## Context

Why this decision needed to be made. What was the question or tension. What sources informed it (meeting notes, Slack threads, stakeholder asks).

Link to source content via `[[wikilinks]]`:
- `[[YYYY-MM-DD Meeting Title]]`
- `[[Some Related Decision]]`

## Rationale

Why this option was chosen over alternatives. Include:

- Other options considered (briefly)
- Trade-offs accepted
- Constraints that shaped the choice

## Consequences

What this means going forward:

- Implementation implications
- Things that need to change because of this
- Open questions this opens up
- What gets harder or easier

## Status: example

Real decision records use one of: `accepted` / `superseded` / `proposed` / `rejected`. If superseded later, add a `superseded_by:` link in frontmatter and a History section at the bottom explaining what replaced this decision.

## Related

- `[[Other related decisions, wiki pages, or meeting notes]]`
