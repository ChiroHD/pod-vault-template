---
type: decision
date: 2026-04-21
status: accepted
project: statement-redesign
decided_by:
  - Sam Patel
  - Ryan Caskey
source: 2026-04-20 Delivery Mechanism Workshop
tags:
  - architecture
  - pdf
  - report-engine
  - backend
---

## Decision

Statement PDFs are generated server-side using the existing ChiroHD report engine — the same pipeline DX reports, Well Health summaries, and the recent Vital Signs CSV export use. A new "statement" template is registered with the engine; the engine handles fonts, layout primitives, page breaks, and practice-level branding. No client-side PDF generation. No new PDF library is introduced.

## Context

The redesigned statement needs to render as PDF for two output paths: (1) the email attachment for send-from-profile and (2) the embedded preview in the patient portal. Both need pixel-stable output across browsers and email clients.

Sam ran a 1-day spike on April 21 to choose between three implementations:

- **A: Existing report engine.** Server-side, jinja-like template language, branding subsystem already in place. Used today for ~30 distinct report types. Engineering maintained.
- **B: New client-side renderer (jsPDF or similar).** Renders in the browser from the statement React component. No server roundtrip for the staff preview.
- **C: New server-side renderer outside the engine.** A standalone microservice using a different library (Puppeteer-headless or wkhtmltopdf).

Ryan Caskey weighed in mid-spike. His position (consistent with prior architecture guidance): "If we already own the pipeline, we use the pipeline. Don't introduce a second PDF stack to maintain."

## Rationale

- **Reuse beats reinvent.** The report engine already solves the hard parts: cross-platform font rendering, page-break handling, branding subsystem (practice logos, header/footer templates), audit logging of generated documents. Re-implementing any of these in V1 is unnecessary risk.
- **One PDF stack to maintain.** Statements would be the second PDF stack if we picked C, or a hybrid stack if we picked B (browser PDFs for preview, server PDFs for email — guaranteed to drift). Single source.
- **Branding consistency.** The report engine reads practice branding (logo, address, colors) from a central source. Statement output matches the visual identity of every other ChiroHD-generated document.
- **Performance is sufficient.** Engine render benchmarks at 200–300ms per statement on typical history. Combined with the recalc-on-render service ([[2026-04-08 Statement balance source — recalc on render vs cached]]), total render-to-PDF stays under 1s.
- **Ryan Caskey's nudge.** Architecture authority confirming the direction. Going against this would require a stronger reason than "shiny new thing."

## Consequences

- New statement template registered with the report engine ([STMT-103](https://example.atlassian.net/browse/STMT-103)).
- Engine registration ([STMT-104](https://example.atlassian.net/browse/STMT-104)) wires the statement template into the engine's template registry; existing branding subsystem applies automatically.
- The portal preview ([STMT-105](https://example.atlassian.net/browse/STMT-105)) embeds the same rendered PDF the staff sees and the patient receives — no separate HTML render path. See [[2026-04-28 Patient portal preview — read-only embedded PDF]].
- No new dependency to manage. No new failure mode (the report engine is already monitored).
- A future request for client-side preview (e.g., "show me the rendered statement without a server roundtrip") would require revisiting this decision. None on the roadmap.

## Alternatives Considered

| Option | Why Rejected |
|--------|--------------|
| B — Client-side renderer | Two PDF stacks guaranteed to drift. Doesn't reuse branding subsystem. |
| C — New standalone microservice | Second PDF stack to operate and monitor. No benefit over the existing engine. |
| Generate HTML and let the browser print | Wildly inconsistent output across browsers and email clients. Non-starter for billing documents. |

## History

- **2026-04-20:** Workshop surfaced the question. See [[2026-04-20 Delivery Mechanism Workshop]].
- **2026-04-21:** Sam ran the 1-day spike. Ryan Caskey weighed in via Slack DM with the "use the pipeline" guidance. Decision locked.
- **2026-04-24:** [STMT-103](https://example.atlassian.net/browse/STMT-103) shipped (template + layout).
- **2026-04-28:** [STMT-104](https://example.atlassian.net/browse/STMT-104) shipped (engine registration). End-to-end render working.
