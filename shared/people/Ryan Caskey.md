---
type: person
name: "Ryan Caskey"
role: "Head Developer (ChiroHD)"
team: "Engineering"
department: "Engineering"
reports_to: ""
relationship_type: peer
communication_style: ""
email: "ryan@chirohd.com"
last_reviewed: 2026-03-08
auto_created: true
tags:
  - type/person
  - people/RyanCaskey
aliases:
  - "Ryan Caskey"
  - "Ryan"
---

## Profile %% fold %%

**Role:** Head Developer (ChiroHD)
**Team:** Engineering
**Reports to:**
**Relationship:** Peer (Engineering lead)

## Key Context %% fold %%

- **Head developer on the ChiroHD side** — key engineering counterpart for {{PM_NAME}}
- Has agreed to require limited release before any full rollout — fully on board with new QA process
- Works directly with Cristina on release process improvements
- Been at ChiroHD about 2.5 years as of March 2026
- Previously worked at I Wonder (with Gabe), then Amobi (as remote employees from Atlanta alongside Phillip Maker and Gabe), then LiveIntent (with {{ENGINEER_NAME}})
- Philip Maker was his wife's youth leader; Philip gave Ryan his first internship in software development
- Started a small business with Philip that didn't take off but was formative
- Previously taught computer science for 5 years at Cornerstone Prep (his kids' school); stopped to focus on ChiroHD — said "if I just won the lotto, that's what I would do" about teaching

## Personal & Background %% fold %%

- Lives north of Atlanta, Georgia; been in the metro area about 20 years
- Previously lived in New York before moving to Atlanta
- Met his wife in Ukraine on a mission trip; started long-distance (he in NY, she in Atlanta)
- Wife teaches kindergarten at Cornerstone Prep (university-model school); this is her last year — getting certified to help students with dyslexia
- Wife was in two car wrecks in 4 months on the road near their house (head-on with teenager, rear-ended by drunk driver) — both totaled her Nissan Armada; she went to hospital and chiropractor but was ultimately fine
- Two kids: Raven (14, into horseback riding — Ryan could see her becoming a vet) and Lawson (10, turning 11 in May, entrepreneurial spirit)
- Owns a double-lot property with a pond north of Atlanta
- Has a gun safe; does his own ammo reloading; has a steel target in the backyard
- Very involved at church; part of a Bible study for which he built a web application
- Big flight simulation enthusiast (Microsoft Flight Simulator 2024) — has rudder pedals, yoke, and throttle at his desk
- Helped with a mod called "Say Intentions" that replaces ATC with AI-based system requiring real phraseology; user #19 (early adopter). Designed a taxiway routing algorithm for it
- Has flown real planes (out of McCollum, a small towered airport) but no pilot's license yet due to time
- Building a Raspberry Pi automatic guitar tuner project with son Lawson using microphones and stepper motors
- Lego enthusiast — has NASA Saturn V set displayed; noticed piece count matches 1969 Apollo year
- Wears space-themed shirts (Space Shuttle shirt from 1981)
- Member of a country club where he met a pilot with a plane

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
FROM "Meetings/1-on-1s" AND #people/RyanCaskey
SORT date DESC
LIMIT 10
```

### Other Meetings %% fold %%
```dataview
TABLE WITHOUT ID file.link as Meeting, date as Date, meeting_type as Type
FROM "Meetings" AND #people/RyanCaskey
WHERE meeting_type != "1-on-1"
SORT date DESC
LIMIT 12
```

### Open Action Items %% fold %%
```dataview
TASK FROM "Meetings"
WHERE contains(tags, "people/RyanCaskey") AND !completed
SORT file.name DESC
LIMIT 10
```

### All References %% fold %%
```dataview
LIST FROM #people/RyanCaskey
WHERE type != "meeting" AND type != "person"
SORT file.mtime DESC
LIMIT 10
```
