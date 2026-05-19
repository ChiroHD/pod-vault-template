---
type: wiki
topic: Stakeholder Communication — Dr. C
created: 2026-05-15
updated: 2026-05-15
sources:
  - "projects/statement-redesign/meetings/2026-04-20 Delivery Mechanism Workshop.md"
  - "projects/statement-redesign/meetings/2026-04-27 Dr. C Sync — Statement UX Direction.md"
  - "projects/statement-redesign/decisions/2026-04-14 Email vs SMS delivery — email-first for V1.md"
  - "projects/statement-redesign/decisions/2026-04-28 Patient portal preview — read-only embedded PDF.md"
  - "projects/statement-redesign/decisions/2026-05-05 Email template — branded vs plain.md"
---

# Stakeholder Communication — Dr. C

Patterns observed working with Dr. Cristina Esposito on the Statement Redesign project. The Atlas pod has run six interactions with her across the project arc; the patterns repeat consistently enough to capture as a working model.

## Pattern 1: "Don't Bundle"

Dr. C reliably refuses to bundle features that should ship on their own merits. The exact phrasing has shown up three times in this project:

- **Delivery workshop (4/20):** "Ship email first. SMS is a real ask but it's not the same project. Don't conflate."
- **UX sync (4/27):** "Pay-now-from-portal is its own feature — don't bundle."
- **(Implicit) Email template direction:** Choosing branded-with-logo over plain, but explicitly NOT adding subject line customization, multiple template options, or HTML customization. One configurable note field, nothing more.

**Operational implication:** When proposing scope to Dr. C, lead with what's NOT in scope and why. She responds well to crisp scope arguments; she pushes back on omnibus requests.

## Pattern 2: "The Practice Is the Customer"

Dr. C frames patient-facing features through the practice's relationship with the patient, not ChiroHD's relationship with either. This was decisive on the email template question:

> "The patient relationship is with the practice, not us. If it doesn't look like the practice sent it, half the patients will think it's a phishing email and delete it. Branded."

**Operational implication:** Patient-facing surfaces should make the practice look professional. "ChiroHD-branded" is generally the wrong answer; the practice is the brand the patient sees.

## Pattern 3: One-Sentence Decisions

Dr. C tends to resolve open questions in a single sentence rather than discussing options. The Atlas pod has learned to:

- Bring her **two options**, not three or four (she'll pick one quickly)
- Pre-align with each other before the meeting (she answers the question asked, not the question implied)
- Document her exact phrasing in meeting notes (her one-sentence framing tends to become the project's reusable shorthand)

**Operational implication:** Use Dr. C meetings to close questions, not to explore them. Exploration belongs in pod working sessions.

## Pattern 4: Approval Pattern

For broad releases, Dr. C signs off via the regular 1:1 cadence — not via Slack, not via standalone email. She wants:

- A one-page summary of the beta-feedback themes
- The pod's recommendation (release Jun 1 or hold for Jun 8)
- A clear "any blockers" answer

Riley's prep for the 6/1 1:1 follows this exact format (see [[Validation Plan]] — "Wrap summary for Dr. C 1:1").

## Pattern 5: Feedback Style — Direct, Visual, Confident

When shown two mockups side-by-side, Dr. C points at the one she likes and gives the reason. Two examples from this project:

- **Layout V2 walkthrough (4/27):** Pointed at the single-bolded-footer mock. "Once. At the bottom. Patient knows where to look."
- **Email template comparison (5/5):** Pointed at the branded mock. Reason came in one sentence about phishing-perception.

**Operational implication:** Bring visuals to Dr. C 1:1s. Don't describe in words what a screenshot or mockup can communicate in one glance.

## What This Pattern Predicts

Going into beta (5/18 – 5/29), and then broad release sign-off (6/1):

- She will **not** want to bundle the broad-release announcement with other product news. Separate communication.
- She will **not** ask for SMS during the beta — it's already deferred, she signed off, the question is closed.
- She **will** ask for a one-page beta-feedback summary in the 6/1 1:1, with the pod's recommendation, in her standard meeting format.
- She **will** approve the broad release in one sentence if the beta wrap-up looks clean.
- She will **not** want a separate "release readiness review" meeting. The 1:1 covers it.

## Anti-Patterns (Things to Avoid)

- Don't bring her three options. Two.
- Don't propose bundling. Even small bundles ("we could fold X into the same release") tend to trip the "don't bundle" reflex.
- Don't ask her to validate technical implementation details. That's the pod's job. She validates direction and patient/practice-facing decisions.
- Don't relitigate a closed decision in a follow-up meeting unless new information has surfaced. She tends to read that as the pod being unsure.

## Sources

- [[2026-04-20 Delivery Mechanism Workshop]]
- [[2026-04-27 Dr. C Sync — Statement UX Direction]]
- [[2026-04-14 Email vs SMS delivery — email-first for V1]]
- [[2026-04-28 Patient portal preview — read-only embedded PDF]]
- [[2026-05-05 Email template — branded vs plain]]

## See Also

- [[Beta Cohort Selection]] — the 6/1 sign-off pattern
- [[Patient Portal Preview UX]] — canonical example of the "don't bundle" pattern
