---
type: decision
date: 2026-04-14
status: accepted
project: statement-redesign
decided_by:
  - Riley Chen
  - Sam Patel
  - Cristina Esposito
source: 2026-04-20 Delivery Mechanism Workshop
tags:
  - delivery
  - scope
  - email
  - sms
---

## Decision

V1 ships email-only statement delivery. SMS delivery is deferred to V2 (post-broad-release backlog as STMT-201). The send-from-profile modal exposes "Email" as the only delivery option for V1, with the UI structured so SMS can be added as a second option later without rework.

## Context

The send-from-profile workflow ([STMT-106](https://example.atlassian.net/browse/STMT-106)) replaces the current "download PDF → switch to email client → attach → send" pattern that billing staff complain about. The original problem brief listed both email and SMS as candidate delivery methods, because billing staff and patients have asked for both.

During the 2026-04-20 delivery mechanism workshop, the pod walked through the implementation surface for each:

- **Email:** The existing outbound email service already handles patient-engagement reminders. We have rendered PDF as an attachment, configurable per-practice sender identity, and existing template rendering. Implementation is mostly composition.
- **SMS:** ChiroHD's SMS infrastructure exists for appointment reminders but has no payload type for "PDF attachment." SMS-with-secure-link is the realistic shape — but that requires a new patient-facing landing page, signed URL handling, link expiration, and a separate audit trail. Significantly larger surface than email.

Dr. C joined the back end of the workshop. Her position: "Ship email first. SMS is a real ask but it's not the same project. Don't conflate."

## Rationale

- **Smaller V1 scope ships faster.** Email-only Phase 3 fits in the existing release cadence (May 25). Adding SMS would push Phase 3 by ~3 weeks for the SMS landing page + signed URL system alone.
- **Email is the higher-value delivery for the primary complaint.** The current pain point is the workflow gap ("download PDF, then go to my email") — billing staff overwhelmingly already use email. SMS is a nice-to-have for patient convenience, not a fix for the staff workflow.
- **SMS-as-V2 is cleaner than email-and-SMS-half-baked.** A separate SMS project can do landing-page UX, secure link expiration, opt-in management, and SMS template config properly. Doing them poorly in V1 would create a feature we can't deprecate.
- **Dr. C explicitly aligned on this scoping.** "Don't conflate" — the same pattern she's applied across other pod projects when delivery channels expand scope.

## Consequences

- Send-from-profile modal ([STMT-106](https://example.atlassian.net/browse/STMT-106)) ships with a delivery-method dropdown that has only "Email" enabled. SMS is greyed out with a tooltip: "Coming soon."
- The modal's data flow is shaped so adding SMS later requires adding an option, not rebuilding the workflow.
- STMT-201 lands in the Post-V1 backlog ([[jira-stories]]) — SMS delivery for statements, owned by a future pod or this one's continuation.
- Beta kit and broad-release announcement explicitly mention email-only — set expectations rather than apologize later.
- Maya's support team won't need to handle SMS-specific issues during beta. Reduces variable surface.

## Alternatives Considered

| Option | Why Rejected |
|--------|--------------|
| Ship email + SMS together in V1 | Pushes Phase 3 ~3 weeks; SMS surface (landing page + signed URL) is its own project. |
| Ship SMS first (the more novel channel) | Doesn't address the staff workflow pain point — that's an email problem. Wrong sequencing. |
| Email-only, no UI surface for SMS at all | Less risky now but leaves no signal to billing staff that SMS is coming. Greyed-out dropdown with tooltip is the better UX signal. |

## History

- **2026-04-14:** Decision recorded after Dr. C aligned during the back end of the 4/20 workshop (yes, written up after the workshop note — the discussion happened progressively over the previous week).
- **2026-04-20:** Workshop closed the discussion. See [[2026-04-20 Delivery Mechanism Workshop]] for the implementation-surface walkthrough.
- **2026-05-08:** [STMT-106](https://example.atlassian.net/browse/STMT-106) shipped with email-only delivery as scoped.
