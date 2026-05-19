---
name: feedback-ingest-transcripts
description: "Every meeting ingest must produce a redacted source transcript in raw/ alongside the meeting note — do not skip Step 0/1b of the ingest skill, even if a meeting note has already been created."
metadata: 
  node_type: memory
  type: feedback
  originSessionId: fe3b3c3b-44fa-45f6-946e-a84d68c71ade
---

Every meeting ingest must produce BOTH a meeting note in `projects/<project>/meetings/` AND a redacted source transcript in `projects/<project>/raw/`. The meeting note's frontmatter must include a `source_transcript:` field, and the body should open with a `[[...]]` wikilink to the redacted transcript.

**Why:** The ingest skill (`.claude/skills/ingest/skill.md`) specifies in Step 0 that the raw transcript from {{PM_NAME}}'s personal vault (`Meetings/Transcripts/`) is the primary source for meeting content, and Step 1b requires redacting it and filing it in `raw/`. Multiple prior ingest sessions in May 2026 skipped these steps — meeting notes for 2026-04-28 (Dr. C 1:1), 2026-05-11 (PJ Pod Working Session + Session 2 + Dr. C Demo) all landed in `meetings/` without their underlying transcripts. {{PM_NAME}} discovered the gap on 2026-05-15 and asked Claude to backfill. Without the transcripts, meeting notes have no source-of-truth to verify against if a claim is later questioned, and the Fathom AI summaries (which are explicitly NOT source-of-truth per the skill) become the only fallback.

**How to apply:**
- Before considering any meeting ingest complete, verify the redacted transcript file exists in `projects/<project>/raw/` and the meeting note frontmatter has `source_transcript:` populated.
- If the personal-vault transcript can't be found, ask the user — do NOT continue with AI-generated summaries as a substitute. The user may have an in-person/unrecorded session (e.g., 2026-05-13 PJ Pod Working Session had no Fathom recording because {{ENGINEER_NAME}} worked solo that morning), in which case flag this explicitly in the meeting note body rather than silently proceeding.
- Redaction rules from the skill: replace personal/off-topic blocks with `[redacted — <brief description>]`, group adjacent off-topic lines into a single marker, keep ALL project-relevant content (including blunt or critical assessments of organizational dynamics that affect the project).
- The Fathom AI Summary can be appended at the bottom of the redacted file labeled as "AI-Generated Summary (for reference only, not source-of-truth)" — it's a useful reference but never the ground truth.

Related: [[user-vault-locations]] if it gets written — {{PM_NAME}}'s personal ChiroHD vault is at `/Users/jeff/Library/Mobile Documents/iCloud~md~obsidian/Documents/ChiroHD/` and transcripts live in `Meetings/Transcripts/` with filenames like `YYYY-MM-DD <Meeting Title> (HH-MM AM/PM) transcript.txt`.
