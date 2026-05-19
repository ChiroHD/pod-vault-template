---
type: person
name: Cristina Esposito
role: CPO / VP of Product
team: Product
department: Product
reports_to: ""
relationship_type: manager
communication_style: ""
last_reviewed: 2026-02-26
tags:
  - type/person
  - people/CristinaEsposito
aliases:
  - CristinaEsposito
  - Dr. Esposito
  - Dr. C
  - Dr. Cristina Esposito
---

## Profile %% fold %%

**Role:** CPO / VP of Product
**Team:** Product
**Reports to:** CEO / Board
**Relationship:** Manager ({{PM_NAME}}'s direct manager)

## Key Context %% fold %%

- **{{PM_NAME}}'s direct manager** — most important relationship to get right
- Background: Naval Academy graduate + Doctor of Chiropractic — unique combination of discipline and domain expertise
- Former naval officer — served 5 years in the Navy, stationed in San Diego
- Became a chiropractor after the military; practiced for ~8 years
- Worked with her older brother (also a chiropractor) — originally bought one of his practices
- Started Align Life (chiropractic franchise), a supplement company, and an EHR software company simultaneously
- Was CEO of Align Life; ran several franchise locations (Network 4 at ChiroHD — their second big franchise)
- Sold the EHR software company ~5 years ago, which led her to ChiroHD as a customer
- Consulted for ChiroHD for ~8 years as product advisor before joining full-time as Director of Product in November 2025
- No formal product management background — chiropractor/clinic owner running the product team
- Owns clinics in Fishers, Indiana and Beech Grove, Indiana
- Joined ChiroHD ~3 months before {{PM_NAME}} (around Dec 2025) — she's also relatively new
- Bringing clinical credibility that the product org previously lacked
- She needs a **PM partner, not a competitor** — frame yourself as making her successful
- Currently a **single point of failure** for product validation — {{PM_NAME}} taking on work relieves pressure
- Likely still establishing her own authority and processes

## Personal & Background %% fold %%

- Originally from New Jersey; lived in Southern California for many years
- Currently lives near Bloomington/Decatur, Illinois (flies out of Bloomington airport — "four gates")
- Looking to buy a house in Charleston, South Carolina to get back to the coast
- Has at least one child (a child appeared during the March 13 call during spring break)
- Has a boyfriend/partner (per {{ENGINEER_NAME}})
- Has a cat (mentioned reacting to wind during a storm)
- Self-described as having ADHD tendencies — "I'll feel the need to get something perfect before I roll it out... but then I'll hit ADHD and want to do something different"
- Described as "a powerhouse" by Kathryn Lucero
- Jumps straight into business in meetings with minimal personal talk
- "True allergy to inefficiency" — her own words

## Communication Preferences %% fold %%

- Loves to write detailed requirements
- Very opinionated and doesn't budge easily — but can see reason and will eventually listen
- Extremely busy; sometimes doesn't respond to messages or review shared documents in a timely manner
- Military background (Navy) — not afraid to work hard or ask hard questions publicly
- Given Naval background, may value structured communication and clear accountability
- Clinical background means she'll appreciate when product decisions reference patient outcomes

## Working Notes %% fold %%
<!-- Private observations about working with this person -->
- Priority 1 relationship — invest heavily in 1:1s during first 2 weeks
- Ask early: "What does success look like for me in your eyes at 30/60/90 days?"
- Understand her vision for the product org before proposing your own ideas
- Find out what's on her plate that she'd love to hand off — that's your quick win
- Watch for dynamics between her and Gabe (founder) — she may be navigating founder tensions too

## Recent Updates %% fold %%
<!-- UPDATES_START -->

*Recent updates: empty — populated by ingest as the pod observes this person.*

<!-- UPDATES_END -->

---

## History %% fold %%

### 1-on-1 Meetings %% fold %%
```dataview
TABLE WITHOUT ID file.link as Meeting, date as Date
FROM "Meetings/1-on-1s" AND #people/CristinaEsposito
SORT date DESC
LIMIT 10
```

### Other Meetings %% fold %%
```dataview
TABLE WITHOUT ID file.link as Meeting, date as Date, meeting_type as Type
FROM "Meetings" AND #people/CristinaEsposito
WHERE meeting_type != "1-on-1"
SORT date DESC
LIMIT 12
```

### Open Action Items %% fold %%
```dataview
TASK FROM "Meetings"
WHERE contains(tags, "people/CristinaEsposito") AND !completed
SORT file.name DESC
LIMIT 10
```

### All References %% fold %%
```dataview
LIST FROM #people/CristinaEsposito
WHERE type != "meeting" AND type != "person"
SORT file.mtime DESC
LIMIT 10
```
