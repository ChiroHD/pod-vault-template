---
type: wiki
topic: Patient Portal Preview UX
created: 2026-05-15
updated: 2026-05-15
sources:
  - "projects/statement-redesign/decisions/2026-04-28 Patient portal preview — read-only embedded PDF.md"
  - "projects/statement-redesign/meetings/2026-04-27 Dr. C Sync — Statement UX Direction.md"
---

# Patient Portal Preview UX

The UX choice for showing statements in the patient portal: a read-only embedded PDF, the exact same file the staff sees in the send-from-profile preview and the patient receives by email. One render, three consumers. No HTML render.

## The Pattern

```
Staff send-from-profile preview  ─┐
Patient email attachment           ├─→  Same rendered PDF
Patient portal preview embed      ─┘
```

Three different surfaces, one underlying file. No drift possible because there's no second render path to drift from.

## Why This Beats HTML

The original portal mock had statements as HTML, with the idea that the patient could click "Pay Now" inline. Two problems:

1. **Two renders to maintain.** HTML for portal + PDF for email means two layout codebases, two sets of tests, two places to update when the design evolves. Guaranteed to drift over time.
2. **Bundles the statement project with payment-flow work.** Pay-now-from-portal requires Fortis-side integration, patient auth tightening, refund-handling surface. None of these are statement work — they're payment work.

Dr. C surfaced both during the 4/27 sync:

> "The patient should see exactly what the staff sent. If it's a PDF in the email, it's the same PDF in the portal. Don't have two renders that can drift. Pay-now-from-portal is its own feature — don't bundle."

This is the same "don't bundle" pattern Dr. C applies consistently — see [[Stakeholder Communication — Dr. C]].

## What the Patient Sees

In the existing patient portal's "Statements" section (which previously listed statement dates and allowed download), each statement now opens an embedded PDF viewer. The viewer is the existing portal iframe used elsewhere for clinical documents shared to patients. Standard browser controls — download, print, zoom.

No "Pay Now" button. No editable HTML. No call-to-action other than the natural ones the browser provides (download, print).

## What the Patient Does NOT See

- Practice-side internal notes
- Staff signatures or initials
- Anything that wasn't in the rendered PDF the staff approved before sending

This is intentional. The principle: the patient sees what the staff sent. Nothing more, nothing less.

## Implementation Note

The portal preview ([STMT-105](https://example.atlassian.net/browse/STMT-105)) reuses the existing portal PDF viewer iframe — the same one used today to share clinical notes with patients. No new portal UI primitives needed. The integration was estimated at 1-2 days; came in at 1.

## Validation

Pre-beta validation (5/11) confirmed the embed works across:

- Desktop browsers (Chrome, Firefox, Safari, Edge)
- Portal mobile-web shell
- iOS app shell

All three render the same PDF; download/print actions work in each context.

## Future: Pay-Now-from-Portal

A separate feature, deferred indefinitely. If it ever ships, it can use the same rendered statement PDF as a starting point — the patient identifies the statement they want to pay against, the pay flow attaches to the existing payment infrastructure. The statement project doesn't need rework to support that future feature.

## The Lesson

When a UX question can be answered by reusing what already exists at zero marginal cost, that's almost always the right answer for V1. The portal preview was a "consider for V1" item from the kickoff meeting; the existing portal PDF viewer made it ~0 effort and pulled it into Phase 2 scope.

The corollary: when reusing a primitive would force a feature bundle (HTML render + pay flow), the right answer is to not reuse. Recognize the trap before committing.

## Sources

- [[2026-04-28 Patient portal preview — read-only embedded PDF]]
- [[2026-04-27 Dr. C Sync — Statement UX Direction]]

## See Also

- [[PDF Generation Pipeline]] — the rendered file that gets embedded
- [[Stakeholder Communication — Dr. C]] — the "don't bundle" pattern
