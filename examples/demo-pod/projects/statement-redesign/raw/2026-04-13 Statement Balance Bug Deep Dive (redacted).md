---
type: raw-transcript
date: 2026-04-13
source: fathom
project: statement-redesign
redacted: true
parent_meeting: "meetings/2026-04-13 Statement Balance Bug Deep Dive.md"
tags:
  - raw
  - transcript
  - redacted
---

# 2026-04-13 Statement Balance Bug Deep Dive (redacted)

Source transcript for [[2026-04-13 Statement Balance Bug Deep Dive]]. Redacted of off-topic discussion, personal info, and pre-meeting small talk.

---

**Riley (00:02):** Okay, let me share my screen. I pulled the Q1 support tickets — there are 47 of them in the "statement balance doesn't match ledger" theme. Spread across 23 practices. Some are duplicates from the same practice.

**Sam (00:18):** Right. And the ones I've poked at, the practice always says "the ledger looks right but the statement is showing some other number." Yeah?

**Riley (00:28):** Yeah. Always the statement is wrong, never the ledger.

**Sam (00:34):** Which is interesting because the statement is supposed to be just reading `cached_balance` off `patient_account`. The ledger is the same source of truth either way. So if the statement is wrong, the cache is wrong.

**Riley (00:52):** That's what I was assuming. Did you confirm?

**Sam (00:55):** I pulled production logs for three of the affected patients. Let me walk you through one.

[redacted — Sam screen-shares production logs with patient identifiers visible. Walks through the sequence for one patient across 18 ledger entries over 14 months. Identifies four points where the trigger appears to have committed without the corresponding ledger entry committing.]

**Sam (12:40):** So you see here — at this timestamp the ledger entry insert failed on a unique constraint, application logged the failure and retried. The retry succeeded. But the trigger had already incremented `cached_balance` by the original attempt's amount, and there's no logic to back that out.

**Riley (13:08):** Wait. The trigger commits in its own connection?

**Sam (13:12):** Yeah. The trigger is on `ledger_entries` and it updates `patient_account.cached_balance`. Those are technically the same transaction at the SQL level, but the way the application handles the retry, the connection state gets fuzzy. I traced it for a while and I think there's a code path where the failure handler doesn't roll back the trigger-side write. It's been like this for years.

**Riley (13:55):** So the drift accumulates every time there's a failed-and-retried ledger write.

**Sam (14:02):** Right. And we don't notice because the cached value is "close enough" to the right value most of the time. The drift is in the noise of normal operations until it isn't.

**Riley (14:18):** Okay so. Options.

**Sam (14:22):** Three I can think of. One: periodic reconciliation job. Sweep the ledger, rewrite the cache. That fixes the drift but doesn't fix the cause — between sweeps the statement is still wrong.

**Riley (14:40):** What's the sweep window?

**Sam (14:42):** Could be nightly. Could be hourly. But even hourly, if a statement gets generated 30 minutes after a failed-retry sequence, the statement is still wrong.

**Riley (14:54):** Okay. Option two?

**Sam (14:57):** Fix the trigger so it's properly transactional. Possible, but the trigger fires on every ledger write — every charge, every payment, every write-off, every credit. Across every practice. The blast radius for a regression is enormous.

**Riley (15:18):** Right. Not for V1.

**Sam (15:22):** Option three is what I think we should do. Recalc balance live at statement render time. We build a service. It sums the ledger for the patient. The render endpoint calls it instead of reading `cached_balance`. The cache stays — it's still used by other features — but it's not the source of truth for statements anymore.

**Riley (15:48):** Performance.

**Sam (15:50):** Let me run a quick test.

[redacted — Sam runs a bench against the longest-history dev patient. Reports 380ms render-to-PDF on a patient with 4,800 ledger entries.]

**Sam (19:30):** Under a second by a comfortable margin. And the query is bounded — single patient, indexed on `(patient_id, posted_at)`.

**Riley (19:42):** What if a practice doubles their statement volume?

**Sam (19:45):** Statements are user-initiated. The render volume is bounded by staff clicking "send statement." It's not a batch job. Even if a practice doubles, we're not running 4,000 statements a second. And if it ever gets tight, we add an index, not a cache.

**Riley (20:08):** Okay. Recalc on render.

**Sam (20:11):** Recalc on render.

[redacted — discussion of which other features read `cached_balance`. Case summary cards, patient list view, dashboard tile, four internal reports. None touched by V1. Decision: leave `cached_balance` updating as-is for backward compat.]

**Riley (28:30):** This is also going to need a decision record.

**Sam (28:33):** Yeah. Want me to draft it?

**Riley (28:36):** I'll do it. I'll backdate the file to the 8th since that's when I first wrote it up in my notes — I had the architecture choice in mind before this dive confirmed the root cause. But the dive is what makes it real.

[redacted — discussion of testing strategy for the new service. Sam plans 12-patient test set with 4 known-drift histories. Riley to write up the project-brief risk addition.]

[redacted — off-topic discussion of the Q2 roadmap unrelated to statements]

**Riley (1:34:10):** Okay. Action items?

**Sam (1:34:13):** STMT-101 for the service. STMT-102 for the wire-through. I'll have the service done by Friday and the wire-through done by next Tuesday. We'll be on the May 11 release for Phase 1.

**Riley (1:34:30):** Beautiful. Let's wrap.

---

[end of transcript]
