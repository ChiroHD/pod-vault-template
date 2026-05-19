---
type: wiki
topic: PDF Generation Pipeline
created: 2026-05-15
updated: 2026-05-15
sources:
  - "projects/statement-redesign/decisions/2026-04-21 Statement PDF generation — server-side via existing report engine.md"
  - "projects/statement-redesign/meetings/2026-04-20 Delivery Mechanism Workshop.md"
---

# PDF Generation Pipeline

How V1 generates statement PDFs. The architectural choice: reuse the existing report engine rather than introduce a new PDF stack. Ryan Caskey's nudge ("if we already own the pipeline, we use the pipeline") was decisive.

## The Existing Engine

ChiroHD's report engine is server-side. It already renders:

- DX reports (clinical documentation summaries)
- Well Health summaries
- Vital Signs CSV export (the recent TRP project)
- Various internal admin reports
- ~30 distinct report types total

The engine handles the hard parts: cross-platform font rendering, page-break logic, branding subsystem (practice logo, header/footer templates), audit logging of generated documents. It's actively maintained, monitored, and well-understood by engineering.

## What V1 Adds

A single new template registered with the engine ([STMT-103](https://example.atlassian.net/browse/STMT-103), [STMT-104](https://example.atlassian.net/browse/STMT-104)):

- Statement template — three-column body (charges / payments / credits), single "amount due" footer, practice header
- Renders to PDF via the engine's standard output path
- Practice branding (logo, name, address, phone) loaded from Statement Settings via the engine's existing branding subsystem
- No new dependency. No new failure mode.

## What Was Rejected

| Option | Why Rejected |
|--------|--------------|
| Client-side PDF (jsPDF or similar) | Two PDF stacks guaranteed to drift over time. Doesn't reuse the branding subsystem. Browser-side rendering is also less consistent across clients. |
| New standalone microservice (Puppeteer or wkhtmltopdf) | A second PDF stack to operate and monitor. No benefit over the existing engine. |
| Browser print to PDF | Wildly inconsistent output across browsers and email clients. Non-starter for billing documents. |

See [[2026-04-21 Statement PDF generation — server-side via existing report engine]] for the full decision.

## Render Path

```
[Statement render endpoint called]
         ↓
[StatementBalanceCalculator computes totals from ledger]    ← see Statement Balance Recalc Pattern
         ↓
[Report engine: load statement template + data]
         ↓
[Engine applies practice branding via branding subsystem]
         ↓
[Engine handles fonts, page breaks, layout]
         ↓
[Renders to PDF buffer]
         ↓
[Returned to caller: send-from-profile modal preview, email attachment, portal embed]
```

The same rendered PDF buffer serves three consumers:

1. **Send-from-profile preview** in the staff modal
2. **Email attachment** to the patient
3. **Portal embed** for the patient via the existing portal PDF viewer iframe

One render, three consumers. See [[Patient Portal Preview UX]] for why the portal embed reuses the same file rather than rendering separately.

## Performance

Benchmarks from the 4/21 spike + the 5/11 pre-beta validation:

| Scenario | Render-to-PDF |
|----------|---------------|
| Single patient, recent 30-day window, ~50 ledger entries | ~140ms |
| Patient with 60-day history, ~150 ledger entries | ~210ms |
| Long-history patient, 90-day window, ~340 entries | ~340ms |
| 4-year-history patient, worst case observed in dev | ~380ms |

Under the 1s render budget by a comfortable margin. The bottleneck is the recalc query, not the engine — the engine itself is in the 80–120ms range across all scenarios.

## Branding

The engine's branding subsystem pulls per-practice values from a central source:

- Practice logo (from Statement Settings or Office Configuration default)
- Practice name (from the practice record)
- Practice address, phone (from the practice record)
- Practice color/font (currently defaults only; per-practice theming is a future feature)

If a practice hasn't configured a logo, the engine falls back to text-only header. Tested in pre-beta.

## When NOT to Use the Engine

The "reuse the pipeline" guidance applies to any document that:

- Needs cross-platform pixel-stable output
- Benefits from per-practice branding
- Has a known, server-side render trigger

It does NOT apply to:

- Real-time interactive previews where the user is editing the document inline (use HTML + WYSIWYG instead)
- Documents that need to be edited after generation (PDF is for output, not interchange)
- High-volume background batch rendering (different scaling profile — would need its own queue worker)

## Sources

- [[2026-04-21 Statement PDF generation — server-side via existing report engine]]
- [[2026-04-20 Delivery Mechanism Workshop]]

## See Also

- [[Statement Balance Recalc Pattern]] — the first half of the render path
- [[Patient Portal Preview UX]] — how the same rendered PDF reaches the patient
- [[Email Delivery Architecture]] — how the PDF gets attached to email
