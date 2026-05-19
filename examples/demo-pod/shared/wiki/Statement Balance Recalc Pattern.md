---
type: wiki
topic: Statement Balance Recalc Pattern
created: 2026-05-15
updated: 2026-05-15
sources:
  - "projects/statement-redesign/decisions/2026-04-08 Statement balance source — recalc on render vs cached.md"
  - "projects/statement-redesign/meetings/2026-04-13 Statement Balance Bug Deep Dive.md"
  - "projects/statement-redesign/raw/2026-04-13 Statement Balance Bug Deep Dive (redacted).md"
---

# Statement Balance Recalc Pattern

The architectural choice underlying V1's stale-balance fix: statement totals are computed live from the ledger at render time, not read from a cached column. This page captures the pattern and why it was the right answer for statements specifically.

## The Problem

`patient_account.cached_balance` was the source of truth for statement totals. It's maintained by a trigger on `ledger_entries`. The trigger drifts when ledger writes fail and retry — the trigger commits a stale increment, the ledger entry rolls back, the retry succeeds and the trigger fires again. Across months of operation the cached value diverges from the actual ledger sum.

This wasn't a new problem. It had been the #3 source of billing-staff support tickets in Q1 2026, with the recurring complaint: "the statement total doesn't match what I see on the ledger."

## The Pattern: Recalc on Render

Replace the cached-column read with a live computation. New service `StatementBalanceCalculator`:

- Single-patient query against `ledger_entries`
- Indexed on `(patient_id, posted_at)` — existing index
- Returns the same shape the statement template needs (charges total, payments total, credits total, current balance)
- Called by the statement render endpoint instead of reading `cached_balance`

The performance trade-off was bounded enough to be acceptable: render-to-PDF goes from ~120ms (cached) to ~380ms worst-case (recalc) on a 4-year-history patient with ~4,800 ledger entries. Under the 1s render budget.

## Why Not Fix the Cache

Three options were considered in the 4/13 deep dive:

| Option | Why Rejected |
|--------|--------------|
| Reconciliation job (periodic sweep that rewrites the cache from the ledger) | Treats the symptom, not the cause. Drift still appears between sweeps. |
| Fix the trigger transactionally | Largest blast radius — the trigger runs on every ledger write across the entire platform. Regression risk too high. |
| **Recalc on render (chosen)** | Correctness by construction. Bounded scope. Isolated from the rest of the ledger code path. |

## Why This Works for Statements Specifically

Statements are **user-initiated** — staff click "send statement" or render a preview. The render volume is bounded by human action, not by a background process. This is what makes "compute live each time" affordable.

The same pattern would not work for, say, the dashboard "total receivables" tile, which renders on every dashboard load across every practice. For that, the cached column still wins — the accuracy bar is lower and the access pattern is read-heavy.

The lesson: choose the data-access strategy based on the access pattern, not just the accuracy requirement.

## What Stays the Same

The `cached_balance` column wasn't dropped. It's still updated by the existing triggers, and it's still read by:

- Case summary cards on the Patient Profile
- The patient list view's "balance due" column
- The dashboard "total receivables" tile
- A handful of internal reports

These have their own accuracy trade-offs, but they're outside V1 scope. A future ticket (STMT-204) tracks dropping the column entirely, but only after the cleanup is justified — at least two clean release cycles past V1.

## When to Apply This Pattern

- **Yes:** Single-entity computation with a bounded query and user-initiated access pattern. Statements. Per-patient invoices. Patient-facing receipts.
- **Maybe:** Multi-entity aggregations where the source-of-truth data has reliable foreign-key indexes. Validate performance first.
- **No:** Cross-practice or platform-wide aggregations. Dashboard tiles. Reports that scan hundreds of patients per render. Cache + acceptable-staleness wins there.

## Sources

- [[2026-04-08 Statement balance source — recalc on render vs cached]] — the decision record
- [[2026-04-13 Statement Balance Bug Deep Dive]] — the root-cause meeting
- [[2026-04-13 Statement Balance Bug Deep Dive (redacted)]] — the redacted transcript with the trigger-sequence reconstruction

## See Also

- [[PDF Generation Pipeline]] — the other half of the render path
- [[Statement Glossary]] — `cached_balance`, `StatementBalanceCalculator`, ledger entry definitions
