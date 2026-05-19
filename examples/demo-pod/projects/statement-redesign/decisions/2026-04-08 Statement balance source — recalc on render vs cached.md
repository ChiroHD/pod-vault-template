---
type: decision
date: 2026-04-08
status: accepted
project: statement-redesign
decided_by:
  - Riley Chen
  - Sam Patel
source: 2026-04-13 Statement Balance Bug Deep Dive
tags:
  - architecture
  - bug-fix
  - balance
  - ledger
---

## Decision

The patient statement render endpoint will compute statement balance **live on render** by summing `ledger_entries` for the patient, rather than reading `patient_account.cached_balance`. The `cached_balance` column is preserved as-is for backward compatibility with other features that read it (case-level summary cards, the patient dashboard tile), but it is no longer the source of truth for statements.

## Context

Statements have been the #3 source of billing-staff support tickets across Q1 2026. The recurring complaint: "the statement total doesn't match what I see on the patient ledger."

During the 2026-04-13 deep dive Sam traced this to drift in `patient_account.cached_balance`. The cache is updated by triggers on `ledger_entries` insert/update. When a ledger write hits a deadlock or constraint failure and is rolled back, the application logs the failure and retries the ledger entry — but the cached-balance trigger had already started executing in a separate transaction and committed a stale increment. There is no reconciliation job to catch this drift. The longer a patient's history, the more opportunities for accumulated drift.

Sam considered two alternative fixes:

- **A: Add a reconciliation job.** Periodically sweep `ledger_entries` and rewrite `cached_balance`. Catches drift but doesn't fix it at the point of failure; statements would still occasionally render wrong values between sweep runs.
- **B: Fix the trigger.** Rework the trigger to be transactional with the ledger write. Possible but touches a hot path used by many other features; risk of regression is significant.
- **C: Recalc on render.** Compute the sum live each time a statement is rendered. The query is bounded — single patient, `(patient_id, posted_at)` index already exists. Bench tests on the longest-history patient in the dev dataset clocked 380ms render-to-PDF.

## Rationale

- **Correctness by construction.** The recalc value is mathematically derived from the source of truth (`ledger_entries`) at render time. There is no possibility of staleness because there is no cache.
- **Bounded scope.** Statements are user-initiated, not background batch. Render volume is low enough that the per-render cost is acceptable. The new `StatementBalanceCalculator` service is internal-only and isolated from the rest of the ledger code path.
- **Avoids the trigger rewrite risk.** Fixing `cached_balance` properly would require touching code that runs on every charge, payment, write-off, and credit — a much larger blast radius for the same fix.
- **Backward-compat preserved.** Other features that read `cached_balance` (case summary cards, dashboard tiles, etc.) continue to work. They have their own accuracy tradeoffs but are outside the statement-redesign scope.

## Consequences

- New service `StatementBalanceCalculator` ([STMT-101](https://example.atlassian.net/browse/STMT-101)) — single-patient ledger sum, returns the same fields the statement template needs (charges total, payments total, credits total, current balance).
- The statement render endpoint ([STMT-102](https://example.atlassian.net/browse/STMT-102)) calls this service instead of reading `cached_balance`.
- Render time increases from ~120ms (cached) to ~380ms worst-case (recalc). Acceptable; well under the 1s render budget. Monitored via APM.
- `cached_balance` column still updates via existing triggers — no regression risk to dependent features.
- Post-V1 follow-up: STMT-204 will eventually drop `cached_balance` after two clean release cycles. Out of scope for V1.

## Alternatives Considered

| Option | Why Rejected |
|--------|--------------|
| A — Reconciliation job | Closes the drift window but doesn't eliminate it. Statements would still occasionally render stale values. Treats the symptom, not the cause. |
| B — Fix the trigger transactionally | Largest blast radius; the cached-balance trigger is on a hot path used by all ledger writes. Risk of regression outweighs benefit. |
| C — Recalc on render (chosen) | Correctness by construction. Bounded performance cost. Isolated scope. |

## History

- **2026-04-08:** Decision written up after the 2026-04-13 deep dive (yes, retroactively — the dive nailed the root cause; this records the architectural choice that followed).
- **2026-04-13:** Root cause confirmed in the deep dive. See [[2026-04-13 Statement Balance Bug Deep Dive]] for the trace and the trigger-vs-ledger sequence reconstruction.
- **2026-04-17:** STMT-101 shipped. Service in place.
- **2026-04-22:** STMT-102 shipped. Render endpoint wired through. `cached_balance` is no longer read for statement renders.
