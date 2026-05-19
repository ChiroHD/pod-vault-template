# Knowledge System

## How It Works

This directory contains accumulated PM learnings organized by domain. The system follows a promotion lifecycle inspired by Pawel Huryn's self-improving knowledge system.

### Lifecycle

```
Observation → Hypothesis (needs validation) → Rule (confirmed, auto-applied)
```

### Files Per Domain

Each domain folder contains three files:
- **knowledge.md** — Confirmed facts (sourced, dated, stable)
- **hypotheses.md** — Observations that need validation (tracked by confirmation count)
- **rules.md** — Validated patterns applied by default in skill outputs

### Promotion Rules

- A hypothesis promotes to a rule after **3 confirmations** across different interactions
- Confirmations must come from distinct sources (different meetings, different people, different contexts)
- A hypothesis with **no confirmations in 2+ weeks** should be re-evaluated (stale)
- A contradicted hypothesis should be demoted or deleted
- Rules can be demoted back to hypotheses if contradicted

### Entry Format

**Knowledge entries:**
```
### [Statement]
- **Source:** [meeting date, person, document]
- **Date:** YYYY-MM-DD
```

**Hypothesis entries:**
```
### [Statement]
- **Source:** [meeting date, person, document]
- **Date added:** YYYY-MM-DD
- **Confirmations:** N/3
- **Last confirmed:** YYYY-MM-DD
- **Evidence:** [list of confirming observations]
```

**Rule entries:**
```
### [Statement]
- **Promoted from hypothesis:** YYYY-MM-DD
- **Confirmations:** N (at least 3)
- **How to apply:** [when/where this rule should influence skill output]
```

### Domains

| Domain | What it covers |
|--------|---------------|
| `stakeholders/` | Decision styles, communication preferences, relationship patterns |
| `chirohd-platform/` | Product constraints, integration gotchas, architecture patterns |
| `process/` | PM workflow patterns, meeting cadences, approval processes |
| `strategy/` | Market signals, competitive patterns, industry trends |
| `artifacts/` | What makes good deliverables at ChiroHD — format, length, evidence standards |

### Weekly Review Integration

The `/weekly-review` skill includes a knowledge review section:
- Hypotheses nearing promotion (2/3 confirmations)
- Stale hypotheses (no confirmation in 2+ weeks)
- Newly promoted rules
- Contradictions or corrections to flag

### Vault Sync

Knowledge files are mirrored to `ChiroHD/Reference/Knowledge/` for browsing in Obsidian. The repo (`work/knowledge/`) is the source of truth.
