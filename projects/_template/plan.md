---
type: project-plan
project: {{PROJECT_SLUG}}
status: active
created: {{CREATED_DATE}}
last_updated: {{CREATED_DATE}}
---

# {{PROJECT_NAME}} — Project Plan

## How We Track Work

Three layers, each serving a different audience and cadence:

| Layer | Tool | What Lives Here | Audience | Cadence |
|-------|------|----------------|----------|---------|
| **Roadmap** | Monday | Milestone status, RAG, target dates | {{ADVISOR_NAME}}, leadership | Weekly |
| **Engineering backlog** | JIRA ({{JIRA_EPIC}}) | User stories, bugs, tech tasks — things {{ENGINEER_NAME}} picks up, builds, tests, closes | {{ENGINEER_NAME}}, engineering, {{PM_NAME}} | Per-story lifecycle |
| **Pod coordination** | This vault (`plan.md` + `tasks.md`) | Project plan, phases, dependencies, risks, PM work, decisions pending, daily "who's doing what" | {{PM_NAME}} + {{ENGINEER_NAME}} | Daily |

**The rule:** If {{ENGINEER_NAME}} writes code for it, it's a JIRA story. If it's PM work, stakeholder coordination, content creation, or a decision to be made, it lives in the vault.

## Current State (as of {{CREATED_DATE}})

### What's built

*Filled in as work ships. Each shipped item gets a row.*

| Component | Status | Completed | Notes |
|-----------|--------|-----------|-------|
| TBD | TBD | TBD | TBD |

### What's in progress or remaining

*The active work list. Move rows out as they ship.*

| Component | Owner | Status | Notes |
|-----------|-------|--------|-------|
| TBD | {{ENGINEER_NAME}} or {{PM_NAME}} | Not started | TBD |

### Key blockers

*Anything currently blocking forward progress. Resolve or escalate.*

| Item | Owner | Notes |
|------|-------|-------|
| TBD | TBD | TBD |

## Phased Plan

*Replace with actual phases as the project takes shape. Below is a generic structure.*

### Phase 1: Discovery
**Target:** TBD
**Goal:** TBD

### Phase 2: Build
**Target:** TBD
**Goal:** TBD

### Phase 3: Validation + Beta
**Target:** TBD
**Goal:** TBD

### Phase 4: Broad release
**Target:** TBD
**Goal:** TBD

## Working Rhythm

| Day | Morning (8:30 AM - 12:30 PM ET) | Afternoon |
|-----|--------------------------------|-----------|
| {{POD_SESSION_DAY_1}} | **Pod session** — build + validate together | {{PM_NAME}}: other responsibilities |
| {{POD_SESSION_DAY_2}} | **Pod session** — build + validate together | {{PM_NAME}}: other responsibilities |

**Pod session structure:**
1. First 15 min: Pull vault, check log.md, review tasks.md, align on today's goals
2. Middle 3.5 hours: Build + validate. {{ENGINEER_NAME}} codes, {{PM_NAME}} validates, both discuss blockers in real time.
3. Last 15 min: Update tasks.md, commit and push vault, note any async items.

## Decision Points

| Date | Decision | Who Decides | Status |
|------|----------|-------------|--------|
| TBD | TBD | TBD | Upcoming |

## Assumptions

*Capture the project's load-bearing assumptions here. When they change, update or supersede.*

- TBD
