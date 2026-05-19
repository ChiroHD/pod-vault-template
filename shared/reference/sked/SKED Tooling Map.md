---
type: reference
product_line: sked
created: 2026-05-19
tags:
  - type/reference
  - product_line/sked
---

# SKED Tooling Map

Where things live on the SKED side. SKED's stack diverges meaningfully from ChiroHD's — GitLab in place of JIRA, Notion + Google Docs for requirements, Intercom as the wiki. Treat this as the orientation cheat sheet for your first week.

## Tool by concern

| Concern | Tool (SKED) | Notes |
| --- | --- | --- |
| Engineering tickets | **GitLab Issues** | Source of truth for everything code-adjacent: bug tickets from support, new feature builds, code review, and lower-environment management. Multiple repos exist (SKED admin, the app, dev side-projects). Leadership has floated a Jira migration; engineers are resistant because they previously used Jira and chose to switch off it. **Alina is a hybrid:** Eshin Griffith works from JIRA personally and mirrors to GitLab as a courtesy to the rest of SKED, so the Alina pod has tickets in both places. Expect to keep one foot in each. |
| Code hosting | **GitLab** | All SKED repos. Lower dev environments (~6–10 of them) are managed via a GitLab build pipeline; environments are turned off daily and rebuilt on demand. Don't learn the pipeline yourself — ask a developer to spin one up when you need it. |
| Requirements docs | **Notion "Builds" + Google Docs (legacy)** | Notion is the home for requirements docs from roughly the last six months; older work lives in Google Docs. Both are linked from the originating GitLab ticket. Notion is under the **SKED product** team space, "Builds" section. There is some legacy clutter from an abandoned tagging initiative but it's searchable. |
| Help center / wiki | **Intercom** | Doubles as customer-facing help center and de facto internal wiki. James Hendrie's candid assessment: "reasonably low quality," missing the "why," substantive questions still require asking a person. A redoc project got ~25% done and stalled. Treat Intercom as a pointer, not a source of truth. |
| Training | **Vimeo** | The SKED training video library. James and Bex confirmed **~95% of videos are out of date.** Articles in Intercom are mostly current; videos are not. Bex and Ellen are exploring AI-generated video refresh. Until then, don't rely on videos for ramp-up. |
| Roadmap | **Monday.com** at `skedlife.monday.com` | A separate board from the ChiroHD Monday workspace. Dr. C runs a unified roadmapping process across both product lines: each team submits priorities, cross-team discussion happens, Dr. C makes the final call. The boards are still evolving — expect them to keep shifting. |
| Beta feedback | **Intercom + group emails** | Beta feedback for in-flight features flows through Intercom messages and a per-feature group email (e.g., `dripcampaigns@sked.life`). No structured beta feedback platform — the PM is expected to monitor the inbox and triage. |
| Design | **Figma** | James Hendrie has been the sole designer for SKED and maintained a Figma-based design system (tokens, typography, iconography, appointment-type selector). The codebase has a partial component library under `/new-ui` that mirrors some of it (~50% complete). After James's departure (last day 2026-05-29) there is **no designer** on SKED or ChiroHD. Plan accordingly. |
| AI evaluations | **None** | There is no AI eval framework anywhere in the org outside the ARIA pod. Alina (the AI scheduling assistant) ships without a formal eval harness. This is a known organization-wide gap — flagged here so the first SKED pod knows it's an open problem, not an oversight. |

## Cross-references — how to navigate from one tool to another

The most useful mental model is **three-way linking**:

```
GitLab ticket  ←→  Notion "Builds" doc (or Google Doc if older than ~6mo)  ←→  Monday.com roadmap record
```

- Start from the GitLab ticket. It's the index — James confirmed it would be rare to find a meaningful Notion doc that isn't linked from a GitLab ticket.
- The ticket links forward to the requirements doc (Notion for recent, Google Drive for older) and back to the Monday.com line item if it's on the roadmap.
- If you can't find a doc from GitLab, it probably doesn't exist in a reconciled form. The Alina requirements are notably uneven this way: a consolidated Notion doc covers some things, but Eshin's JIRA-side requirements have drifted from it.

For Alina specifically: also check **JIRA** (Eshin's personal workflow) because Alina-pod tickets exist in both systems.

## Differences from ChiroHD

> [!note] If you're coming from a ChiroHD pod
> - **JIRA → GitLab Issues.** Same shape, different tool. The major ergonomic shift is that GitLab issues feel more code-centric.
> - **Confluence/Notion → Notion "Builds" + Google Docs.** SKED uses Notion for recent work and Google Docs for anything older than ~6 months. ChiroHD's Notion is "a huge mess" by Jeff's own assessment; SKED's is comparatively orderly but has its own legacy quirks.
> - **No standardized help center → Intercom.** Quality is uneven; do not assume Intercom is authoritative.
> - **Monday boards are separate.** ChiroHD's roadmap and `skedlife.monday.com` are two different workspaces, even though Dr. C runs the same prioritization process across both.
> - **No AI eval tooling.** ChiroHD's ARIA pod has evals; SKED does not. If you're working on an AI feature on the SKED side, this is the gap you'll feel first.

Related: [[SKED Stakeholder Map]] · [[SKED Process Notes]] · [[SKED Glossary]]
