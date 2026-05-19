---
type: decision
date: 2026-04-28
status: accepted
project: statement-redesign
decided_by:
  - Riley Chen
  - Cristina Esposito
source: 2026-04-27 Dr. C Sync — Statement UX Direction
tags:
  - ux
  - portal
  - pdf
  - scope
---

## Decision

The patient portal's statement preview is a **read-only embedded PDF** — the exact same rendered file the staff sees in the send-from-profile preview and the same file the patient receives as an email attachment. No separate HTML render. Patients view it via the existing portal PDF viewer iframe; download is available; in-portal payment from the statement is **not** in V1.

## Context

The original portal mock had the statement rendered as HTML so patients could click "Pay Now" inline. During the 2026-04-27 sync, Riley showed Dr. C the design. Dr. C's feedback (paraphrased from meeting notes):

> "The patient should see exactly what the staff sent. If it's a PDF in the email, it's the same PDF in the portal. Don't have two renders that can drift. Pay-now-from-portal is its own feature — don't bundle."

Two design questions implicitly resolved by Dr. C's framing:

- Single render vs two renders (HTML + PDF)? — Single.
- Pay-now-from-portal in V1? — No.

## Rationale

- **One source of truth for the rendered statement.** What staff previewed, what patient received, what patient sees in the portal — all the same file. No drift possible.
- **Halves the design surface.** Cuts portal-side HTML layout work entirely. The existing portal PDF viewer iframe (already used for clinical notes shared to patients) hosts the embed for free.
- **Pay-now-from-portal is a different feature.** Bundling it would couple this project to payment processor work (Fortis-side flow), patient-side authentication tightening, and refund-handling surface. Dr. C's "don't bundle" pattern, consistent across past pod projects (see [[Stakeholder Communication — Dr. C]]).
- **Patients can still pay** — by calling the practice or via the existing Patient Profile pay flow staff drive. The statement is for visibility, not transaction.

## Consequences

- [STMT-105](https://example.atlassian.net/browse/STMT-105) ships as read-only embedded PDF in the portal. Existing PDF viewer iframe is reused; no new portal UI primitives needed.
- Patients see download + print actions from the embedded viewer (browser-default chrome). No "Pay Now" button.
- A future pay-now-from-portal project can use the same statement output as its starting point — no rework needed.
- Documentation in the beta kit tells practices the patient experience clearly: "Your patient receives the statement by email and can also view the same PDF in their portal."
- Saves an estimated 4–5 days of portal-side HTML implementation work.

## Alternatives Considered

| Option | Why Rejected |
|--------|--------------|
| HTML render with inline Pay Now | Two renders to maintain (drift risk). Pulls in payment-flow work. Out of scope. |
| Portal redirect to a payment landing page when patient clicks the statement | Cleaner than inline pay but still bundles V1 with payment work. Pay-now is its own feature. |
| No portal view at all (email-only) | Misses a low-cost win — patients ask "where can I see my statement?" Read-only embed answers it for ~0 marginal effort. |

## History

- **2026-04-27:** Sync with Dr. C. See [[2026-04-27 Dr. C Sync — Statement UX Direction]].
- **2026-04-28:** Decision written up after sync. Sam started portal embed work.
- **2026-05-04:** [STMT-105](https://example.atlassian.net/browse/STMT-105) shipped. Verified across desktop browsers + the portal mobile web shell.
