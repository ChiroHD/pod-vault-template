---
name: feedback-ingest-assumptions
description: "When ingesting content, prefer asking clarifying questions over making assumptions. Surface ambiguity rather than resolving it silently. Don't capture assumptions as truth — they become hard-to-detect drift in the vault's compiled knowledge."
metadata: 
  node_type: memory
  type: feedback
  originSessionId: fe3b3c3b-44fa-45f6-946e-a84d68c71ade
---

When ingesting content into the pod vault, **prefer asking clarifying questions over making assumptions**. If something in the source material is ambiguous, contradicts other sources, or requires inference to file, **stop and ask** rather than silently resolving. Don't capture assumptions as truth.

**Why:** The vault's compiled-knowledge layer (decisions, wiki, living artifacts) gets re-read months later as ground truth. Assumptions that get captured as truth become hard-to-detect drift — they're indistinguishable from things the user actually said, and they propagate through cross-links + reconciliation. A wrong attribution today becomes a wrong "{{ENGINEER_NAME}} decided X" in three weeks when nobody remembers the source. Better to slow down and ask.

This rule complements [[feedback-ingest-transcripts]] (capture the raw source) and [[feedback-wiki-synthesis]] (force a wiki-update call) — together they cover three classes of vault integrity failure: missing source, skipped synthesis, and silent assumption.

## When to ASK vs when to FLAG in output

**Ask the user (interactive)** if the ambiguity materially changes what gets filed:

- Is this a real decision, or just a discussion item? (Affects whether a decision record gets created)
- Who is the decided_by — a single person or the group? (Affects attribution and follow-up)
- Does this statement supersede an earlier decision, or coexist with it?
- Two sources contradict on a fact — which is canonical?
- An action item has no owner — who actually owns it?
- A status update is ambiguous ("done" — does that mean shipped to prod, shipped to dev, or just code-complete?)
- A new entity is mentioned but unclear if it's already in the vault under another name

**Flag in output (non-blocking)** if the ambiguity is small and the ingest can proceed:

- Minor wording / labeling differences
- Speaker attribution in a multi-speaker transcript where the speakers don't matter much for the content
- Date precision (year vs month vs day) when context strongly implies one
- Cross-link guess where the wrong target is recoverable

For "flag" cases, add `confidence: low` or `needs_review: true` to the file frontmatter, and include the item in the final ingest report under a "Review queue" section.

## How to phrase clarifying questions

- **Be specific about what's ambiguous** — quote the source text and explain why it's unclear
- **Offer the candidate interpretations you considered** — easier for the user to pick than to write
- **Ask one question at a time** unless they're tightly related — batching can confuse
- **Don't ask trivial things** — if the answer is obvious from context or the user has been clear elsewhere, just file

Bad: "I'm not sure how to handle this — what do you want?"
Good: "The 5/15 transcript has Dr. C saying 'we should ship this Friday' — but the 5/16 follow-up Slack shows the actual ship date was Tuesday. Is the decision record's `decision_date` 5/15 (when said) or 5/19 (when shipped)?"

## When the user isn't available (async ingest)

Sometimes ingests run without an immediate user — e.g., {{ENGINEER_NAME}} triggers `/ingest` overnight after a meeting. In that case:

- File the content with the best available interpretation
- Mark affected items with `confidence: low` or `needs_review: true` in frontmatter
- List them prominently in the ingest report under "**Needs Review**" so the user sees them on next session
- Add to tasks.md as a low-priority cleanup item if material

## Anti-patterns to avoid

These have all happened in past ingests. Calling them out so they get spotted:

1. **Inferring decisions from discussion.** "{{ENGINEER_NAME}} said the mapping should be in the modal" — is that a decision, or him thinking out loud? If the source doesn't say "decided" or there's no follow-through, ask.
2. **Filling in unstated dates.** If a meeting note says "ship next week" without a date, don't write "ship 2026-05-26" — write "ship next week (date TBD)" or ask.
3. **Resolving conflicting sources silently.** If two transcripts give different numbers, file both and flag — don't average or pick one.
4. **Attributing to a single person when the source is collective.** "The pod decided" ≠ "{{ENGINEER_NAME}} decided." Use the right granularity.
5. **Promoting a wiki Candidate Topic prematurely.** If a topic surfaced once with minimal context, leave it in Candidate Topics. Don't create a half-baked page.
6. **Cross-linking to similar-but-different concepts.** "Care plan" in one context might mean the data model; in another, the UI component. Check before linking.
7. **Adding inferred causation.** "X happened, then Y happened" doesn't mean "X caused Y." Don't add causal language unless the source explicitly says it.

## Related

- [[feedback-ingest-transcripts]] — capture the raw source so future you can verify against it
- [[feedback-wiki-synthesis]] — force a wiki-update call to prevent silent skips
- This rule is the third leg: prevent silent assumptions from being treated as facts
