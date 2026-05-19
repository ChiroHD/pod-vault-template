---
type: meeting-note
date: 2026-04-13
attendees:
  - Riley Chen
  - Sam Patel
duration: ~95 min
source: fathom-transcript
source_transcript: "raw/2026-04-13 Statement Balance Bug Deep Dive (redacted).md"
project: statement-redesign
tags:
  - bug
  - root-cause
  - architecture
  - ledger
---

**Source transcript:** [[2026-04-13 Statement Balance Bug Deep Dive (redacted)]]

## Summary

Sam walked Riley through his root-cause investigation of the stale-balance bug. The cause is drift in `patient_account.cached_balance` caused by a non-transactional trigger updating the cached value while the underlying ledger write fails and rolls back. The team rejected two "fix the cache" options in favor of computing balance live at statement render time. The architecture decision was recorded as [[2026-04-08 Statement balance source — recalc on render vs cached]] (written up after this meeting). Sam estimates Phase 1 ships by April 17.

## Key Decisions

1. **Recalc on render is the path forward.** The `cached_balance` column stays for backward compat with other features that read it (case summary cards, dashboard tiles), but is no longer the source of truth for statements. See [[2026-04-08 Statement balance source — recalc on render vs cached]].
2. **New service `StatementBalanceCalculator`.** Single-patient ledger sum, isolated from the rest of the ledger code path. STMT-101 created.
3. **No reconciliation job.** The "fix the cache periodically" option was considered and rejected — it treats the symptom, not the cause.

## Key Discussion Points

### The Trigger Sequence Problem

Sam reconstructed the sequence from production logs (3 representative practices, 4 confirmed drift incidents in the last 90 days):

1. Application opens a transaction, writes a ledger entry.
2. The trigger on `ledger_entries` fires inside the same transaction, computes the new cached balance, writes to `patient_account.cached_balance`.
3. The ledger write hits a constraint failure (race condition on a unique-write-off-reason constraint, rare but real).
4. The application logs the failure and retries the ledger entry.
5. The retry succeeds; the trigger fires again; cached balance increments again.

The bug: in some failure modes the application catches the failure mid-trigger-execution, the trigger commits in its own connection context, and the cached value doesn't roll back even though the ledger write does. Drift accumulates over months.

Sam: "It's been there forever. We just haven't noticed because nobody compares the statement to the live ledger more than once or twice per practice per year — and when they do, the answer is always 'must be a timing thing, restart the page.'"

### Three Options Considered

| Option | Description | Why Rejected (or Picked) |
|--------|-------------|--------------------------|
| A — Reconciliation job | Periodic sweep to rewrite `cached_balance` from `ledger_entries` | Treats the symptom, not the cause. Drift would still appear between sweeps. |
| B — Fix the trigger transactionally | Rework the trigger to be properly transactional | Largest blast radius. The trigger fires on every ledger write across the entire platform. Regression risk too high for V1. |
| C — Recalc on render (chosen) | New service computes balance live at statement render | Correctness by construction. Isolated scope. Bounded performance cost. |

### Performance Sanity Check

Riley pushed on the performance question. Sam ran a quick bench during the meeting on the longest-history patient he could find in dev — 4 years of history, ~4,800 ledger entries:

- Cached read (current): ~120ms total render-to-PDF
- Recalc on render (new): ~380ms total render-to-PDF

Under the 1s render budget by a comfortable margin. Sam: "If a single practice doubles their statement volume in a year, we still have headroom. And if it ever gets tight, we add an index, not a cache."

### Cross-References to Other Features

Sam confirmed `cached_balance` is read by:

- Case summary card on the Patient Profile
- The patient list view's "balance due" column
- The dashboard "total receivables" tile
- A handful of internal reports

None of these are touched by V1. They continue to use `cached_balance`. If they need correctness later, that's a follow-up project — possibly STMT-204 (drop the column entirely) plus follow-ons.

## Action Items

| Owner | Action | Due | Status |
|-------|--------|-----|--------|
| Sam | Create STMT-101 — `StatementBalanceCalculator` service | Apr 14 | Open |
| Sam | Create STMT-102 — wire service into render endpoint | Apr 14 | Open |
| Sam | Build + ship STMT-101 | Apr 17 | Open |
| Sam | Build + ship STMT-102 | Apr 22 | Open |
| Riley | Write up decision record for the recalc-on-render choice | Apr 14 | Open |
| Riley | Update project-brief.md "Known Risks" — recalc-performance monitoring | Apr 13 | Open |

## Cross-Refs

- Decision recorded: [[2026-04-08 Statement balance source — recalc on render vs cached]]
- Source transcript: [[2026-04-13 Statement Balance Bug Deep Dive (redacted)]]
- Wiki: [[Statement Balance Recalc Pattern]]
- Next meeting: [[2026-04-20 Delivery Mechanism Workshop]]
