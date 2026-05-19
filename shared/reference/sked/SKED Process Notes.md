---
type: reference
product_line: sked
created: 2026-05-19
tags:
  - type/reference
  - product_line/sked
---

# SKED Process Notes

How product development actually runs on the SKED side. Read this before your first pod meeting — the rituals you might expect from a ChiroHD pod (deep-work sessions, structured discovery, fixed cadences) are largely absent here.

## The PDLC is gone

James Hendrie spent ~3 years building a structured Product Development Lifecycle for SKED — discovery, requirements, design review, release planning. With the shift to the **pod model**, that PDLC has been abandoned. There is no company-wide standardized process anymore. Each pod sets its own.

What that means for a new SKED PM:

- **Do not expect inherited process.** When you join a pod, the first conversation should be about how the pod currently operates — not assumptions imported from another pod.
- **Don't try to impose a process in week one.** Process is the right answer to many problems you'll see, but Gabe Doty is famously process-averse and the org-wide drift away from the PDLC is not an accident. Earn the right to add structure by demonstrating value first.
- **Pod processes will vary widely.** The Alina pod is continuous-shipping under engineer leadership; the Drip Campaigns pod followed the old structured method and is nearly done. Two pods, two different operating models, both legitimate.

## Alina runs autonomously, engineer-led

Eshin Griffith effectively runs the Alina pod product-wise in addition to engineering. The pod operates "like a factory" — continuous shipping, no fixed planning cadence, no structured estimation. James's role on Alina had been deliberately reduced to a "ride-along" (design review, user-story review, weekly pod standup, asking "is anybody blocked?") at Dr. C's direction — she chose engineer bandwidth over PM-driven accountability.

If you're taking over Alina, that ride-along framing is what the team expects. Whether you accept it, expand it, or renegotiate it is a conversation to have explicitly with Dr. C **before** you have it with Eshin — accountability without authority is the failure mode you want to avoid.

## Drip Campaigns is light-touch by design

Dr. C scoped Drip Campaigns as: **accountability + feedback collection**. Beta feedback flows in via the `dripcampaigns@sked.life` group inbox and Intercom. A weekly touch-base with André is the recommended cadence. There isn't much more to it — the feature is nearly done, V2 work is incremental, and André largely self-directs.

## What's missing across the board

- **No standardized estimation.** No story-point convention, no velocity tracking, no consistent "definition of done." Each pod handles this differently or not at all.
- **No QA team.** This is consistent with ChiroHD. No formalized automated testing infrastructure either.
- **No AI eval framework.** Outside the ARIA pod, no part of the org has a working eval harness. Alina ships without one. This is a known organization-wide gap (see [[SKED Tooling Map]]).
- **No formal beta-testing framework.** Beta cohorts exist for Alina (~100 clinics) and Drip (~10–12 testers), but the structure for measuring beta success and gating GA is informal.
- **No designer post-departure.** James Hendrie was the sole SKED designer and there is no replacement queued up. Plan around it.

## Handoff context (current as of 2026-05-19)

The first SKED pod is being seeded specifically because James Hendrie is departing 2026-05-29 and a new PM (initially Jeff Williams) is absorbing Alina + Drip Campaigns alongside their existing ChiroHD-side pods. This is **interim coverage**, not a long-term staffing plan — Dr. C's framing is that AI-augmented productivity makes one-PM-many-pods workable, but the model has not been stress-tested at SKED scale. If you're the second or third SKED PM reading this, the situation has presumably evolved; verify the current org chart with Dr. C before assuming any of the above still holds.

Related: [[SKED Tooling Map]] · [[SKED Stakeholder Map]] · [[SKED Glossary]]
