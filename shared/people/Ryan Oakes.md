---
type: person
name: Ryan Oakes
role: ""
team: ""
department: ""
reports_to: ""
relationship_type: ""
communication_style: ""
email: roakes@chirohd.com
last_reviewed: 2026-03-08
auto_created: true
tags:
  - type/person
  - people/RyanOakes
  - status/stub
aliases:
  - Ryan Oakes
  - roakes
---

## Profile %% fold %%

**Role:** 
**Team:** 
**Reports to:** 
**Relationship:** 

## Key Context %% fold %%

- 

## Communication Preferences %% fold %%

- 

## Working Notes %% fold %%
<!-- Private observations about working with this person -->
- 

## Recent Updates %% fold %%
<!-- UPDATES_START -->

*Recent updates: empty — populated by ingest as the pod observes this person.*

<!-- UPDATES_END -->

---

## History %% fold %%

### 1-on-1 Meetings %% fold %%
```dataview
TABLE WITHOUT ID file.link as Meeting, date as Date
FROM "Meetings/1-on-1s" AND #people/RyanOakes
SORT date DESC
LIMIT 10
```

### Other Meetings %% fold %%
```dataview
TABLE WITHOUT ID file.link as Meeting, date as Date, meeting_type as Type
FROM "Meetings" AND #people/RyanOakes
WHERE meeting_type != "1-on-1"
SORT date DESC
LIMIT 12
```

### Open Action Items %% fold %%
```dataview
TASK FROM "Meetings"
WHERE contains(tags, "people/RyanOakes") AND !completed
SORT file.name DESC
LIMIT 10
```

### All References %% fold %%
```dataview
LIST FROM #people/RyanOakes
WHERE type != "meeting" AND type != "person"
SORT file.mtime DESC
LIMIT 10
```
