---
type: meeting
date: YYYY-MM-DD
project: {{PROJECT_SLUG}}
attendees:
  - {{PM_NAME}}
  - {{ENGINEER_NAME}}
duration: "HH:MM - HH:MM ET"
source_transcript: "raw/YYYY-MM-DD <Meeting Title> (redacted).md"
tags:
  - type/meeting
  - project/{{PROJECT_SLUG}}
---

# YYYY-MM-DD Example Meeting — Use This As A Template

This file is a reference for the meeting-note format. Delete it (or repurpose) once you have a real meeting to record.

**Source transcript:** [[YYYY-MM-DD Example Meeting transcript (redacted)]]

## Summary

1-2 paragraphs summarizing what was discussed and the outcome. Optimized for someone reading this 3 months later who doesn't remember the meeting.

## Decisions

List of decisions made in this meeting. Each one should also get its own decision record in `decisions/` — link to it here.

1. **<Decision summary>** — see [[YYYY-MM-DD <Decision summary>]]
2. ...

## Action Items

What needs to happen post-meeting and who owns it.

- [ ] {{PM_NAME}}: Action item 1 — `due YYYY-MM-DD`
- [ ] {{ENGINEER_NAME}}: Action item 2 — `due YYYY-MM-DD`

## Key Discussion Points

Top 3-5 things discussed. One subsection per topic. Be specific — not "we talked about scope" but "we agreed Phase 1 includes X but not Y because of Z constraint."

### <Topic 1>

What was discussed, what was concluded.

### <Topic 2>

What was discussed, what was concluded.

## Open Questions

Anything raised but not resolved. Each should either get filed elsewhere (tasks, research note, next meeting agenda) or surface as a clarifying question.

- TBD
- TBD

## References

- [[Related decision]]
- [[Related wiki page]]
- [[Related prior meeting]]
