---
type: person
name: "Luke Doty"
role: "President"
team: "Executive"
department: "Leadership"
reports_to: ""
relationship_type: executive
communication_style: ""
last_reviewed: 2026-02-26
tags:
  - type/person
  - people/LukeDoty
aliases:
  - "LukeDoty"
  - "Luke"
---

## Profile %% fold %%

**Role:** President
**Team:** Executive
**Reports to:** Board
**Relationship:** Executive

## Key Context %% fold %%

- **President of ChiroHD** — co-founded with brother Gabe, part of the family leadership core
- Previously worked at Amobi under Ryan Caskey ("Luke actually worked for me at Amobi. Now he's like president here")
- [[Kathryn Lucero]]'s sister used to be married to Luke; Kathryn grew up with him and Gabe
- No direct interaction during hiring process
- Likely handles day-to-day business operations while Gabe focuses on strategy
- Understanding the Gabe/Luke dynamic is important for navigating leadership
- May have different perspectives on process and structure than Gabe

## Personal & Background %% fold %%

- Based in Greenville, South Carolina
- UNC (University of North Carolina) fan — shares that with Jon Belmonte
- Has children — references "I don't know what the world's going to look like for my children" about AI changes
- Was skeptical about AI but tone changed drastically in the past 4 months before the March All Hands
- Emphasizes the team: "The thing that comes up over and over is really just how special this team is"
- Encourages experimentation: "the best thing everybody can do is just lean in and experiment"
- Casual about salary discussions — "for him, it was like no big deal" when negotiating {{ENGINEER_NAME}}'s salary

## Communication Preferences %% fold %%

- TBD — observe early and adapt
- Understand his operational priorities before engaging deeply

## Working Notes %% fold %%
<!-- Private observations about working with this person -->
- Learn early: what's his scope vs Gabe's scope?
- What operational pain points does he care about?
- How does he view the PM function — as supporting operations or driving product?
- Build this relationship independently from the Gabe connection

## Recent Updates %% fold %%
<!-- UPDATES_START -->

*Recent updates: empty — populated by ingest as the pod observes this person.*

<!-- UPDATES_END -->

---

## History %% fold %%

### 1-on-1 Meetings %% fold %%
```dataview
TABLE WITHOUT ID file.link as Meeting, date as Date
FROM "Meetings/1-on-1s" AND #people/LukeDoty
SORT date DESC
LIMIT 10
```

### Other Meetings %% fold %%
```dataview
TABLE WITHOUT ID file.link as Meeting, date as Date, meeting_type as Type
FROM "Meetings" AND #people/LukeDoty
WHERE meeting_type != "1-on-1"
SORT date DESC
LIMIT 12
```

### Open Action Items %% fold %%
```dataview
TASK FROM "Meetings"
WHERE contains(tags, "people/LukeDoty") AND !completed
SORT file.name DESC
LIMIT 10
```

### All References %% fold %%
```dataview
LIST FROM #people/LukeDoty
WHERE type != "meeting" AND type != "person"
SORT file.mtime DESC
LIMIT 10
```
