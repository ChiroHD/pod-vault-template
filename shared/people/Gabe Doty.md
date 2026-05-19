---
type: person
name: Gabriel Doty
role: Chief Strategist (formerly CEO)
team: Executive
department: Leadership
reports_to: ""
relationship_type: executive
communication_style: Direct, vision-oriented, values authentic relationship-building
email: "gdoty@chirohd.com"
last_reviewed: 2026-04-13
tags:
  - type/person
  - people/GabeDoty
aliases:
  - GabrielDoty
  - Gabriel Doty
  - Gabe
  - GabeDoty
---

## Contact Info %% fold %%

- Email: gdoty@chirohd.com
- LinkedIn: https://www.linkedin.com/in/gabrieldoty/
- Website: www.chirohd.com

## Profile %% fold %%

**Role:** Chief Strategist (formerly CEO)
**Team:** Executive
**Reports to:** Board / Mainsail Partners
**Relationship:** Executive

## Key Context %% fold %%

- **Founder** of ChiroHD — co-founded with brother Luke; built it from scratch, deeply emotionally invested
- Previously worked at I Wonder, then Amobi
- Describes himself as "the visionary that works 150 hours a week if he has to"
- Has a semi-permanent board seat — even in an exit, very little likelihood he'd be completely out
- Helped {{ENGINEER_NAME}} transition from support to dev at Amobi
- Has been through two PE companies: Ridge Peak, now Mainsail Partners
- Recently transitioned from CEO to Chief Strategist as new CEO (Jon Belmonte) is brought in
- This transition means he's navigating a new identity — be sensitive to founder dynamics
- **Values innovation preservation** — wants new ideas to keep flowing, not get buried by process
- **Process-averse** — historically resistant to heavy frameworks; prefers agility
- Defines PM success as: "a reliable roadmap and things shipping on time"
- Has been doing product work himself — bringing a PM means he's giving up control
- Likely needs to see early wins to build trust in the PM function
- ~75% of ChiroHD clinics are one-doctor clinics (from his deep analysis)
- Philip Maker was one of his former bosses — Gabe called him "best boss I ever had, kindest human"
- Known by [[Kathryn Lucero]] her entire life; she grew up with him and Luke

## Personal & Background %% fold %%

- Has a baby/young child (mentioned at All Hands baby show-and-tell)
- Self-described as "a betta fish" — gets an idea and dives headfirst; nickname from ~15 years ago
- Self-described as not the most organized person
- Started vibe-coding AI projects on weekends; was able to do "10 times more coding than he's ever been able to do in his life" using AI tools
- Known for being emotional — someone joked "I'm crying so Gabe doesn't have to" at the All Hands
- Humble — {{ENGINEER_NAME}} says his humility is genuine and contributes to minimal turnover
- Uses metaphors well (e.g., "evil genie" to describe AI behavior)
- Opens presentations with quotes — used a Moneyball quote at the All Hands
- Passionate about company culture; sees preserving the spirit of the company as his #1 responsibility
- "Pretty open book" about company information

## Communication Preferences %% fold %%

- Prefers direct, honest conversations
- Values relationship-building — invest in the personal connection
- Likely responds well to showing respect for what he's built
- Frame changes as "building on the foundation" not "fixing what's broken"

## Working Notes %% fold %%
<!-- Private observations about working with this person -->
- Key risk: if he feels the PM is adding bureaucracy, trust erodes fast
- Key opportunity: if he sees the PM protecting his vision while adding structure, becomes strongest ally
- Learn his product vision deeply before proposing any process changes
- He and Luke (Gabe’s brother, President) are the family leadership core

## Recent Updates %% fold %%
<!-- UPDATES_START -->

*Recent updates: empty — populated by ingest as the pod observes this person.*

<!-- UPDATES_END -->

---

## History %% fold %%

### 1-on-1 Meetings %% fold %%
```dataview
TABLE WITHOUT ID file.link as Meeting, date as Date
FROM "Meetings/1-on-1s" AND #people/GabrielDoty
SORT date DESC
LIMIT 10
```

### Other Meetings %% fold %%
```dataview
TABLE WITHOUT ID file.link as Meeting, date as Date, meeting_type as Type
FROM "Meetings" AND #people/GabrielDoty
WHERE meeting_type != "1-on-1"
SORT date DESC
LIMIT 12
```

### Open Action Items %% fold %%
```dataview
TASK FROM "Meetings"
WHERE contains(tags, "people/GabrielDoty") AND !completed
SORT file.name DESC
LIMIT 10
```

### All References %% fold %%
```dataview
LIST FROM #people/GabrielDoty
WHERE type != "meeting" AND type != "person"
SORT file.mtime DESC
LIMIT 10
```

## Pre-Hire Interaction Log %% fold %%

> Migrated from Job-Hunt-2025 vault. {{PM_NAME}}'s hiring journey with ChiroHD, Oct 2025 - Feb 2026.

- **2025-10-29** — {{ENGINEER_NAME}} first told {{PM_NAME}} about the product leader opportunity at ChiroHD
- **2025-11-05** — {{ENGINEER_NAME}} shared Gabe's contact info (gdoty@chirohd.com)
- **2025-11-06** — {{PM_NAME}} emailed Gabe (drafted with Claude). {{ENGINEER_NAME}} mentioned {{PM_NAME}} in his meeting with Gabe that day. Gabe confirmed he'd reach out.
- **2025-11-18** — {{PM_NAME}} sent follow-up email to Gabe
- **2026-01-10** — Allison Gambone (Gabe's assistant) emailed with times to meet. Scheduled 1/21 at 10:30am ET.
- **2026-01-21** — [[2026-01-21 Gabe Doty - Interview]] — First meeting. Very strong values-alignment conversation. Gabe impressed, already socializing {{PM_NAME}} with Dr. C and Mainsail. Key: positioned as force multiplier for Dr. C. Only friction point: PE experience (Gabe pushing past it).
- **2026-01-22** — Follow-up email to Gabe thanking him, expressing excitement about connecting with Dr. C and Bashir
- **2026-01-30** — Gabe told {{ENGINEER_NAME}} (unprompted in a 1-on-1) he's "extremely impressed" with {{PM_NAME}} and "pushing as hard as he can with Mainsail"
- **2026-02-03** — Bashir interview completed. Bashir gave "three thumbs up." {{ENGINEER_NAME}}: "The chances of it not working out now are super small."
- **2026-02-04** — Dr. C sent verbal offer email
- **2026-02-11** — Offer negotiation discussion with {{ENGINEER_NAME}} and Amy
- **2026-02-28** — Offer accepted: $220K total comp guarantee ($180K base + $18K guaranteed bonus + equity)
