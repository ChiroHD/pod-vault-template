---
type: reference
product_line: sked
created: 2026-05-19
tags:
  - type/reference
  - product_line/sked
---

# SKED Glossary

Terms you'll hear in SKED Slack channels, standups, and KT docs. Seed list — extend as new vocabulary appears.

## Products and features

- **SKED** — Patient engagement add-on for EHRs. Integrates with five EHRs via two-way sync for clients and appointments. Primary user surface is an SMS Inbox (~80% of office-staff time). Core capabilities: automated messages, drip campaigns, review requests, form builder with conditional logic, patient portals, CLA Synapse integration, GoHighLevel sync.
- **Alina** — SKED's AI call and chat assistant for chiropractic offices. Currently handles inbound scheduling of new appointments; rescheduling and cancellations are the scope expansion required before exiting beta. Architecturally simple: knowledge base lives in a system prompt (not RAG), no formal eval harness. ~100 beta clinics as of mid-May 2026. Onboarding is a support-led admin task, not self-service. Sometimes referred to as "the scheduling agent."
- **Drip Campaigns** — Multi-step automated message sequences (SMS, email, push) built on the standard SKED messaging stack. Three step types: Trigger, Send Message, Delay. The Delay step is more sophisticated than competitors — supports waiting for days, appointment arrivals, or combinations. Beta with ~10–12 testers as of mid-May 2026; nearly production-ready. Feedback inbox: `dripcampaigns@sked.life`.
- **Spark** — Separate branded product built on SKED tech. Sold to ChiroHD offices that don't have SKED. Orange sidebar vs. SKED's blue. Limited feature set, outcomes-based pricing, managed campaigns (clients don't get the full campaign builder).
- **ARIA** — AI report builder. Deferred to Q3. Notably, the ARIA pod is the **only place in the org where an AI evaluation framework exists** — anyone working on AI features elsewhere should treat ARIA as the internal benchmark.

## Tools and locations

- **Builds** — The primary section of the SKED Notion team space where requirements docs live (last ~6 months). Linked from originating GitLab tickets. Older requirements docs are in Google Docs.
- **`skedlife.monday.com`** — SKED's Monday.com workspace. Separate board from ChiroHD's. Hosts the SKED roadmap; Dr. C runs prioritization across both product lines.
- **Ideas repo** — A GitLab repo maintained by John Covert with markdown notes and research. Non-obvious but occasionally linked from requirements doc appendices.

## Roles and groups

- **Pod model** — The current organizational unit at ChiroHD/SKED: one PM + small engineering team owning a product or feature area end-to-end. Replaced the prior structured PDLC at SKED. Each pod sets its own process.
- **Alina pod** — Eshin, Phillip, Adam Davis, Yannick, Daniel Brice.
- **Drip Campaigns pod** — André Assunção (primary engineer).
- **ARIA pod** — Brian Wiggins, Josh Benner. The pod that attended Gauntlet AI training; holds the org's only AI eval discipline.
- **Design committee** — Stephanie Schmidt and Matt Godfrey. James Hendrie was their primary comms bridge to product/eng leadership.
- **SKED Role Intros** — Shorthand for the group introduction meeting with the three non-pod-specific SKED contacts: Becks Drobnik (Training), Jennifer Glenn (Support), Matt Godfrey (Product SME).

## Process terms

- **PDLC** — Product Development Lifecycle. Specifically refers to the structured process James Hendrie built for SKED over ~3 years and which has now been formally retired in favor of the pod model. If someone says "the old PDLC" they mean this. See [[SKED Process Notes]].
- **Lower environments** — SKED's non-production dev environments (~6–10 of them) managed via a GitLab build pipeline. Turned off daily; rebuilt on demand. Ask a developer to spin one up rather than learning the pipeline.
- **Eval harness** — A test suite of representative cases used to measure AI feature behavior over time. Built from observed production failure modes. Standard practice in AI product development. Does not exist for Alina; does exist for ARIA. The absence org-wide is a known gap.

Related: [[SKED Tooling Map]] · [[SKED Stakeholder Map]] · [[SKED Process Notes]]
