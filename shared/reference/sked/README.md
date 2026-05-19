---
type: reference-stub
product_line: sked
status: tbd
---

# SKED Reference Layer — TBD

This directory will hold the same kind of reference content that `shared/reference/chirohd/` holds, but for the **SKED** product line:

- Product guide (per-feature how-it-works pages)
- Deep dives (subsystem architecture and behavior)
- Glossary (terms, IDs, naming conventions)
- Operating principles (cross-cutting product rules)
- Stakeholder map (SKED-specific org chart and decision authority)
- Customer personas
- Tagging best practices

**Why it's empty:** SKED hasn't been a focus of pod work yet. The first SKED pod to run will be expected to seed this directory using the ChiroHD reference layer as the structural template. The setup skill picks `chirohd` as the active reference layer if the pod's `{{PRODUCT_LINE}}` is ChiroHD; it'll pick `sked` once content exists here.

**For the first SKED pod:**

1. Don't try to populate this from scratch on day one — start the pod, ingest content as you would normally, and let real meeting notes / decisions drive what reference pages need to exist.
2. When patterns repeat (you keep linking to the same external doc, you keep re-explaining the same concept), promote them into this directory.
3. Mirror the structure of `shared/reference/chirohd/` so future SKED pods can navigate it the same way.
4. The first wave of pages typically: product guide index, glossary, stakeholder map. Deep dives accumulate over months.

Until this layer has content, SKED pods will reference back to ChiroHD's layer where overlapping concepts apply (billing, scheduling primitives, etc.) and rely on raw transcripts + meeting notes for everything SKED-specific.
